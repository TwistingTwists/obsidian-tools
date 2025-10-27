---
tags:
  - category-theory
  - course
---


You've asked for a practical-first approach, which is an excellent way to learn. This learning path is heavily inspired by books like **"Category Theory for Programmers" by Bartosz Milewski**, which uses programming (like in Haskell or Scala) as a concrete playground, and **"Basic Category Theory" by Tom Leinster** which provides a concise and modern introduction. We'll then build up to the more formal, classic ideas found in **"Categories for the Working Mathematician" by Saunders Mac Lane** and **"Category Theory in Context" by Emily Riehl**.

Here is a detailed index designed to get you using the ideas first, and then formalizing them.

---

## A Practical-First Index to Category Theory

### Part 1: The Practical Toolkit (What Can I _Do_ With This?)

This section focuses on the "what" and "how." We'll treat category theory as a design pattern language for building composable systems, a perspective championed by **Bartosz Milewski**.

---

#### **Module 1: The Essence — Thinking in Composition** 💭

- **Big Idea:** The most important thing in the universe is how things _compose_. Instead of looking _inside_ an object, we only care about how it _connects_ to other objects.
    
- **Practical Takeaway:** How to model systems as "black boxes" and "arrows." This is the core of modular, testable, and composable software design.
    
- **Key Concepts:**
    
    - **Objects (Things):** We don't care what they are. In programming, think of these as **types** (e.g., `Int`, `String`, `User`).
        
    - **Morphisms (Arrows):** We care a lot about these. They are relationships, transformations, or functions. In programming, these are just **functions** (e.g., `String -> Int`).
        
    - **Composition:** The star of the show. If you have an arrow from A to B (`f`) and an arrow from B to C (`g`), you _must_ have an arrow from A to C (`g ∘ f`). This is just **function composition**.
        
    - **Identity:** For every object A, there is a "do nothing" arrow from A to A. This is the **identity function** (`id`).
        
- **Book Reference:** This is the starting point for all books. Milewski (Ch. 1-2) frames this as "Types and Functions" becoming "Objects and Morphisms."
    

---

#### **Module 2: Building Blocks — Universal Constructions (Part 1)** 🧱

- **Big Idea:** We can describe common patterns for _combining_ or _choosing_ things in a totally abstract way.
    
- **Practical Takeaway:** Understanding familiar programming concepts like **tuples/structs**, **unions/enums**, and the `void` type in a new, powerful light.
    
- **Key Concepts:**
    
    - **Terminal Object (The "One"):** An object that _everything_ has an arrow to.
        
        - **Practical Example:** In programming types, this is the **`unit`** type (also called `()` or `void` in some contexts). There is only one function you can write _to_ `unit` from any other type.
            
    - **Initial Object (The "Zero"):** An object that has an arrow _to_ everything.
        
        - **Practical Example:** In programming types, this is the **`Void`** or **`Bottom`** type (a type with no values). You can write a function _from_ `Void` to any other type (because it will never be called).
            
    - **Products (The "And"):** The best way to have "a thing A _and_ a thing B."
        
        - **Practical Example:** A **tuple** `(A, B)` or a **struct/record** `{ a: A, b: B }`. The "universal" part is that it comes with two projection functions (`fst: (A, B) -> A` and `snd: (A, B) -> B`) and it's the _only_ thing you need to create pairs.
            
    - **Coproducts (The "Or"):** The best way to have "a thing A _or_ a thing B."
        
        - **Practical Example:** An **enum** or **tagged union** `Either<A, B>`. The "universal" part is that it gives you a way to "inject" an `A` or a `B` into the new type, and you handle it with a `case` or `switch`.
            
- **Book Reference:** Milewski (Ch. 5-6) covers these as "Products and Coproducts" and "Simple Algebraic Data Types." Leinster (Ch. 5) and Riehl (Ch. 3) introduce them as examples of a more general concept called **Limits** and **Colimits**.
    

---

#### **Module 3: The Workhorse — Functors** 🚀

- **Big Idea:** A way to map one category (system) to another category (system) _while preserving the structure_ (composition and identity).
    
