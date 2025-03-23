---
tags:
  - interview
---
DBMS 
- What lies where?
	- Locations - backward, forward, tote, processing counters
- What will expire when ?





---
  
a warehouse has inbound orders and outbound orders.

one inbound order has items, so does outbound order.

inbound orders have to be placed (put) onto locations. outbound orders have to picked from locations.

now, locations maybe of different types - storage locations - big racks for bulk packed items or smaller locations - small racks for loose items, unboxed / small box items

another way to think about this is - backward locations (in warehouse) and forward locations (in warehouse - smaller ones)

put can have different strategies pick can have different strategies.

```
This is a great example of warehouse operations with complex location management. Let me break down the different query patterns needed to handle this warehouse scenario:

# Inbound Processing Queries

- **Receiving Verification**: Matching received items against purchase orders
- **Put-Away Location Assignment**: Determining optimal storage locations based on item characteristics
- **Bulk Storage Allocation**: Finding available rack space for palletized/boxed items
- **Cross-Docking Identification**: Flagging items that can bypass storage and go directly to outbound

# Location Management Queries

- **Location Type Lookup**: Identifying which locations are bulk/backward vs. small/forward
- **Location Capacity**: Checking available space in specific locations
- **Location Compatibility**: Matching item attributes (size, weight, hazard class) to location constraints
- **Location Proximity**: Finding nearby locations for efficient put-away or picking

# Put-Away Strategy Queries

- **First-Available Location**: Finding the first empty location in a zone
- **Zone Optimization**: Locating space in zones based on turnover rates
- **FIFO/FEFO Placement**: Positioning based on date (first-in-first-out or first-expired-first-out)
- **Item Affinity**: Grouping related items or keeping incompatible items separated
- **Load Balancing**: Distributing inventory across multiple locations

# Picking Strategy Queries

- **Batch Picking**: Grouping multiple orders for efficient picking routes
- **Wave Picking**: Organizing picks by zone or time windows
- **Zone Picking**: Routing pickers through specific warehouse areas
- **FIFO/FEFO Selection**: Picking oldest inventory or soonest-to-expire items first
- **Pick Path Optimization**: Calculating optimal routes through the warehouse

# Inventory Movement Queries

- **Replenishment Triggers**: Identifying when forward locations need restocking from bulk storage
- **Location Transfer**: Moving items between backward and forward locations
- **Consolidation**: Combining partial quantities from multiple locations
- **Slotting Optimization**: Repositioning items based on velocity and access frequency

# Operational Status Queries

- **Order Fulfillment Status**: Tracking progress of picks against outbound orders
- **Pick Completion Verification**: Confirming all items for an order have been picked
- **Inventory Discrepancy**: Identifying location content mismatches during operations
- **Labor Tracking**: Monitoring picker/putter efficiency and workload distribution

These query patterns need to handle the complex workflows and decision points in warehouse operations, considering both the physical constraints of your facility and the optimization strategies you've implemented.
```

---
