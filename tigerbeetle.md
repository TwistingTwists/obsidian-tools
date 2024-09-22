

## Durability - guarantee.

=> data persistence **if promised**.

- Disk failure
- OS failure

ACID - D : durability

- postgres - ack => data loss 0%
	- postgres - ack => data loss != 0% (***0.01%***)
		- D: mmap() + write / fsync -> fsyncgate + 
			- OS + CMU db course


Tigerbeetle DB 
- Financial transactions = D: 0.01% , disk failures
	- postgres financial => 

- DIrect I/O syscall => buffered + 
----

## Crash Recovery in DB

- WAL - write ahead log
- Distributed systems - cluster - replica (repair)


## OS