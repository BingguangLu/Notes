# 3 Transactions

## 3.1 Database Transactions

In relational DBs unit of work is not one SQL query at a time

Transaction is a unit of work in a database which may have multiple SQL queries bundled together

- A transaction can have many number and type of operations in it
- Either all happens as a whold or not, which is the most important factor that makes it an atomic unit of execution
- Transactions ideally have four properties, commonly known as ACID properties

## 3.2 ACID Properties

ACID:

- Atomicity
- Consistency
- Isolation
- Durability

## 3.3 Type of actions

- Unprotected actions: no ACID property, easy to run with weaker guarantees
- Protected actions: these actions are not externalised before they are completely done. These actions are controlled and can be rolled back if required. These ACID properties guaranteed.
- Real actions: these are real physical actions once performed cannot be undone. In many situations, atomicity is not possible at all (e.g., printing two reports as a single atomic action). So if you hook you DB to a real world action, beware!
