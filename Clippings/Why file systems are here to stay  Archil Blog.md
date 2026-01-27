---
title: Why file systems are here to stay | Archil Blog
source: https://archil.com/post/why-file-systems-are-here-to-stay
author:
published:
created: 2026-01-27
description: The industry has discovered that the file system is a surprisingly effective interface to give AI access to data. Here's why file systems are becoming the universal context layer for AI agents.
tags:
  - clippings
  - turbopuffer
---
[Back to Blog](https://archil.com/blog)

AGENTS • 2026-01-14

## Why file systems are here to stay

Hunter Leath • 8 min read


We're two weeks into 2026, and I've never seen this much discussion about file systems. The industry has discovered that the file system is a surprisingly effective interface to give AI access to data. Why? All of the frontier labs are racing to build the best model and harness for coding tasks, which means that a disproportionate amount of training effort is going into making these systems great at using bash and other command-line tools.

Files Are All You Need

llamaindex.ai

How file-based context is becoming the standard for AI agent development

[View original](https://www.llamaindex.ai/blog/files-are-all-you-need)

## The rise of file systems for context

The next generation of applications built on AI are increasingly skewing towards "agents". At Archil, we consider something an "AI agent" if it calls an LLM in a turn-based loop, tells the LLM about external "tools", and uses a framework to parse structured LLM output to invoke real code.

This model is simple but incredibly powerful, giving AI the ability to make decisions using a large amount of data, over a long time horizon — if you can give access to the right data and systems to make those decisions.

Since 2025, the industry has been focused on the Model Context Protocol (MCP) as a way to connect these agents to external data. For example, if you were building an AI agent that communicated through Slack, you might build an MCP for your company's Slack workspace which exposed tools like `postMessageToChannel` and `readChannel` that the LLM could "invoke" when needed.

Around June, we started to notice a pattern with teams building at the frontier of AI. We kept hearing that each MCP tool that they introduced into an AI agent would subtly degrade the performance of that agent.

One key property of today's LLMs is that their performance degrades with the amount of data that you send to them. If you talk to a chatbot for enough time, it will start producing less and less coherent output. This is known as "context rot".

With MCP, each tool provided to the LLM was bespoke, and that required that the model spend some of its "context" on actually parsing the instructions for what the tool was used for and how it should be invoked. This made context rot happen faster.

These companies realized that you could reduce this problem by simplifying the vast array of MCP tools into a single, universal tool that allowed the model to do anything that a computer user would otherwise do: bash (or, in Cloudflare's case, Python). If you pushed everything through bash, the model already knew how to use it (because it's extensively trained on bash for code generation), but, more importantly, the model had the ability to iteratively discover context as it went along. If you had a file called "slack-cli" and a file called "database-cli", the model wouldn't need to even look at the "database-cli" if it was asked to check Slack for something.

As a result, the world is moving towards providing context to these models in a file system. We've even seen the rise of simple tools that make it possible to project a single file out as a file system for portability reasons.

How to Build Agents with Filesystems and Bash

vercel.com

A practical guide to building AI agents using file system-based context and bash commands

[View original](https://vercel.com/blog/how-to-build-agents-with-filesystems-and-bash)

## Cutting out the database

The file system has a long, controversial history in the age of computing — it's so controversial that we try to brand our product, Archil, as "volume storage" instead of a "file system" so we avoid common misconceptions around file system performance.

More recently, people have started to notice that databases, like SQLite or Postgres, have a similar property in the models — they provide the ability to iteratively discover structure, data, and context, as the model determines what it needs to accomplish its task. Unlike a file system, they make it easier for the models to perform relational queries on their data. Lots of people selling databases are leading the charge to push back against file systems as a context system.

*Dax very recently spoke about moving the data in OpenCode from a bunch of JSON files into a SQLite database.*

I want to emphasize that I do not believe that file systems are going to replace databases for the kind of data that can be easily expressed in a schema. I fully expect that the power of AI is the ability to work with heterogeneous data, and many agents are going to have file systems with lots of SQLite files on them to bridge this gap.

I think that part of this pushback is because using a file system directly is unnatural for most developers. For the vast majority of software history, the first thing that a software developer would do in order to CRUD-ify a process would be to define a structured schema for its data and put that data into a database.

AI models are changing this process because they have given us the first tools to easily work with unstructured data (images, binaries, code, documents) at scale **without** needing to first put it in a database. I expect this will actually **reduce** demand for databases in the human-led, document-based processes that haven't yet been converted to clean schemas. This is also one of the reasons why AI as a field is going to unlock so much innovation in fields like healthcare, finance, and the government.

In the past, file systems ([like SQL databases](https://www.youtube.com/watch?v=b2F-DItXtZs)) have been known for being cumbersome, not scalable, and incredibly expensive. In the same way that serverless databases and Vitess have made scalable SQL databases accessible, the next generation of file systems will slowly turn the tide on how people view this technology.

## File systems are the universal interface

Like language wars, neither databases, nor file systems, nor MCP will be declared the overall "winner" of the context-engineering space. Rather, they will work together to provide AI with the information it needs to accomplish the vast majority of work.

I think that file systems will play an outsized, fundamental role in the next 10 years of AI agent development. They are not just "bad databases". At the end of the day, databases are just wrappers around file formats that include a query engine. This makes them very good at reading and writing data, but not very good at providing simple access to data that they don't control (excepting, of course, DuckDB and Clickhouse).

File systems, on the other hand, are an open-ended interface into a world of different kinds of data. This is why you see Linux and Unix represent everything as a file: sockets, storage devices, and even just functions that are implemented in the kernel. [Everything is a file](https://en.wikipedia.org/wiki/Everything_is_a_file), but the cloud hasn't caught up to this reality.

The other looming challenge with file systems as a context tool is how you package up and share them. This is essential to how we think through the deployment and orchestration systems of the future. It's why we're currently seeing some people finding ways to package entire file systems into single-files. This makes them super simple to share, but really bad at holding large amounts of data.

## The missing "state container" for agents

At Archil, we're building our own answer to the file system container problem, and that there's a better solution to zipping up your entire file system and moving it around.

We think that context for agents will move in the direction of software dependencies. Rather than "vendoring" all of your dependencies into your git tree, you specify the dependencies that you have and the compiler (or runtime) will dynamically pull those dependencies in later. We foresee a future in which developers are packaging agents with "agent.json" files that specify the data that the agent has access to. These could be data in S3 buckets, data from business applications with simple adapters, data in databases like SQLite, and the agent's own prompts, code, or history.

Archil already transforms remote S3 buckets into local file systems for use with POSIX applications and AI agents. In the next few months, like Postgres, we're opening Archil to backend extensions which will allow developers to manifest and synchronize *any* data into a local file system with a simple S3 interface adapter — for the first extensible file system. Developers get a simple way to bring in enormous context for their agents and we take care of snapshotting/checkpointing and authentication.

/agent/context

Archil Volume

Data Sources

Materialized File System

slack

engineering

2026-01-19.json 24 KB

2026-01-18.json 31 KB

2026-01-17.json 28 KB

general

2026-01-19.json 12 KB

logs

api-gateway

access.log 2.3 MB

error.log 156 KB

workers

job-processor.log 890 KB

database

syncing

users.db 4.2 MB

sessions.db 1.1 MB

analytics.db 12 MB

repo

src

index.ts 2 KB

handlers

README.md 3 KB

agent.json → 4 data sources materialized as local paths

This ability to package up remote data into something like a dependency list will be the first step towards making agents first-class primitives for deployment, sharing, and executing. Using the same underlying storage system will easily allow these agents to share state when they do handoffs, like from the coding agent to the code review agent.

We think that materialization is only the beginning of what's possible with the file system at the center of data access. File system stagnation over the past 20 years has meant that we haven't even begun to explore what it might be like to: automatically store vectors for documents in a file system folder for similarity search or produce more powerful retrieval tools like map-reduce-grep.

We are so excited for what's to come in 2026, and we hope that you'll be a part of it with us.

[Back to Blog](https://archil.com/blog)