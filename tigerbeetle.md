

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

- DIrect I/O syscall => buffered + event loop + scheduling
----

## Crash Recovery in DB

- WAL - write ahead log
- Distributed systems - cluster - replica (repair)


## OS

- buffered + event loop + scheduling
- io_uring =  event loop 

## Testing 


- postgres -- 




## BIGTABLE


| key    | col1              | col2              |
| ------ | ----------------- | ----------------- |
| gaurav | val1 (t0, t1, t2) | val2 (t0, t1, t2) |
| url2   |                   |                   |
