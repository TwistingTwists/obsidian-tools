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

Queries
- which slots are empty 
	- filter_by type, filter_by floor, sort_by price
- Payment - price for that slot (slot_type + floor + hour)

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