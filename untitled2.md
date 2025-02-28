




in the jsonish folder, add support for json-patch. 

use this crate https://crates.io/crates/json-patch

here are the docs
```
Create and patch document using JSON Patch:

#[macro_use]
use json_patch::{Patch, patch};
use serde_json::{from_value, json};

let mut doc = json!([
    { "name": "Andrew" },
    { "name": "Maxim" }
]);

let p: Patch = from_value(json!([
  { "op": "test", "path": "/0/name", "value": "Andrew" },
  { "op": "add", "path": "/0/happy", "value": true }
])).unwrap();

patch(&mut doc, &p).unwrap();
assert_eq!(doc, json!([
  { "name": "Andrew", "happy": true },
  { "name": "Maxim" }
]));
Create and patch document using JSON Merge Patch:

#[macro_use]
use json_patch::merge;
use serde_json::json;

let mut doc = json!({
  "title": "Goodbye!",
  "author" : {
    "givenName" : "John",
    "familyName" : "Doe"
  },
  "tags":[ "example", "sample" ],
  "content": "This will be unchanged"
});

let patch = json!({
  "title": "Hello!",
  "phoneNumber": "+01-123-456-7890",
  "author": {
    "familyName": null
  },
  "tags": [ "example" ]
});

merge(&mut doc, &patch);
assert_eq!(doc, json!({
  "title": "Hello!",
  "author" : {
    "givenName" : "John"
  },
  "tags": [ "example" ],
  "content": "This will be unchanged",
  "phoneNumber": "+01-123-456-7890"
}));
```

so, idea






---
DH = average of 10 years 

PB ratio = indicator of book value

long term of PB ratio does not change a lot 
Long term PB ratio (moving average) =  PB ratio will converge to that 
difference between current and average   = log(current / avg)

For index, book value is slow moving (5 yr ish) ,but price is volatile

PE ratio = price / earnings
PEG ratio =
(earnings is volatile number)


Techincals = regression to the mean and mean exists
stock is expected to RISE. The moving average has to rise. this increasing price has to be adjusted to something.

Book value = more stable indicator (book value) in PE ratio


----


- Roadmap -- compilation issues on rust??
	- BIG goals?
	- Developer docs --- HOW to guides are less.
		- if a company is using your product , what would be the most jaw dropping example of your product TODAY 
	- kapa.ai --- for generating relevant answers from docs
	- testing suite -- boundary studio -- sqlite database for all trials -- test suite for evals stuff 
- Two way binding :
	- proc-macro <-> baml file 
	- clients 
		- Stream structs??
		- Validators in rust 
	- Tool calling?



Relevant experience 

- instructor_ex 
- Rust api clients - stream + chat 
- grafana + victoria metrics + prometheous + flink
