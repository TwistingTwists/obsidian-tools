---
tags:
  - lean-4
---
The Cedar SDK consists of three core components: the evaluator, the authorizer, and the validator.

- The _evaluator_ determines the value of expressions like `resource.owner == principal`, and 
- the _authorizer_ applies the evaluator to your policies to decide whether a request should be allowed. 
- The _validator_ rules out certain classes of evaluation errors by typechecking policies when you create them. 
	- For example, in the first policy above, the validator knows that the access `resource.owner` will never result in an error because`resource` is guaranteed to be of type `Document`.

### verification guided development 

First, we create an executable formal model of each core component in Lean. The model is a highly readable prototype, and with it we prove key correctness properties using Lean’s _proof assistan_t. You construct proofs in Lean by applying _tactics_ — procedures that transform one state of the proof (known facts) into another (implied facts). Lean checks each step, preventing mistakes.

Second, we implement Cedar’s production code in Rust, optimizing for performance and usability. We confirm that the code matches the model by using _differential random testing_. This involves generating millions of random inputs and checking that both model and code produce the same output on every input. A new version of Cedar isn’t released unless its model, proofs, and differential tests are up to date.