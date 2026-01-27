---
title: "Disks Lie: Building a WAL that actually survives"
source: https://blog.canoozie.net/disks-lie-building-a-wal-that-actually-survives/
author:
  - "[[Jeremy Tregunna]]"
published: 2025-12-12
created: 2026-01-27
description: A write-ahead log (WAL) is one of those database concepts that sounds deceptively simple. You write a record to disk before applying it to your in-memory state. If you crash, you replay the log and recover. Done.Except your disk is lying to you.PostgreSQL, SQLite, RocksDB, Cassandra... every production
tags:
  - clippings
  - database
---
A write-ahead log (WAL) is one of those database concepts that sounds deceptively simple. You write a record to disk before applying it to your in-memory state. If you crash, you replay the log and recover. Done.

Except your disk is lying to you.

PostgreSQL, SQLite, RocksDB, Cassandra... every production system that claims to be durable relies on a WAL. It's the fundamental contract: "Write here, and I promise your data survives." But making that promise actually stick requires understanding all the ways disk fail silently.

## The Naive Approach vs Reality

Let's say you implement a WAL like this:

```c
write(fd, record, sizeof(record)); // Done, right... RIGHT?
```

In a test environment on your laptop, this works great. But when you handle millions of writes a day, those 1 in a million errors happen multiple times a day. Some of these systems will fail in ways your tests never catch:

- **The page cache problem**: That `write()` just copied your data into the kernel's buffer. It hasn't touched the disk, yet. Crash now, and it's gone.
- **The disk that lies about success**: Your `write()` returns success. The kernel tells you it's synced. The disk firmware tells you it's on stable storage. Then a latent sector error silently corrupts it anyway.
- **The ordering chaos**: Write operation A starts. Write operation B starts. B completes first. Your recovery code sees B without A and has no idea what happened.
- **The single point of failure**: One bad sector on your only copy of the WAL, and you lose everything.

This is why people who've lost data in production are paranoid about durability. And rightfully so.

## Building the Better Mousetrap

There are 5 layers of defense that we can use to build a better mouse trap. Think of them as increasingly specific answers to the question: " ***How can this fail?***"

### Layer 1: Checksums (CRC32C)

Every record includes a checksum of its contents. After writing, we verify the checksum hasn't changed. Simple, right?

```c
Record Header (20 bytes):
    [magic_num: 4][sequnce_num: 8][checksum: 4]
    [payload: variable]
    [padding to 512 byte alignment]
```

Why this matters: Hardware bit flips happen. Disk firmware corrupts data. Memory busses misbehave. And here's the kicker: None of these trigger an error flag. The I/O subsystem returns success. The data is just silently wrong. Without checksums, you discover this weeks later when you try and recover and find your log is garbage.

### Layer 2: Dual WAL Files (LSE Protection)

Another solid strategy to help protect against a specific kind of failure: a latent sector error (LSE), is to keep two WAL files, ideally on different disks.

An LSE is when a disk sector fails silently. Your `write()` returns success. `fsync()` returns success. The data is literally not there, or it's corrupted. But the disk won't tell you. It just returns stale data or silently drops the write. Both are very bad options.

Google's study of their production fleet found that LSEs occur regularly enough that having a single copy of critical data is negligence, not caution. [Other studies](https://dl.acm.org/doi/abs/10.1145/1837915.1837917?ref=blog.canoozie.net) have shown consumer and enterprise drives fail at different rates, but still higher than you'd expect. You write the same record to two files. If verification detects corruption in one, the other still has your back.

```c
WAL Primary        WAL Secondary
    [Record 1]         [Record 1]
    [Record 2]         [Record 2]
    [Record 3]         [Record 3]
    ...                ...
```

### Layer 3: O\_DIRECT + O\_DSYNC

This is about being honest with yourself. When you call `write()`, you're lying to yourself about what just happened if you don't use these flags.

- **O\_DIRECT**: Bypasses the kernel's page cache. Stop pretending the kernel is doing you a favor by buffering. When you need durability, you don't want buffering. You want the data on disk. **Caution: Not all filesystems in Linux support O\_DIRECT**.
- **O\_DSYNC**: Synchronous writes. Don't return from `write()` until the data is actually stable on the disk.

Together, they mean: "I know this is slower. I also know I actually care about durability."

This matters because the kernel's page cache is a performance optimization for read-heavy workloads. For a WAL, it's a liability. Data sits in RAM indefinitely. A crash wipes it out. Applications that skip this are the ones discovering in production that they're not actually durable.