- **Practical Takeaway:** A **Functor** is any "container" or "context" that you can `map` over. This is the single most common abstraction in functional programming.
    
- **Key Concepts:**
    
    - **What is a Functor?** It's a mapping of _objects_ (e.g., `T` -> `List<T>`) AND a mapping of _morphisms_ (e.g., a function `f: T -> U` becomes `map(f): List<T> -> List<U>`).
        
    - **The Functor Laws:** `map(id) == id` and `map(g ∘ f) == map(g) ∘ map(f)`. These just mean `map` doesn't break composition.
        
    - **Practical Examples:** `List`, `Optional`/`Maybe`, `Promise`/`Future`, `Task`, `Reader` (the "dependency-injection" context). `map` for `List` is just the standard `map`. `map` for `Optional` only runs the function if the value exists.
        
- **Book Reference:** This is a core concept everywhere. Milewski (Ch. 7-8), Leinster (Ch. 1.2), Riehl (Ch. 1.3), Mac Lane (Ch. I.3).
    

---

#### **Module 4: The Bridge — Natural Transformations** 🌉

- **Big Idea:** A way to transform one Functor into another Functor _naturally_ (i.e., without messing up the contents).
    
- **Practical Takeaway:** A "natural transformation" is a function that converts one container type to another (e.g., `List<T>` to `Optional<T>`) for _any_ possible type `T` inside, without looking at the `T` values themselves.
    
- **Key Concepts:**
    
    - **Polymorphic Functions:** A natural transformation is just a polymorphic function `alpha: forall T. FunctorF<T> -> FunctorG<T>`.
        
    - **Example:** A `safe_head` function `List<T> -> Optional<T>`. This works for a `List<Int>`, a `List<String>`, or a `List<User>`—it's "natural" with respect to `T`.
        
    - **The "Naturality Square":** The diagram that proves your transformation is lawful. It's the "theory" part, but the _intuition_ is just "it doesn't matter if I `map` my function _before_ or _after_ I change the container."
        
- **Book Reference:** Milewski (Ch. 9), Leinster (Ch. 1.3), Riehl (Ch. 1.4), Mac Lane (Ch. I.4).
    

---

#### **Module 5: The Power-Up — Monads** ✨

- **Big Idea:** A monad is a "programmable semicolon." It's a pattern for composing functions that have "effects" (like I/O, error handling, or asynchronicity) without tangling your core logic.
    
- **Practical Takeaway:** How to write clean, sequential code where each step might fail (like `Optional`), produce multiple results (like `List`), or be asynchronous (like `Promise`). This is what `flatMap` (or `>>=` / `bind`) is for.
    
- **Key Concepts:**
    
    - **Not Just a Functor:** A Monad is a Functor you can also `flatMap` (or `join`) with.
        
    - **`flatMap` / `bind`:** The key operation. Takes `A -> Monad<B>`. It lets you chain operations where each step returns a value _in the monadic context_.
        
    - **`return` / `pure`:** The function `A -> Monad<A>`. It lifts a simple value into the context.
        
    - **Practical Examples:** `Optional` (for chaining failable computations), `List` (for non-deterministic "search" computations), `Promise` (for chaining async computations).
        
