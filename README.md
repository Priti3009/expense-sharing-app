# expense-sharing-app
This document describes the low-level design of a simplified expense sharing application similar to Splitwise.
The system allows users to create groups, add shared expenses with different split types, track balances, and settle dues.

## Objective
Design a backend system that supports:
•	Creating users and groups
•	Adding shared expenses
•	Tracking balances (who owes whom)
•	Settling dues
•	Simplifying balances

## Requirements
•	Users can create groups
•	Users can add expenses in a group
•	Supported split types:
            o	Equal split
            o	Exact amount split
            o	Percentage split
•	Each user can view:
            o	How much they owe
            o	How much others owe them
•	Balances should be simplified

## Core Entities
- User
- Group
- Expense
- Split
- BalanceManager


