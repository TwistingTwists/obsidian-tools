
- vehicle types 
- floors 
- pricing model - vehicle type. not hr 

questions: 
1. realtime avaiability by vehicle type (entire view) - filters -> per floor, vehicle type
2. for a vehicle -> which slot is empty?
3. park / unpark for a vehicle
	1. parking -> get a ticket 
	2. unparking -> make payment 
4. edge cases: 
	1. What if parking for one vehicle type is full?
	2. assumption - one entry and one exit at each floor
	3. prebooking, dynamic pricing, loyalty cards or discounts - not implemented
	4. finding out average parking time, max parking time, notifying user for a very long parked vehicle - not implemented 

```python

class Vehicle(BaseModel):
	vehicle_number: str 
	vehicle_type: Literal["heavy-duty-4-wheels", "cars", "two-wheelers", "accessible-vehicle"]
	

class ParkingSpot(BaseModel):
	number: int
	vehicle_type: Literal["heavy-duty-4-wheels", "cars", "two-wheelers", "accessible-vehicle"]
	floor: int

class Ticket(BaseModel):
	id: uuid7
	vehicle: Vehicle
	parked_location: ParkingSpot
	parket_at: utc
	unparked_at: utc 

class Payments(BaseModel):
	ticket: Ticket
	
	amount_paid: int
	currency: str # INR, USD
	denominations: str # INR -> rupees, paise. USD -> dollars , cents
	paid_at: utc
	payment_status: str # SUCCESS / FAIL 
	
```