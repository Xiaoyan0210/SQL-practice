# Leetcode - SQL

## 175 Combine Two Tables
```sql
select FirstName, LastName, City, State
From Person p
LEFT JOIN Address a
on p.PersonId =a.PersonId
```
