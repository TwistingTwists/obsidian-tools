---
tags:
  - elixir
  - book
  - blogs
  - content_creation
---

### [[Writing a webserver ]]

### Buffered writer

### Writing a Scalable QuizBot in elixir -  QuizSession in OTP book yellow

### Processing tasks - Patterns from Svilen Gospodinov book 

- In order, parallel processing using Tasks

### Broadway - recursive broadway 
https://youtu.be/TcLemx0MFV0

### Workflow orchestration

Broadway -> Telemetry + reactor + live dashboard 
https://hexdocs.pm/pacer/Pacer.Docs.html


### UniqueTask - SystemWide

### Task.Supervisor.async_nolink 

### Task.async 

- [ ] [[Monitor your ports! ]] - towards porcelian?
- [ ] Write your own await -> [Source](https://github.com/alco/porcelain/blob/d240c7946e12f7fc878a6663b4e31952c78dc1da/lib/porcelain/process.ex#L46-L59)
- [ ] https://github.com/tonyc/elixir_ports_example
- [ ] 
- [ ] 


### Cachex - architecture - request coalescing

### receive do in Task.async 

### Handling exits  

https://github.com/koudelka/honeydew/blob/7c0e825c70ef4b72c82d02ca95491e7365d6b2e8/lib/honeydew/worker.ex#L209



### fast `send` across many processes 
https://erlangforums.com/t/send-optimization-for-sending-to-many-processes/2200

[manifold](https://github.com/discord/manifold)

The Erlang code snippet provided is part of a message queuing system designed to improve message passing between Erlang nodes, particularly across a network. The code is from an article by Roberto Ostinelli, which discusses the performance of message passing in Erlang and presents a queuing mechanism to boost internode communication. The article can be found at Boost message passing between Erlang nodes.

Here's an explanation of the code based on the article:

### handle_cast/2 Function

This function is part of a gen_server behavior and is responsible for handling asynchronous messages (cast operations) sent to the server. The message pattern it expects is {{queue, DestNode}, {ToPid, Message}}, where DestNode is the destination node's identifier, ToPid is the process identifier where the message should be routed, and Message is the actual message to be sent.

- If the destination node (DestNode) is not already in the queue, a new queue entry is created with the current time and the message.

- If the destination node is already in the queue, it checks if the message list for that node has reached a predefined length (?QUEUELENGTH). If it has, it sends all queued messages to the destination node's qr process and empties the queue for that node. If not, it adds the new message to the existing message list.

### purge_queue_selective/1 Function

This function is used to clean up the queue by checking for timeouts. It loops through the queue and checks if the time difference between the current time and the creation time of the message list for each destination node exceeds a predefined timeout value (?QUEUETIMEOUT).

- If the timeout has passed, it sends all messages in the list to the destination node's qr process and removes the node from the queue.

- If the timeout has not passed, it leaves the node in the queue.

The FilterFun is a filtering function used with lists:filter/2 to apply this logic to each item in the queue.

The qr process mentioned in the code acts as a router and queue manager, collecting messages and deciding when to send them based on queue length or timeout. This strategy is similar to the Nagle's algorithm in TCP, which also aggregates small packets to improve network efficiency.