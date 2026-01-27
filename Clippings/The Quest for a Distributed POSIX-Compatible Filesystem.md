---
title: "The Quest for a Distributed POSIX-Compatible Filesystem"
source: "https://materializedview.io/p/the-quest-for-a-distributed-posix-fs"
author:
  - "[[Chris]]"
published: 2024-12-06
created: 2026-01-27
description: "Distributed POSIX filesystems have proven elusive, but we're getting closer. Perhaps that's all we need."
tags:
  - "clippings"
---
### Distributed POSIX filesystems have proven elusive, but we're getting closer. Perhaps that's all we need.

Years ago, I was working on [Apache Samza](https://samza.apache.org/) with [Jay Kreps](https://www.linkedin.com/in/jaykreps/). At one point during a discussion about Samza’s state management system, Jay turned to me and said, “You know, we wouldn’t need any of this if we had a distributed filesystem that worked.” A scalable remote filesystem with normal [POSIX](https://en.wikipedia.org/wiki/POSIX) semantics would let us build distributed systems that were stateless services; we could use the distributed filesystem to store everything. This comment stuck with me and I still think about it a lot.

Alas, we didn’t have such a system at the time. But object storage systems like [S3](https://aws.amazon.com/s3/) have grown to give us all the properties we need; they are [insanely scalable](https://www.youtube.com/watch?v=sc3J4McebHE) and provide [atomic operations](https://aws.amazon.com/about-aws/whats-new/2024/11/amazon-s3-functionality-conditional-writes/) needed for transactional workloads. Object stores are still missing POSIX semantics, though. You can’t take any old system that uses filesystem I/O libraries and use S3 as its storage layer.

In the absence of a POSIX API for S3, two approaches have emerged to leverage what object stores have to offer. The first is building S3 directly into the systems, which most database and streaming companies are doing. [Neon](https://neon.tech/), [WarpStream](https://warpstream.com/), [Turbopuffer](https://turbopuffer.com/), and [Responsive](https://responsive.dev/) ﹩ are all in this category. The other is to wrap S3 in a [filesystem in userspace](https://en.wikipedia.org/wiki/Filesystem_in_Userspace) (FUSE) interface or an NFS-based implementation. [Amazon S3 File Gateway](https://aws.amazon.com/storagegateway/file/s3/), [JuiceFS](https://juicefs.com/), [s3fs](https://github.com/s3fs-fuse/s3fs-fuse), and [Goofys](https://github.com/kahing/goofys) are examples of such an approach. (If you squint, [Apache OpenDAL](https://opendal.apache.org/) fits in this category, but inverts the relationship by wrapping everything in its own API.)

Direct integration requires a storage-layer rewrite. I/O calls must be converted to S3-compatible API calls. Moreover, each system needs to figure out how to deal with higher object storage latencies, a subject I’ve written about before.

Direct object storage integration makes sense for systems like databases, whose primary job is to store and query data. But for systems, frameworks, and libraries that just need to read and write files as part of a broader workload, rewriting the storage layer for object storage is too burdensome.

FUSE and NFS-based implementations present their own challenges. s3fs and other FUSE-based systems implement only a subset of the POSIX interface, often missing features such as random access writes or appends, metadata operations, atomic renames, hardlinks, and inotify features. Such limitations simply won’t work for many systems and libraries. It’s not clear if RocksDB, for example, [can safely be run on EFS](https://slatedb.io/docs/faq#how-is-this-different-from-rocksdb-on-efs). And some implementations, such as JuiceFS, store files in their own block format, which limits interoperability.

The need for scalable filesystems has grown, too. Database and streaming use cases have been around for a long time, but the growth of Kubernetes and AI workloads are new. Kubernetes has made every system distributed. And with AI, multi-modal data such as audio, text, and video is a critical ingredient. Many ML and AI libraries are built with local filesystems, and those that support object storage often don’t have good caching to speed up workloads.

I think we’re finally getting to the point where we have both the technology and the demand to get us what we need: a distributed POSIX-compatible filesystem. [Regatta Storage](https://regattastorage.com/) is building in this direction, with a stated goal of replacing EFS and [elastic block store](https://aws.amazon.com/ebs/) (EBS) [general purpose](https://aws.amazon.com/ebs/general-purpose/) (gp3) use cases.

![](https://substackcdn.com/image/fetch/$s_!T0_1!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F054d5492-9aba-4c87-8f97-4750c8bcfbee_687x161.png)

[View Post](https://x.com/jhleath/status/1864688036694642873)

Though Regatta doesn’t have complete POSIX compatibility, their offering is compelling. They are using NFS now, and moving to their own protocol. This should give them a lot of flexibility as they try to implement more obscure POSIX features. Even if they never get complete POSIX support, they should be able to get closer than current solutions. Regatta also provides a generic cache to reduce latency, a key ingredient for a disk-like experience. Such an architecture is akin to a generic version of Neon’s [Safekeepers and Pageservers](https://neon.tech/docs/introduction/architecture-overview), something [I’ve been dreaming of for a while](https://bsky.app/profile/archive.chris.blue/post/3l7k2th2pno2l). ==Plus, unlike JuiceFS, files appear in object storage as normal files rather than opaque blocks that must be accessed only through the JuiceFS interface.==

I expect this area to get more competitive. Much as competition with S3 has driven AWS to improve its offering, I anticipate more EFS features in the future. Other object store providers such as [Tigris](https://tigrisdata.com/) ﹩are also well positioned to pursue this area. JuiceFS and Alluxio will also continue to make progress, too. AI-specific offerings could also emerge. Fortunately, I think the TAM is big enough (and the use cases diverse enough) to support winners for different use cases, even if the underlying technology is largely the same.

