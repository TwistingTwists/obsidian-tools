---
tags:
  - elixir
---

## observability 

## Do you really need processes ?

## Kafka clients in elixir
1. 3 clients have different configurabilities 
2. Jinterface and elixir wrapper over official apache kafka client may help in overcoming feature deficit 
	1. Downsides are - memory and speed overhead
	2. One can use ports -- with Port.spawn_executable 


## CLIP based models for Reverse image search 
1. CLIP - vectorises text via text-encoder and similarly image-encoder
2. Bumblee can help use hf models right off the bat 
3. now, both vectors can be compared similarity with cosine / other metrics 
4. Advantage of Nx.Serving is that the number of queries are batched and transferred to GPU without changing the code.
5. distributed elixir is for free with Nx.Serving.

## advantages of datalog 
1. Declarative programming skips defining the control flow
2. datalog = sql with recursion 
3. three pillars = facts, rules, queries  
4. interesting use cases in 
	1. network configuration and policy framework 
	2. static analysis 


## De umbrellisation 
1. Leveraing libcluster and a macro to do 
	1. :erpc calls with elixir 
	2. generate the output from remote node and take it here.
2. removes the need to implement protobuf etc like needed for grpc 
3. also, leverages erlang.term_to_binary and reverse to :erpc calls 
4. downside -- the challenges of disributed systems are there. Other system  / microservice might be down and there needs to be that many application monitoring / logs 
5. 