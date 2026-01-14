---
tags:
  - lld
---
# Parking Lot

Explore the design of a parking lot system by understanding requirements like vehicle and parking spot types, payment options, pricing models, and real-time tracking. Learn to apply object-oriented design principles and a bottom-up approach to create scalable, maintainable solutions suitable for low-level design interviews.

## Problem definition

A **parking lot** is a designated area for parking vehicles, commonly found in venues like shopping malls, sports stadiums, and office buildings. It consists of a fixed number of parking spots allocated for different types of vehicles. Each spot is charged based on the duration a vehicle remains parked. Parking time is tracked using a parking ticket issued at the entrance. Upon exit, the customer can pay using either an automated exit panel or through a parking agent, with a credit/debit card or cash as accepted payment methods.

In this LLD interview case study, you’ll focus on:

- Managing parking for various vehicle types (car, van, truck, motorcycle, etc.) across potentially multiple floors and zones.
- Allocating and tracking parking spot availability by type (e.g., compact, large, accessible, motorcycle).
- Issuing and managing parking tickets for tracking entry, parking duration, and exit.
- Handling flexible payments at automated exit panels or via human agents, supporting multiple payment methods (cash, card, coupon).
- Implementing and enforcing pricing models that may vary by hour, spot type, or vehicle type.
- Ensuring real-time tracking of spot occupancy and enabling efficient entry/exit flows, especially during busy periods.
- Accommodating special considerations like accessible parking spots and overflow situations.

This parking lot system model can be adapted for various venues (malls, stadiums, offices, airports) and is extensible to different business rules (such as prebooking, dynamic pricing, or loyalty programs).

![](data:image/svg+xml,%3csvg%20xmlns=%27http://www.w3.org/2000/svg%27%20version=%271.1%27%20width=%27550%27%20height=%27337.94000000000005%27/%3e)![The layout of the parking lot](https://www.educative.io/api/collection/10370001/5583710957338624/page/6276164341727232/image/4723785166880768?page_type=collection_lesson&get_optimised=true&collection_token=undefined "The layout of the parking lot")

The layout of the parking lot

## Expectations from the interviewee[](https://www.educative.io/courses/grokking-the-low-level-design-interview-using-ood-principles/getting-ready-parking-lot#Expectations-from-the-interviewee)

A typical parking lot system has several components, each with specific constraints and requirements. The following section provides an overview of some major expectations the interviewer will want an interviewee to discuss in more detail.

### Payment flexibility [](https://www.educative.io/courses/grokking-the-low-level-design-interview-using-ood-principles/getting-ready-parking-lot#Payment-flexibility)

One of the most significant attributes of the parking lot system is the payment structure that it provides to its customers. An interviewer would expect you to ask questions like these:

- How can customers pay at different exit points (i.e., at the automated exit panel or to the parking agent) and by different methods (cash, credit, or coupon)?
    
- If the parking lot has multiple floors, how will the system keep track of customers who have already paid on a particular floor rather than at the exit?
    

### Parking spot type [](https://www.educative.io/courses/grokking-the-low-level-design-interview-using-ood-principles/getting-ready-parking-lot#Parking-spot-type)

Another topic of discussion that an interviewer would expect you to be aware of is the different parking spot types (accessible, compact, large, and motorcycle), regarding which you can ask the following questions:

- How will the parking capacity of each lot be considered?
    
- What happens when a lot becomes full?
    
- How can one keep track of the free parking spots on each floor if there are multiple floors in the parking lot?
    
- How will the parking spots be divided among the four parking spot types in the lot?
    

### Vehicle types[](https://www.educative.io/courses/grokking-the-low-level-design-interview-using-ood-principles/getting-ready-parking-lot#Vehicle-types)

Similar to the parking spot, an interviewer would also expect you to discuss the different vehicle types (car, truck, van, motorcycle), which can have the following set of questions:

- How will capacity be allocated for different vehicle types?
- If the parking spot of any vehicle type is booked, can a vehicle of another type park in the designated parking spot?
    

### Pricing[](https://www.educative.io/courses/grokking-the-low-level-design-interview-using-ood-principles/getting-ready-parking-lot#Pricing)

We touched upon the payment structure offered by the parking lot system. Now, the pricing model needs to be clarified by the interviewer, and therefore, you may ask questions like these:

- How will pricing be handled? Should we accommodate having different rates for each hour? (For example, customers will have to pay $4$4 for the first hour, $3.5$3.5 for the second and third hours, and $2.5$2.5 for all the subsequent hours.)
    
- Will the pricing be the same for the different vehicle types?
    

## Design approach[](https://www.educative.io/courses/grokking-the-low-level-design-interview-using-ood-principles/getting-ready-parking-lot#Design-approach)

We will design this parking lot system using a bottom-up design approach. For this purpose, we will follow the steps below:

- First, we’ll identify the core entities such as `Vehicle`, `ParkingSpot`, `ParkingTicket`, and `Payment`, and define their primary responsibilities.
    
- Next, we’ll model how vehicles are assigned to appropriate parking spots based on type and availability, how entry/exit is managed, and how parking duration is tracked.
    
- We’ll design the system to support different payment methods and flexible pricing rules, ensuring secure and accurate fee calculation.
    
- We’ll ensure the system supports scalability (multiple floors, high traffic), real-time updates on availability, and follows SOLID principles for maintainability.
    

Diagrams and code will illustrate the main workflows and class structures, discussed later in this case study.