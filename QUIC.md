

### 0-RTT resumption o fQUIC 

- MoQ - subscribale track over media
- QUIC - 0-RTT 

### MoQ over QUIC 

- connection resumption - no more TLS enogitaion if done already 
- QUIC merges transport (TCP) + cyrptographic (TLS) hanshakes into ONE
- issue - if someone captures the request in middle -> they can replay it. the handling of the request by the server HAS to be idempotent 
