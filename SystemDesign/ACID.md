---
tags:
  - system_design
---
A transaction is a collection of instructions. To maintain the integrity of a database, all transactions must obey ACID properties. **ACID** is an acronym for  
_atomicity_, _consistency_, _isolation_, and _durability_. Let’s go over each of these properties.

# Atomicity 
The transaction object is the smallest unit (the atom) of a transaction. Either all instructions of a transaction execute, or none of them do. 
E.g. if Alice sends Bob $20, to maintain atomicity, both Alice's table should show a decrease of -20 and Bob's table an increase of 20 to be atomic, if one is not possible, the transaction should not occur.

# Consistency 
If a database is initially in a consistent state, it should seek to remain in that consistent state. If the database is in an invalid state, it should be rolled back.
E.g. if Alice has $20 in her account, and gives Bob $30, the system should not allow the transaction to go through and leave Alice with a balance of -10, it should instead rollback to the last valid state and undo the transaction.

# Isolation
Transactions should not be able to affect one another, and should all "seem" to be executed alone.
E.g. A transaction involves checking if a seat is free, and if it is books the seat. If two people attempt to book the seat at the same time, one transaction should go first before the other one. We want to avoid the scenario where transaction 1 checks the seat, and then before it buys, transaction 2 checks the seat and also thinks it is free to buy, which would avoid in 2 people being booked to the same seat.

# Durability
Transactions should be permanent and once stored, should survive system failure.
E.g. If I deposit $100 in my bank account right before a system crash, as long as the transaction was entered in the database, my account should reflect the added money.