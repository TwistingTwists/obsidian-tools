---
finished: 2025-12-18
---
### Solving "Seat Reservation & Concurrency"

#### L5 

- redis for locking with TTL, postgres for transactions 
	- postgres is a south of truth 
- postgres FOR UPDATE (row level locks)
	- contention at scale
#### L7 

- [[row level update#1. The Approach "Database-Only Optimistic Locking" | Optimistic DB Locks]]
	- 5000 users booking same seat, at same time?
	- what if user B takes the row before A can pay?



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