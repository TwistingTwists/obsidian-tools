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

- vehicle has_one slot 
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
	price: u64,
	vehicle_number_plate: String,
	slot_id: String

}

enum SlotTypes {
	Car, Bike, Handicapped
}

enum SlotStatus {
	Vacant, Occupied
}

struct Slot {
	id: String
	floor_id: Floor, 
	slot_type: SlotTypes,
	status: SlotStatus
}

enum PricingUnits {
	Minutely, 
	Hourly, 
	Daily,
	Weekly
}

enum PossiblePricingDrivingFactors {
	Slot(Slot),
	Vehicle(Vehicle)
}

struct PricingFactors {
	// pricing_factors - name_fkey (from Slots Table), unit_price, unit, description
	name_fkey: String,
	// name_table: String,
	name_table: PossiblePricingDrivingFactors
	unit_price: f32,
	unit: PricingUnits,
	
}

struct DisplayBoard {
	slots: BTreeMap<Floor, Slot>
}

struct Payments {
	ticket_id: uuid, 
	amount: f32,
	
}

// ------ 

enum SlotFilters{
	SlotType(SlotTypes),
	SlotStatus(SlotStatus)
}

enum SlotFilterExpr{
	All, 
	Filter(SlotFilter),
	And(Box<SlotFilterExpr>,Box<SlotFilterExpr>),
	Or(Box<SlotFilterExpr>,Box<SlotFilterExpr>)
}

impl SlotFilterExpr{
	pub matches(&self,slot: &Slot) -> bool {
		match self {
		All -> true,
		Filter(slot_filter) -> {
			match slot_filter {
				SlotType(slot_type) -> slot_type == slot.slot_type ,
				SlotStatus(slot_status) -> slot_status == slot.status
			}
		},
		And(filter_1, filter_2) -> filter_1.matches(&slot) && filter_2.matches(&slot),
		Or(filter_1, filter_2) -> filter_1.matches(&slot) || filter_2.matches(&slot)
		}	
	}
	
	pub and( self, other_filter_expr: SlotFilterExpr) -> SlotFilterExpr{
		SlotFilterExpr::And(Box::new(self), Box::new(other_filter_expr))
	}
	
	pub or( self, other_filter_expr: SlotFilterExpr) -> SlotFilterExpr{
		SlotFilterExpr::Or(Box::new(self), Box::new(other_filter_expr))
	} 
}

// ------ 

struct Parking{}

impl Parking{

	fn park(vehicle: Vehicle) -> Ticket {
	}

	fn unpark(ticket: Ticket) -> Vehicle {
	}

	fn available_slots(filter_by: SlotFilterExpr) -> DisplayBoard {
	
	}
}


// ------- 
// TRAITS 

trait SlotFinder {
	pub fn find_vacant_slot(filter_expr: SlotFilterExpr) -> DisplayBoard {
		let default_filter = SlotFilterExpr::Filter(SlotType(Vacant))
	}
}
```



Alternate way to implement `SortFilter`

```rust
enum SlotFilterExpr {
    All,
    Filter(SlotFilter),
    And(Vec<SlotFilterExpr>),
    Or(Vec<SlotFilterExpr>),
}
```