---
tags:
  - zig
  - elixir
---


AIM : 

- [ ] ship elixir executables on systems where C might not be installed! 

Problems to solve 
- [ ] Setup needed for the machine - zig scripts / bash scripts to run before the application is booted up 
	- [ ] on a fresh server, install ABC stuff, install elixir / erlang with wx etc and then go on and launch the application 
- [ ] 

How?
1. Use zig to statically compile everything into self contained binaries - burrito
2. zig scripts collections to various use cases
	1.  zig can cross compile
3. Testcontainers API - run the 'packaging' job as github action or on your machine 
4. 