---
tags:
  - work
---




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



- [ ] binance iamge  remove
- [ ] radio button to circle
- [ ] loading icon on the block room 