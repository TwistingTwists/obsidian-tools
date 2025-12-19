---
title: "A Log Would Help, LLC"
source: "https://alogwouldhelp.com/"
author:
published:
created: 2025-12-19
description:
tags:
  - "clippings"
---
A Log Would Help, LLC is a small software consulting company based in Seattle, Washington. It's run by [Diego Ongaro](https://ongardie.net/), who has been writing code for over 20 years. Diego specializes in distributed systems and databases, and also has experience in security and operating systems. He holds a PhD in Computer Science from Stanford University and is best known for his work on the [Raft consensus algorithm](https://raft.github.io/).

## Would a log help?

Maybe! Many systems replicate data from a primary server to one or more backups using a *log*. The primary appends changes to the log as its state changes. A backup can catch up to recent changes from the primary efficiently by processing just the end of the log. Meanwhile, the primary can append new entries to the log by writing sequentially, which is also efficient. The entire state of a backup can be summarized by how many log entries it has processed.

[Raft](https://raft.github.io/) <sup>*</sup> uses a log in a similar way. It elects a new primary (called a leader) to write to the log whenever the previous primary becomes unavailable. This allows Raft-based systems to continue operating while any minority of servers in the cluster fail.

Logs are used in many other ways:

- [Journaling filesystems](https://en.wikipedia.org/wiki/Journaling_file_system) and [traditional databases](https://en.wikipedia.org/wiki/Transaction_log) (*transaction log* or *write-ahead log*) use logs as an auxiliary structure to preserve consistency upon crashes.
- [Log-structured filesystems](https://en.wikipedia.org/wiki/Log-structured_file_system) and [log-structured merge trees](https://en.wikipedia.org/wiki/Log-structured_merge-tree) use logs as a primary structure for high write throughput, as well as crash consistency.
- [Flash memory controllers](https://en.wikipedia.org/wiki/Flash_memory_controller) use logs to minimize wear caused by program-erase cycles, thereby extending device lifetimes.
- [RAMCloud](https://www.usenix.org/system/files/conference/fast14/fast14-paper_rumble.pdf) <sup>*</sup>, a low-latency in-memory storage system, uses logs to efficiently manage both objects in memory (with high utilization) and backup copies on disk (with high write throughput).
- [Kafka](https://kafka.apache.org/) and other event stores use logs to efficiently route and deliver asynchronous events from publishers to subscribers.
- [Certificate Transparency](https://en.wikipedia.org/wiki/Certificate_Transparency), a system for auditing digital certificates on the internet, combines public logs of certificates with [Merkle trees](https://en.wikipedia.org/wiki/Merkle_tree) to efficiently prove that a particular certificate is in the log and that one log is an extension of an earlier log.
- [Juicebox](https://github.com/juicebox-systems/juicebox-hsm-realm) <sup>*</sup>, a system for secret storage, combines a replicated log with a hash chain of log entries, allowing specialized security hardware ([HSMs](https://en.wikipedia.org/wiki/Hardware_security_module)) to verify the authenticity and freshness of a log that is stored on cheap cloud storage.

A log may not help in every situation. Despite its name, this consulting business can help with a variety of software systems—not just those involving logs.

\* indicates Diego is/was a core contributor to the project.

## Contact

If you're interested in learning more or exploring whether A Log Would Help, LLC would be a good fit for your needs, email `diego@`.