# expense-sharing-app
This document describes the low-level design of a simplified expense sharing application similar to Splitwise.
The system allows users to create groups, add shared expenses with different split types, track balances, and settle dues.

## Objective
Design a backend system that supports:
-	Creating users and groups
-	Adding shared expenses
-	Tracking balances (who owes whom)
-	Settling dues
-	Simplifying balances

## Requirements
-	Users can create groups
-	Users can add expenses in a group
-	Supported split types:
            o	Equal split
            o	Exact amount split
            o	Percentage split
-	Each user can view:
            o	How much they owe
            o	How much others owe them
-	Balances should be simplified

## Core Entities
- User
- Group
- Expense
- Split
- BalanceManager
  
## Class Diagram

![Class Diagram](./class-diagram/class-diagram.png)


## Entity Design

### User
- userId
- name
- email

### Group
- groupId
- name
- members : List<User>

### Expense
- expenseId
- description
- totalAmount
- paidBy : User
- splitType : SplitType
- splits : List<Split>

### Split
- user : User
- amount : Double


## Balance Tracking
The system tracks net dues between users using a bidirectional balance map.

### Data Structure - 

           Map<User, Map<User, Double>> balances

### Interpretation -

          balances[A][B] = X

- X > 0 → User B owes A amount X
- X < 0 → User A owes B amount |X|
- X = 0 → No dues between users
  
This structure represents both payables and receivables clearly.


## Split Types

1. Equal Split
Total amount is divided equally among all users.

2. Exact Split
Sum of all split amounts must equal total expense amount.

3. Percentage Split
Sum of all percentages must be 100.

Each user’s share is calculated accordingly.

## Expense Addition Flow
1. Expense is created
2. Expense is split based on split type
3. Payer is credited
4. Other users are debited
5. Balances are updated

## Balance Simplification

Objective - Minimize the number of settlement transactions while preserving net balances.

Simplification Steps

1. Compute net balance for each user:
   
       netBalance[user] = sum(balances[user][*])
   
- netBalance > 0 → user should receive money
- netBalance < 0 → user needs to pay money

3. Divide users into:
    - Creditors
    - Debtors

4. Match debtors to creditors greedily:
    -  Settle minimum possible amount
    -  Reduce both balances
    -  Continue until all balances become zero

## Settlement
When a user settles dues, the balance between users
is reduced and marked as settled.


## Conclusion
This design provides a clean and extensible low-level
design for an expense sharing application with simplified balances.








