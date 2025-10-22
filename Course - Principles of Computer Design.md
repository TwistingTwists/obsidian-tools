---
tags:
  - course
---
Link Sources

https://web.stanford.edu/class/cs110/

https://web.stanford.edu/class/archive/cs/cs110/cs110.1224/examples/

Accompanying book read todo

# Ch 009 - Atomicity

### All-or-Nothing Atomicity for Failure Masking

All-or-nothing atomicity ensures that any given operation or sequence of operations is performed in its entirety or not at all. This design strategy is essential for masking failures that may occur during the execution of a program. If a failure happens midway through a set of operations, the system is returned to its state before the operations began, as if nothing happened. This prevents the system from being left in an inconsistent or corrupted state.

A classic example is a ***financial transaction***. When transferring money from one account to another, two actions must occur: debiting the source account and crediting the destination account. If the system crashes after the debit but before the credit, without all-or-nothing atomicity, the money would be lost. With this principle, the incomplete transaction would be rolled back, and the source account would be restored to its original balance.

Techniques used to achieve all-or-nothing atomicity often involve mechanisms like ***transaction logs***. Before any changes are made, the intended operations are recorded in a log. If a failure occurs, this log can be used to undo any partial changes. Another approach is to work on a copy of the data, and only when all operations are successfully completed is the original data updated.

### Before-or-After Atomicity for Coordinating Concurrency

Before-or-after atomicity addresses the challenges of coordinating concurrent activities, where multiple processes or threads access shared resources simultaneously. This design principle guarantees that from the perspective of any one thread, the actions of other concurrent threads appear to have occurred either entirely before or entirely after its own actions. This creates the ***illusion of a serial execution***, even though the activities may be happening in an interleaved manner.

Consider two users trying to book the last available seat on a flight at the same time. Without before-or-after atomicity, both users might see the seat as available and proceed to book it, leading to an overbooking. With this principle, the system ensures that one user's entire booking process (checking availability, selecting the seat, and confirming the booking) is treated as a single, atomic step that completes before the other user's process can begin.

This is typically achieved through mechanisms such as ***locks.*** A thread can acquire a lock on a shared resource, preventing other threads from accessing it until the lock is released. This serialization of access ensures that concurrent operations do not interfere with each other, thus maintaining the consistency of the shared data. Simple serialization is a straightforward way to achieve this, though it can limit concurrency.

