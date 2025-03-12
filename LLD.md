---
tags:
  - interview
  - lld
---
### **Question 1: Design a Parking Lot System**

Imagine you need to design a system to manage a **parking lot**. The system should handle the following requirements:

- Multiple floors and different types of parking spots (compact, large, handicapped, etc.).
- Vehicle entry and exit.
- Payment system for parking fees.
- Real-time availability display.


Clarifications;

Queries:
- which slots are empty 
	- filter_by type, filter_by floor, sort_by price
- Payment - price for that slot (slot_type + floor + _hour of the day_) *  duration

Relationships:
- floor has_many slots
- slot =  type, floor_id, status (VACANT, OCCUPIED),  
- price has_many pricing_factors
- pricing_factors - name_fkey, unit_price, unit, description
	- pricing_factors - type, floor, unit_price, unit, description

- vehicle can occupy spot 
- vehicle = number_plate, in, out
- ticket has_one vehicle
- payment is made for a ticket
- payments - amount, ticket_id

OOP style

```



```

Functional Style (Rust)

```rust

type Floor = u8;

struct Parking {
spots: BTreeMap<Floor,
}


```