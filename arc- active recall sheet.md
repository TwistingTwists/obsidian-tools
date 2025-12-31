## 1. The Core Problem: Reference Cycles

• Trigger: Two objects hold `Arc` (Strong Pointers) to each other (e.g., Parent <-> Child).

  

• Result: The "Infinite Hug". Reference counts never reach 0.

  

• Consequence: Memory leak (data is never dropped).

  

## 2. The Solution: Strong vs. Weak

• `Arc` (Strong): "I own this. I keep it alive." (Ref Count > 0).

  

• `Weak` (Non-owning): "I see this, but I don't own it." (Does not increase Ref Count).

  

## 3. The Mechanics

The `upgrade()` Check:

  

• Returns `Some(Arc)`: The object is alive. You have temporary ownership. Safe to use.

  

• Returns `None`: The object was dropped. Do not access.

  

## 4. Key Patterns & Use Cases

## 5. Mental Model: The Radio Station

• DJ (`Arc`): Keeps the station on air.

  

• Listener (`Weak`): Has the phone number.

  

• Calling (`upgrade`): You dial the number.

  

  • Someone answers (`Some`) -> Station is live.

  

  • Dead air (`None`) -> Station is gone.