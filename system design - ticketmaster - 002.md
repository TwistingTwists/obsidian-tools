---
finished: 2025-12-18
---


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