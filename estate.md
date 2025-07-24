2025-07-01

- backend_canister - build_index for payment_id - What if there is a duplicate id already? -- show the passed one?


```json 
INFO estate_fe::logging: gzip = false , response_bytes_or_string : {"data":{"bookingId":"a0QUjt4wD","clientReference":"HB0107-73281-54385","supplierBookingId":"8103915","supplierBookingName":"nuitee","supplier":"nuitee","supplierId":2,"status":"CONFIRMED","hotelConfirmationCode":"","checkin":"2025-07-04","checkout":"2025-07-05","hotel":{"hotelId":"lp3ce3e","name":"Hotel Southern"},"bookedRooms":[{"roomType":{"name":"Deluxe Room, Private Bathroom"},"boardType":"RO","boardName":"Room Only","adults":2,"children":0,"rate":{"retailRate":{"total":{"amount":33.95,"currency":"USD"}}},"firstName":"Utkarsh","lastName":"Goyal","mappedRoomId":null}],"holder":{"firstName":"Utkarsh","lastName":"Goyal","email":"utkgoyal@gmail.com","phone":""},"createdAt":"2025-07-01T04:11:00Z","cancellationPolicies":{"cancelPolicyInfos":[{"cancelTime":"2025-07-03 18:29:00","amount":32.03,"type":"amount","timezone":"GMT","currency":"USD"}],"hotelRemarks":[],"refundableTag":"RFN"},"price":33.95,"commission":1.92,"currency":"USD","remarks":"","guestId":49388},"guestLevel":0}

```



```bash

2025-07-01T04:11:03Z app[d8d9ee6f123458] bom [info]  2025-07-01T04:11:03.197583Z  INFO estate_fe::logging: deserialize_response- JsonParseFailed: "path: data.cancellationPolicies.hotelRemarks - inner: invalid type: sequence, expected a string at line 1 column 869 "
2025-07-01T04:11:03Z app[d8d9ee6f123458] bom [info]    at ssr/src/logging.rs:52
2025-07-01T04:11:03Z app[d8d9ee6f123458] bom [info]    in estate_fe::ssr_booking::booking_handler::book_room_and_update_backend_v1
2025-07-01T04:11:03Z app[d8d9ee6f123458] bom [info]    in estate_fe::ssr_booking::booking_handler::process_booking_status
2025-07-01T04:11:03Z app[d8d9ee6f123458] bom [info]    in estate_fe::ssr_booking::booking_handler::make_booking_from_booking_provider_run
2025-07-01T04:11:03Z app[d8d9ee6f123458] bom [info]    in estate_fe::ssr_booking::booking_handler::execute_make_booking
2025-07-01T04:11:03Z app[d8d9ee6f123458] bom [info]    in estate_fe::ssr_booking::pipeline::pipeline_step with name: MakeBookingFromBookingProvider
2025-07-01T04:11:03Z app[d8d9ee6f123458] bom [info]    in estate_fe::ssr_booking::pipeline::process_pipeline with event: ServerSideBookingEvent { payment_id: Some("4792896816"), provider: "nowpayments", order_id: "NP$18:HB0107-73281-54385$18:utkgoyal@gmail.com", user_email: "utkgoyal@gmail.com", payment_status: None, backend_payment_status: Some("started_processing"), backend_booking_status: None, backend_booking_struct: None }, correlation_id: "0197c42e-2d29-7798-b684-03837c2f930c"
2025-07-01T04:11:03Z app[d8d9ee6f123458] bom [info]  2025-07-01T04:11:03.197613Z ERROR estate_fe::ssr_booking::booking_handler: Booking failed: Hotel service book_room failed: ProviderError(ProviderErrorDetails { provider_name: LiteApi, api_error: JsonParseFailed("path: data.cancellationPolicies.hotelRemarks - inner: invalid type: sequence, expected a string at line 1 column 869 "), error_step: HotelBookRoom })

```


Next steps
- experience with stable diffusion - hands on way to experiment
- challenges you are facing -- can hlep alleviate this 
- veo - pub sub / api / load balancing / query expansion 

2025-07-24

cannot get the `get_booking_by_id` from bakend 

1. try to deploy on estate staging - and see if you can get data there?
	- [x] dockerfile setup for staging
	- [ ] setup github actions?? 
	- [ ] on production, setup canister_ids.json properly?? in the ci 

2. use the sns canister in prod
	- [ ] find the principal of the controller in prod and staging 
	- [ ] add that to the backend and raise a proposal 
	- [ ] see if that passes and you can then store data in canister
	- [ ] 