- **Book Reference:** Milewski (Ch. 10 in Part 2, but it's a primary goal of his book). Riehl (Ch. 5), Mac Lane (Ch. VI).
    

---

### Part 2: The Theoretical Foundation (Why Does This _Work_?)

This section focuses on the "why." We'll see how the practical patterns from Part 1 are all just specific examples of a few, deep, powerful ideas. This is the world of **Leinster**, **Riehl**, and **Mac Lane**.

---

#### **Module 6: The Grand Unifier — Universal Properties** 👑

- **Big Idea:** Instead of defining a thing by _what it is_ (its internal data), we define it by _what it does_ (its unique relationship with everything else).
    
- **Theoretical Takeaway:** This is the _real_ definition for Products, Coproducts, Limits, and Colimits. An object is "universal" if it is the _best_ (initial or terminal) solution to a specific mapping problem.
    
- **Example (Product):** The `(A, B)` tuple is the _universal_ object `P` that comes with maps `p1: P -> A` and `p2: P -> B`, such that _any other_ object `X` with maps `x1: X -> A` and `x2: X -> B` can be mapped to `P` via a _unique_ arrow `m: X -> P`. This `m` is just the `(x1, x2)` pairing function.
    
- **Book Reference:** This idea is central to _all_ modern category theory. Leinster (Ch. 0-2) and Riehl (Ch. 2) introduce it right at the beginning.
    

---

#### **Module 7: The Master Pattern — Limits and Colimits** 🎯

- **Big Idea:** **Limits** and **Colimits** are the generalization of _all_ "combining" patterns.
    
- **Theoretical Takeaway:**
    
    - **Limits:** Generalize "all-of-these-must-hold" patterns.
        
        - **Products** (from Module 2) are the limit of a diagram with just two objects.
            
        - **Pullbacks:** A universal way to find "where two things agree." (e.g., in databases, a JOIN).
            
        - **Terminal Objects** are the limit of an empty diagram.
            
    - **Colimits:** Generalize "any-of-these-can-hold" patterns.
        
        - **Coproducts** (from Module 2) are the colimit of a diagram with just two objects.
            
        - **Pushouts:** A universal way to "glue" two things together along a common part.
            
        - **Initial Objects** are the colimit of an empty diagram.
            
- **Book Reference:** This is the core technical machinery. Leinster (Ch. 5), Riehl (Ch. 3), Mac Lane (Ch. III).
    

---

#### **Module 8: The Secret Weapon — The Yoneda Lemma** 🤯

- **Big Idea:** An object is _completely and totally_ determined by all the arrows _into_ it (or _out of_ it). You can know everything about an object just by "probing" it with all other objects.
    
- **Theoretical Takeaway:** This is the most profound result in basic category theory. It formally states that "the outside of an object _is_ its inside." It connects objects (things) with functors (mappings of systems).
    
- **Practical Consequence:** In programming, this lemma is the foundation for `Representable` functors and justifies why `(A -> B)` is the best way to model functions. It's the "why" behind the saying "my type is defined by its API."
    
- **Book Reference:** The first "summit" of any category theory book. Milewski (Ch. 12-13), Leinster (Ch. 4.2), Riehl (Ch. 2.2), Mac Lane (Ch. III.2).
    

---

#### **Module 9: The Ultimate Relationship — Adjunctions** 🤝

- **Big Idea:** An **Adjunction** is a deep, symmetrical, and efficient relationship between two functors (and thus, two categories). It's a "doing and undoing" relationship.
    
- **Theoretical Takeaway:** This is the concept that unifies almost everything else. Products, limits, monads, and even logic itself (`AND` and `Implication`) are all expressions of adjunctions.
    
- **Key Concept (The Hom-Set Definition):** Two functors, `F: C -> D` and `G: D -> C`, are adjoint (`F ⊣ G`) if there's a _natural_ one-to-one correspondence between arrows: `Hom_D(F(C), D) ≅ Hom_C(C, G(D))`.
    
- **Practical Example:** The "Product" functor `Δ: A -> (A, A)` and the "Coproduct" functor `∇: (B, B) -> B` are _not_ adjoint. But `(A, -)` (a functor that takes `X` to `(A, X)`) _is_ adjoint to `(A -> -)` (a functor that takes `Y` to `(A -> Y)`). This is the "currying/uncurrying" you see in programming: `(A, X) -> Y` is the same as `X -> (A -> Y)`.
    
- **Book Reference:** The second "summit" of category theory. Leinster (Ch. 2), Riehl (Ch. 4), Mac Lane (Ch. IV).
    

---

### Part 3: Where to Go Next?

Once you have this foundation, you can branch out:

- **Monads (Again):** Now you can understand them properly as "monoids in the category of endofunctors." (Riehl Ch. 5, Mac Lane Ch. VI).
    
- **Applied Category Theory:** Using these tools to model real-world systems in biology, network theory, physics, and databases (e.g., **"Seven Sketches in Compositionality"** by Fong and Spivak).
    
- **Higher Category Theory:** What if the "arrows between arrows" (Natural Transformations) also had arrows between them? This leads to 2-categories and $\infty$-categories.


----


My "black box" $P$ _claims_ to be a pair of $(A, B)$. How do you, the user of my box, _verify_ this claim without opening it? You can only use arrows.

What good is a pair if you can't get the pieces back out?

---

## Hint 1: What Can You _Do_ With a Pair?

If I give you a `(String, Int)` tuple, say `("Alice", 30)`, what are the two _most basic operations_ you can perform on it? You can...

1. Get the `String` part ("Alice").
    
2. Get the `Int` part (30).
    

These are just functions!

- `fst: (String, Int) -> String`
    
- `snd: (String, Int) -> Int`
    

So, for our abstract object $P$ to _behave_ like a pair of $A$ and $B$, it _must_ provide us with two arrows _out_ of it:

- $p_A: P \rightarrow A$
    
- $p_B: P \rightarrow B$
    

Let's call these "projections."

---

## Hint 2: The "Is It _Just_ a Pair?" Pain Point

Okay, so we have $P$ with its two projection arrows. Is that _enough_?

Let's consider another object. What about a `User` object, which has a `name: String` and `age: Int`?

- Our `User` object ($X$) also has an arrow to `String` (let's call it $f = \text{getName}$).
    
- It also has an arrow to `Int` (let's call it $g = \text{getAge}$).
    

So, $X$ _also_ satisfies our rule. But a `User` object isn't _just_ a `(String, Int)` pair. It might have _other_ data, like `userId`, `lastLogin`, etc.

The pair $P$ is meant to be the _most basic, universal_ object that just holds $A$ and $B$ and _nothing else_. How do we use arrows to state this "nothing else" property?

---

## Hint 3: The "Universal" Test

Let's use our `User` object ($X$) to "test" our $P$ object.

If $P$ is truly the "universal pair," we should be able to _losslessly_ convert our `User` object _into_ $P$. That is, we should be able to make a `(String, Int)` tuple _from_ a `User`.

How would you write that function in code?

func convert(user: User) -> (String, Int) { ... }

What would go inside that function? You'd use the exact two functions $X$ came with:

return (user.getName(), user.getAge())

In our arrow language, this `convert` function is just an arrow $m: X \rightarrow P$.

Here's the key: this arrow $m$ is _uniquely_ determined. You have _no other choice_. To build the $P$, you _must_ use the $f$ (`getName`) and $g$ (`getAge`) arrows that $X$ provided.

This gives us our "Aha!" moment.

---

## The "Aha!" Moment: Products (The "AND" Pattern)

You've just discovered the **Universal Property of the Product**.

An object $P$ is the **Product** of $A$ and $B$ if:

1. It has two projection arrows, $p_A: P \rightarrow A$ and $p_B: P \rightarrow B$.
    
2. For _any other_ object $X$ that _also_ has two arrows $f: X \rightarrow A$ and $g: X \rightarrow B$, there exists one and _only one_ (a **unique**) arrow $m: X \rightarrow P$ such that the diagram "commutes."
    

"Commutes" is a fancy word for "it doesn't matter which path you take."

- If you go from $X$ to $P$ and _then_ to $A$ (i.e., $p_A \circ m$), you get the _exact same result_ as just going from $X$ to $A$ (i.e., $f$). So, $p_A \circ m = f$.
    
- Similarly, $p_B \circ m = g$.
    

This unique $m$ is the "pairing" function. The fact that it's _unique_ is what guarantees $P$ holds _only_ $A$ and $B$ and nothing else. It's the most essential, minimal "AND" object.

**Practical Takeaway:** You just defined a `tuple` or `struct` using only arrows.

---

---

## Module 2 (Continued): The "OR" Pattern

Great! We've solved the "AND" pattern. Now for the next pain point.

The "OR" Pain Point:

How do we model data that can be "this or that"?

- A function that can return _either_ a `User` (on success) _or_ an `ErrorMessage` (on failure).
    
- A list of items on a website that can be _either_ a `Book` _or_ a `Movie`.
    

In programming, this is an `enum` or a `tagged union` (like `Either<A, B>`). Let's call our abstract "OR" object $C$.

How can we define $C$ using _only_ arrows?

---

## Hint 1: The Other Direction

With our Product $P$, we had arrows _out_ of it ($p_A: P \rightarrow A$ and $p_B: P \rightarrow B$).

What about our "OR" object $C$? Can we have an arrow $C \rightarrow A$?

- This would be a function `func getA(c: C) -> A`.
    
- What does this function do if the $C$ it receives is _actually_ holding a $B$? It has to crash or return `null`. This isn't a "total function" and isn't very "categorical."
    

This suggests arrows _out_ aren't the right way to think about $C$.

- **Question 1:** Instead of getting data _out_ of $C$, what's the most basic thing you can _do_ with an `Either<A, B>` type? How do you _create_ one?
    

---

## Hint 2: Injections and "Case Handling"

You're right! You don't "get" from an `Either`, you "put in."

1. You can take a simple $A$ and "wrap" it into $C$. (e.g., `Left("Error")`).
    
2. You can take a simple $B$ and "wrap" it into $C$. (e.g., `Right(user)`).
    

These are just arrows _into_ $C$:

- $i_A: A \rightarrow C$
    
- $i_B: B \rightarrow C$
    

Let's call these "injections."

But just like before, this isn't enough. (I could have $A = \text{Int}$ and $B = \text{String}$ and $C = \text{String}$, with $i_A = \text{toString}$ and $i_B = \text{id}$. This isn't an "OR" type!)

**The Real Pain Point:** What's the _other_ thing you do with an `Either`? You don't just _create_ them, you _use_ them. How do you _use_ a value of type `Either<A, B>`?

- **Question 2:** Think about a `switch` or `case` statement. You're writing a _single_ function, let's say `handle(c: C) -> Y`, where $Y$ is some result type (like a `String` to display to the user). What _two pieces of logic_ must you provide to make this `handle` function work?
    

---

## The "Aha!" Moment: Coproducts (The "OR" Pattern)

You got it. To write your `handle: C \rightarrow Y` function, you _must_ provide:

1. A function $f: A \rightarrow Y$ (to handle the "A" case).
    
2. A function $g: B \rightarrow Y$ (to handle the "B" case).
    

Your `handle` function $m$ is just the `switch` statement that _combines_ $f$ and $g$. And again, this $m$ is _uniquely_ determined. You have no other choice but to use $f$ and $g$.

This is the **Universal Property of the Coproduct**.

An object $C$ is the **Coproduct** of $A$ and $B$ if:

1. It has two injection arrows, $i_A: A \rightarrow C$ and $i_B: B \rightarrow C$.
    
2. For _any other_ object $Y$ that _also_ has two arrows $f: A \rightarrow Y$ and $g: B \rightarrow Y$, there exists one and _only one_ (a **unique**) arrow $m: C \rightarrow Y$ such that the diagram commutes.
    

"Commutes" here means:

- If you "wrap" an $A$ into $C$ and _then_ handle it (i.e., $m \circ i_A$), you get the _exact same result_ as just running the $A$-handler $f$. So, $m \circ i_A = f$.
    
- Similarly, $m \circ i_B = g$.
    

**Practical Takeaway:** You just defined an `enum` or `Either` type using only arrows. Notice how this is the _exact same diagram_ as the Product, but with **all the arrows flipped**? This deep, beautiful idea is called **duality**.

---

---

## Module 2 (Final Part): The "Zero" and "One"

The "Minimalist" Pain Point:

We've handled "AND" (Product) and "OR" (Coproduct) for two objects.

What about the simplest cases:

- What is the "AND" of _zero_ objects? (A product of nothing)
    
- What is the "OR" of _zero_ objects? (A coproduct of nothing)
    

---

## Hint 1: The "Unit" Type `()`

Let's think about the "AND" of zero objects. In programming, this is a struct with no fields: struct {}. Or a tuple with no elements: ().

We call this the Unit type.

How many distinct values can this `()` type have?

- `String` has "a", "b", ...
    
- `Int` has 1, 2, ...
    
- `Bool` has `true`, `false`.
    
- `()` has... just one value: `()`.
    

Let's apply our **Product** definition to a set of _zero_ objects.

- The object $P$ (let's call it $T$ for "Terminal") is the product.
    
- The "list" of projection arrows (like $p_A, p_B$) is empty.
    
- The rule says: For _any other_ object $X$, there must be a _unique_ arrow $m: X \rightarrow T$.
    
- **Question 3:** In programming, what is the type $T$ that _every other type_ has a unique function _to_?
    
- Think: `func f(s: String) -> T` and `func g(i: Int) -> T`. If there's only _one_ way to write these functions, what must $T$ be?
    

---

## The "Aha!" Moment: Terminal Object (The "One")

It's the **Unit** type `()`!

- `func f(s: String) -> () { return () }`
    
- `func g(i: Int) -> () { return () }`
    

There is _only one_ possible implementation for these functions, because there is _only one_ value of type `()` to return.

This object is called the **Terminal Object**. It's the "Product of zero things." It's an object that _everything_ has a single, unique arrow _to_.

---

## Hint 2: The "Void" Type

Now let's do the "OR" of zero objects. In programming, this is an enum with no cases: enum Never {}.

We call this the Void type (or Bottom, or Never).

How many distinct values can this `Void` type have?

- `Bool` has `true`, `false`.
    
- `()` has `()`.
    
- `Void` has... _zero_ values. You can _never_ create an instance of this type.
    

Let's apply our **Coproduct** definition to a set of _zero_ objects.

- The object $C$ (let's call it $I$ for "Initial") is the coproduct.
    
- The "list" of injection arrows (like $i_A, i_B$) is empty.
    
- The rule says: For _any other_ object $Y$, there must be a _unique_ arrow $m: I \rightarrow Y$.
    
- **Question 4:** In programming, what is the type $I$ that has a unique function _to every other type_?
    
- Think: `func f(i: I) -> String` and `func g(i: I) -> User`. How can you write a function that _promises_ to return a `String` when you give it _nothing_?
    

---

## The "Aha!" Moment: Initial Object (The "Zero")

It's the **Void** type!

- `func f(v: Void) -> String { ...what??... }`
    

This function can be "written" (it type-checks) precisely _because it can never be called_. Since you can never create a `Void` value $v$, the function's body will never run. This is called "vacuous truth."

This object is called the **Initial Object**. It's the "Coproduct of zero things." It's an object that has a single, unique arrow _to everything else_.

---

---

## Painting the Bigger Picture (Setting up Module 3)

Wow! Look what you've done.

- You re-discovered **functions** ($f: A \rightarrow B$) and **composition** ($g \circ f$).
    
- You defined **structs/tuples** (Products) using only arrows.
    
- You defined **enums/Either** (Coproducts) using only arrows.
    
- You defined **`()`** (Terminal Object) and **`Void`** (Initial Object) using only arrows.
    

You've just described the entire "Algebraic Data Type" system used in languages like Swift, Rust, and Haskell, _without ever "looking inside" a type_.

The Next "Pain Point" (for Module 3):

So far, all our arrows have been simple functions, $A \rightarrow B$.

But in real programming, things are more complex. We don't just have `String`. We have `List<String>`. We don't just have `User`. We have `Optional<User>`. We don't just have `Int`. We have `Promise<Int>`.

We have these "containers" or "contexts."

Now, we have a new problem.

- We know how to compose $f: A \rightarrow B$ and $g: B \rightarrow C$.
    
- But what if we have a function $f: A \rightarrow B$ and a _container_ of $B$'s, like `List<B>`?
    
- Or what if we have a function $f: A \rightarrow B$ and we want to apply it to `List<A>` to get a `List<B>`?
    
- **Guiding Question for Next Time:** You have a function $f: \text{String} \rightarrow \text{Int}$ (like `strlen`). You have a `List<String>`: `["a", "b", "c"]`. You want to get a `List<Int>`: `[1, 1, 1]`.
    
    - What is the _generic_ operation you're using here?
        
    - This operation (let's call it `map`) _lifts_ your simple $f: A \rightarrow B$ into a new arrow: `map(f): List<A> \rightarrow List<B>`.
        
    - What _rules_ must this `map` operation follow to be "sane" and not break our rules of **composition** and **identity**?