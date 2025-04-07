---
tags:
  - interview
---


| year | topic           | tech                     | good                                                                                                                                                                                      | not so good                                                            | keywords                                                                 | anecdotes                                                   |
| ---- | --------------- | ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------------------ | ----------------------------------------------------------- |
| 2017 | UPSC            |                          |                                                                                                                                                                                           |                                                                        |                                                                          |                                                             |
| 2019 | project ias     | node + react             |                                                                                                                                                                                           |                                                                        |                                                                          |                                                             |
| 2020 | plants n you    | python workflows         | got me started - fastapi + pandas + airflow for bg jobs                                                                                                                                   | ops heavy business<br><br>(end to end visibility in system is limited) | fastapi                                                                  | infra in other langs (airflow) =>  library in elixir (oban) |
| 2021 | pumpumpum       | angular +                | dashboard + nested forms from json - component libraries                                                                                                                                  | ops heavy business                                                     | full stack , product ownership                                           |                                                             |
|      | mathocrat       | liveview full stack      | end-to-end                                                                                                                                                                                |                                                                        | end-to-end manager (PM -> sales -> Engineering)                          |                                                             |
| 2022 | Tinymesh        | elixir + IoT + liveview  | elixir + event_bus + elixir on rpi +<br>Tauri                                                                                                                                             |                                                                        | IoT, elixir, gen_state_machine, liveview                                 |                                                             |
| 2023 | tinymesh        |                          |                                                                                                                                                                                           |                                                                        |                                                                          |                                                             |
|      | stord freelance |                          |                                                                                                                                                                                           |                                                                        |                                                                          |                                                             |
|      | xplor freelance | graphql API optimisation |                                                                                                                                                                                           |                                                                        |                                                                          |                                                             |
| 2024 | yral            |                          | log and observability system using vector (fly logs exporter) nats streaming<br><br>nginx like reverse proxy in Rust (Why?)<br><br>EventBus in rust <br><br>Workflow orchestrator in rust |                                                                        | axum, observability,  event_bus, workflow_orchestrator, payment systems, |                                                             |
| 2025 | yral            |                          | <br>- frontend components => rust wasm (futures) + js interface (promises)<br><br>- rust axum reverse proxy + tonic grpc                                                                  |                                                                        | wasm, grpc,                                                              |                                                             |
|      |                 |                          |                                                                                                                                                                                           |                                                                        |                                                                          |                                                             |


### Projects 

- OS event loop - in rust?? epoll to listen fd 
- writing multi threaded - redis
- [[connection pool ]]

 <details>
  <summary>state machine for managing resumable FOTA uploads</summary>
  <p>17 minutes to 7 minutes</p>
</details>

- pubsub in rust - event_bus_tokio  
- (todo) wal implementation
- (todo) Distributed logs backed by S3tables
- (todo) fly dist sys
- dns_query_caching
- linux peripheral devices

- [[TCP window scaling algorithm]] but for probing keep-alive of the server.


DBMS 
- (todo) partioning / sharding / query patterns / replication / 
- 

Kafka
- (todo) grafana dashboard

AI 
- gvisor based python repl - with runsc
- project-manager-cli
- meilisearch - semantic search over questions
- (todo) deep dive containerd

### Professional
- grafana dashboards for linux node + kafka
- aws 




### most impactful projects?

- mathocrat
	- ffmpeg - trancode to 3 formats - display via CDN 
		- HLS streaming  + ABR streaming
	- Resumable uploads
	- live_svelte integration for carousel component

- Angular - libary to render form component based on nested json 

- [[graphql query optimisation]]
- [[AWS Lambda for video encoding pipeline ]]


- kafka 
	- implementing a [[queue mechanism to buffer the requests]] to overloaded kafka queue
	- Detecting consumer lag , and rate of consumer lag in datadog 

- elixir
	- PRs in Ash recently 
	- Instructor
- Rust 
	- Tauri, 
	- leptos (WASM) = Box / Callable use krke - wasm_bindgen - extensible modal component
	- cli - project manager cli -- 
- Session management at L4 layer - GenStateMachine - directly on top of TCP socket (gen_tcp)
	- resumable data transfer on RF



