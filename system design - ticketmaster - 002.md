---
finished: 2025-12-18
---
### Solving "Seat Reservation & Concurrency"

#### L5 

- redis for locking with TTL, postgres for transactions 
	- postgres is a source of truth 
- postgres FOR UPDATE (row level locks)
	- contention at scale
#### L7 

- [[row level update#1. Approach - "Database-Only Optimistic Locking"]] with Cron cleanups
	- 5000 users booking same seat, at same time?
	- what if user B takes the row before A can pay?
- [[row level update#To Cron job or NOT | Cron Alternative]]

> [!summary]- Cron - perfect ans
> 
> ### The "Perfect" Interview Response
> "While a Cron job is a good safety net, it introduces 'Inventory Drag' where seats are unavailable but not sold.
> For the primary mechanism, I would implement a **Delayed Task Queue** (like SQS/RabbitMQ). When a seat is held, we push a message with a 10-minute delay. When the worker receives it, it performs an idempotent check against the database to release the seat if it's still 'HELD'. This gives us near-real-time inventory recovery without the massive database load of scanning the entire table every minute."

 [[Other exploration - framework for choosing the queue]]
### Solving "Thundering Herd" (The Taylor Swift Problem)

#### L5 

#### L7 


| functional                                                                                                                                                | my answer |
| --------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| Users must be able to convert a "Hold" into a permanent "Booking" upon successful payment confirmation.<br><br>                                           |           |
| When a user selects a seat, it must be temporarily "locked" or "held" for a configurable duration (e.g., 10 minutes) while they complete payment.<br><br> |           |
| Users must be able to view the availability of specific seats in near real-time.<br><br>                                                                  |           |
Entities 
- payments
- bookings
- users
- events
- seats
- venue

event has a venue
seat has a booking  

[[ai-chat-001-ticketmaster]]
[[pg advisory locks]]