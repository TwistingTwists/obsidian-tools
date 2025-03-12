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


### Clarifications

Queries:
- which slots are empty 
	- filter_by type, filter_by floor, sort_by price
- Payment - price for that slot (slot_type + floor + _hour of the day_) *  duration

Relationships:
- floor has_many slots
- slot =  type, floor_id, status (VACANT, OCCUPIED),  ticket_id
- price has_many pricing_factors
- pricing_factors - name_fkey, unit_price, unit, description
	- pricing_factors - type, floor, unit_price, unit, description

- vehicle has_one spot 
- vehicle = number_plate, in, out, spot_id
- ticket has_one vehicle
- payment is made for a ticket
- payments - amount, ticket_id

Core questions:
- un/park vehicle
- issue ticket for vehicle
- find empty slot by type , floor, price


OOP style

```



```

Functional Style (Rust)

```rust

type Floor = u8;

enum VehicleType {
Car, Bike, Truck
}

struct Vehicle {
	number_plate: String,
	vehicle_type: VehicleType,
	ticket_id: uuid
}

struct Ticket {
	id: uuid, 
	price: u64
}

enum SlotTypes{
	Car, Bike, Handicapped
}

enum SlotStatus{
	Vacant, Occupied
}

struct Slot {
	floor_id: Floor, 
	slot_type: SlotTypes,
	status: SlotStatus
}

struct DisplayBoard{
	slots: BTreeMap<Floor, Slot>
	

}
```