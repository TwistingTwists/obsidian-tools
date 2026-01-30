---
title: Viewstamped Replication in Go, Deterministic Testing at Dropbox, FoundationDB's Simulation, and more...
source: https://materializedview.io/p/viewstamped-replication-deterministic-simulation
author:
  - "[[Chris]]"
published: 2023-11-09
created: 2025-12-19
description: Viewstamped Replication makes a comeback, and I dig into distributed simulation testing with Dropbox, FoundationDB, and Tigerbeetle.
tags:
  - clippings
  - blog
  - sync-engines
---
### Viewstamped Replication makes a comeback; I dig into distributed simulation testing with Dropbox, FoundationDB, and Tigerbeetle.

## View Stamped Replication in Go

[Joran Dirk Greef](https://twitter.com/jorandirkgreef) (of [Tigerbeetle](https://tigerbeetle.com/) \[$\]) has me on the [Viewstamped Replication](https://dl.acm.org/doi/10.1145/62546.62549) band wagon. Viewstamped Replication is a consistency algorithm that predates [Paxos](https://en.wikipedia.org/wiki/Paxos_\(computer_science\)) and [Raft](https://raft.github.io/). See Joran’s [lovely talk](https://www.youtube.com/watch?v=qeWyc8G-lq4) for more details on it.

There seem to be relatively few actual implementations. I’ve only seen [Tigerbeetle’s Zig implementation](https://github.com/tigerbeetle/viewstamped-replication-made-famous/blob/main/src/vsr.zig), a [Rust library](https://github.com/penberg/vsr-rs), and some abandoned attempts in Go ([vrr-go](https://github.com/joshuabezaleel/vrr-go), [vrgo](https://github.com/BoolLi/vrgo)). So I am excited to see that [Utkarsh Srivastava](https://twitter.com/tangledbytes) is working on a new [Go implementation](https://github.com/tangledbytes/go-vsr).

![](https://substackcdn.com/image/fetch/$s_!a_b0!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe4f09030-2a4a-47ed-a2c2-f568b10c3704_692x238.png)

It’s a young project but he’s building in the open and it’s already an instructive learning tool.

## Deterministic simulation testing at Dropbox

Utkarsh’s comment on simulation testing led me to Dropbox’s [lengthy technical write-up](https://dropbox.tech/infrastructure/-testing-our-new-sync-engine) of their “sync engine” rewrite in 2020. Sync engine merges a user’s local and remote filesystem changes. States are modeled as file trees and a “sync tree” (similar to a [VCS](https://en.wikipedia.org/wiki/Version_control) diff) is generated to determine changes.

The post covers strategies to write deterministic and easily testable code.

> In Nucleus, we sought to make writing tests as ergonomic and predictable as possible. Nearly all of our code runs on a single “control” thread. Operations benefiting from concurrency (e.g. network I/O, filesystem I/O, and CPU-intensive work like hashing) are offloaded to dedicated threads or thread pools. When it comes to testing, we can serialize the entire system. Asynchronous requests can be serialized and run on the main thread instead of in the background. Say goodbye to flaky tests! This single-threaded design is the key to the determinism and reproducibility of our randomized testing systems.

Many of these ideas apply when testing consensus algorithms like Utkarsh’s Viewstamped Replication library, above.

*As an aside, Dropbox mentions using SQLite for client state. I’ve been [searching for more examples](https://materializedview.io/i/138463142/sqlite-for-blueskys-pds-server) of SQLite in large production deployments; this is definitely one.*

## Writing deterministic code

Continuing on the deterministic code theme, [Alex Kladov](https://github.com/matklad) ’s recent talk has been making the rounds. As the title suggests, TigerBeetle \[$\] uses similar techniques to Dropbox. The talk is a pithy 15 minute watch.

<iframe src="https://www.youtube-nocookie.com/embed/AGxAnkrhDGY?rel=0&amp;autoplay=0&amp;showinfo=0&amp;enablejsapi=0" frameborder="0" allow="autoplay; fullscreen" allowfullscreen="true" width="728" height="409"></iframe>

Alex recommends reducing variability in both space (IO) and time (clocks), and gives some pointers to do this. The claimed benefits are that deterministic code:

- Is easier to debug and test.
- Improves runtime predictability because resources are managed statically.
- Improves runtime throughput because, again resources are statically managed. (Especially if dynamic memory management goes away.)

## FoundationDB’s simulation testing

FoundationDB’s Simulation is a real-world example of simulation testing. [Will Wilson](https://www.linkedin.com/in/will-wilson-330276112/) talks about the framework in a 2014 [Strangeloop](https://www.thestrangeloop.com/index.html) (🪦) presentation.

<iframe src="https://www.youtube-nocookie.com/embed/4fFDFbi3toc?rel=0&amp;autoplay=0&amp;showinfo=0&amp;enablejsapi=0" frameborder="0" allow="autoplay; fullscreen" allowfullscreen="true" width="728" height="409"></iframe>

See the [write-up](https://apple.github.io/foundationdb/testing.html) on FoundationDB’s site for a more bite-sized introduction.

One interesting technique FoundationDB uses in Simulation is something called swizzle-clogging.

> To swizzle-clog, you first pick a random subset of nodes in the cluster. Then, you “clog” (stop) each of their network connections one by one over a few seconds. Finally, you unclog them in a random order, again one by one, until they are all up. This pattern seems to be particularly good at finding deep issues that only happen in the rarest real-world cases.

## Parting thoughts

The core take-away from all of these systems is to:

1. Stamp out non-determinism in your code.
2. Write a framework that runs with a pseudo-random number seed so you can reproduce failures.

This is harder than it sounds, hence all of this wonderful content.

## More Awesome Infrastructure

*Keep up with new projects on [awesome-infra](https://github.com/criccomini/awesome-infra) Github repo. New submissions are encouraged.*

[ReadySet](https://github.com/readysettech/readyset) \- A lightweight query cache that sits between an application and database turning SQL reads into lookups.

---

*I occasionally invest in infrastructure startups. Companies that I’ve invested in are marked with a \[$\] in this newsletter. See my [LinkedIn profile](https://www.linkedin.com/in/riccomini/) for a complete list.*