### Layer 4: Linked I/O Ordering (io\_uring in Linux)

For the purposes of this post, I'm focusing on Linux where io\_uring is a modern asynchronous I/O implementation for Linux. At its core, io\_uring uses two ring buffers: a **submission queue** (where applications submit I/O requests) and a **completion queue** (where the kernel returns results). This design reduces context switches and system call overhead, enabling applications to handle thousands of I/O operations with minimal CPU usage.

Other operating systems have similar purposed modern async I/O tooling, but that's left as an exercise for the reader.

In our case, we're going to leverage Linux's io\_uring with operation linking. Here's the sequence we'll focus on:

1. Submit the write to the primary file
2. Link fsync to that write (`IOSQE_IO_LINK`)
3. The fsync's completion queue entry only arrives after the write completes
4. Repeat for secondary file

This creates an ordering guarantee without context switches. Both writes complete before we return control to the application. No race conditions. No reordering.

```c
Submit:    [Write Primary] -> [Fsync Primary] -> [Write Secondary] -> [Fsync Secondary]
           (each operation on the primary/secondary are linked to the other)

Completes: Fsync Primary waits for Write Primary
           Write Secondary waits for both Write & Fsync Primary
           Fsync Secondary waits for Write Secondary
```

This matters because without explicit ordering, the kernel I/O scheduler might reorder your operations. Your recovery code assumes A completed before B, but B actually completed first. Your durability guarantees evaporate into thin air.

### Layer 5: Post-fsync Verification Reads

After both fsyncs complete, we should re-read the exact data we just wrote and verify its checksum. This detects latent sector errors immediately, while the data is fresh and before we consider it durable.

If verification fails, we have the secondary WAL on standby.

This matters because its our insurance policy. Writes can appear to succeed and still be silently corrupted. Only by reading back and validating do you catch these failures in time to fall back to redundancy. It's also why the dual WAL is necessary and not overkill.

## Recovery: All the King's Horses, and all the King's Men...

Humpty Dumpty may be screwed, but our data isn't.

When the system restarts:

1. Scan both WAL files sequentially, collecting all records with valid checksums
2. Merge records, handling duplicates (primary and secondary logged the same operations)
3. Find the highest contiguous sequence of valid records; this is your recovery point
4. Replay operations in order to reconstruct the in-memory state

This guarantees that:

- All durably-committed operations are replayed
- No uncommitted operations slip through
- You recover to a consistent point even if one WAL file is partially corrupted

## Why Any of This Matters?

You might think this is overkill. You might be right, for your use case. In a hobby project, you probably don't need all 5 layers. But consider these two scenarios from real production systems:

**Scenario 1: The Silent Corruption**

- Your WAL write completes
- 48 hours later, a latent sector error occurs
- You don't know. The disk doesn't tell you. The checksum fails only when you try to recover
- Without dual WALs: Data is lost. The application violated its durability promise
- With dual WALs + verification: The secondary WAL detects the corruption immediately. Recovery proceeds normally. No data loss.

**Scenario 2: The Page Cache Surprise**

- Your application writes to the WAL
- The kernel returns success
- The data sits in the page cache. Seconds pass. Minutes pass.
- A kernel crash occurs (driver bug, OOM handler, whatever)
- All that "written" data was never actually synced to disk
- Without O\_DIRECT: Data is lost. The application thought it was durable
- With O\_DIRECT: The data never sits in RAM. It's on disk the moment fsync returns.

These aren't theoretical failures. They've happened to production systems. Some of them learned the hard way. Others read about it and built their systems properly.

## Conclusion

A production-grade WAL isn't just code, it's a contract. The contract is: "Write this record, and I will guarantee it survives any single hardware failure." Fulfilling that contract requires:

- **Checksums** to detect corruption
- **Redundancy** to survive component failure
- **O\_DIRECT + O\_DSYNC** to ensure data physically reaches storage
- **Operation linking** to maintain write ordering without losing concurrency
- **Verification reads** to catch silent failures before they matter

Build the contract *honestly*. Disks ***lie***. Page caches ***lie***. Firmware has ***bugs***. And disk sectors ***fail silentl*** y (or act ***maliciously***). A proper WAL doesn't **trust** any of them.

The code that demonstrates this can be found [on GitHub](https://github.com/jeremytregunna/wal?ref=blog.canoozie.net).