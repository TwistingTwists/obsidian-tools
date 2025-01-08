---
tags:
  - work
  - yral
---


  

Debugging failed canister upgrade

- [x] Root is not the controller for estate_backend
- [ ] canister post_upgrade hook is failing between `99bdc3d1229046ab0bc8d9bbbf8513e4ec8ac713` and `v0.2.2`
	- [x] tested locally by upgrading from [[99bdc3d1229046ab0bc8d9bbbf8513e4ec8ac713 to v0.2.2]] to v0.2.2 : upgrade is successful 
	- [ ] test if the data is retained between upgrades?
- [ ] canister_status shows this
	- Why is memory allocation 0 ? Is that stable memory?

```md
Status: Running
Controllers: abhsa-pyaaa-aaaaq-aac3q-cai znxnh-f2v3a-dwwtd-jhyr2-tzdg7-xrm43-xf6xc-q4nfk-arhfb-54k5n-zae
Memory allocation: 0
Compute allocation: 0
Freezing threshold: 2_592_000
Idle cycles burned per day: 20_810_872
Memory Size: Nat(2036445)
Balance: 2_595_210_175_927 Cycles
Reserved: 0 Cycles
Reserved cycles limit: 5_000_000_000_000 Cycles
Wasm memory limit: 3_221_225_472 Bytes
Module hash: 0xb3fb8822e07a932e6f06e65002577b846bd5d6aea405156780e84344e4e8df4c
Number of queries: 0
Instructions spent in queries: 0
Total query request payload size (bytes): 0
Total query response payload size (bytes): 0
Log visibility: public
```

 

2024-12-19 09:50:11
- [ ] fly ips allocate-v6 --private
- [ ] 

2024-12-18 12:59:57

- [ ] fly egress IP 
- [ ] deploy UI changes 
- [ ] Study sns canister deployment 
	- [ ] write and deploy 
	- [ ] add the guards  - admin etc 
- [ ] Oban review




myapp -> nginx (server) IPv4 -> provab

myapp -> GCP SWP (secure web proxy) IPv6 -> provab


SWP : envoy(server)
server = VM (compute Engine)

Network = certbot = Certificate Manager (TLS)

network services = envoy / nginx

network security = cloud armor (x-api-key = "asdfasdfds") (header, IAM roles, website name , rate limit)

Global / regional

- SWP-montreal : proxy subnet (internal 24-64 IPv4's = E_pool)

........... -> SWP (E) -> CloudNAT / VPC


........... -> SWP (E_pool) -> (10.1.1.1) CloudNAT (reserved IPv4 = 5.67.63.56) -> Provab

........... -> SWP (E_pool) -> (10.1.1.2) CloudNAT (reserved IPv4 = 5.67.63.56) (TLS) -> Provab


TLS belongs to admin (why? - admin can literally SEEEEEE the traffic)



> "You can use external load balancer backend services as targets."

  
  
  
----

ERROR 1:

Failed to create estatedao-provab-proxy: resource.network: provided network projects/yral-swp-test/global/networks/default does not contain a subnet with purpose REGIONAL_MANAGED_PROXY

  

https://www.perplexity.ai/search/operation-type-insert-failed-w-XMxhcZMBS..e8K55PLgopw

----



----
2024-11-21 09:32:59




- [ ] booking handler 
  - [x] overview - make plan 
  - [x] redeploy the backend with latest code 
  - [x] check if the data is being saved in backend
  
  - [x] make check_payment_status  api call 
  - [x] implement book room api call 
  - [ ] book_room backend call
	  - [x] setup serverFN to make backend call 
	  - [x] book_room - BE - make request
	  - [ ] set signals for Sucess variant
	  - [ ] book_room test api call 


- [x]  payments handler


- [ ]  BACKEND
  - [ ]  guard - admin / controller 
  - [ ]  [optional]   add_admin / add_controller
  - [ ]  book_room_response - save data 