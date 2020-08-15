# Leetcode - SQL

## 175 Combine Two Tables
```sql
select FirstName, LastName, City, State
From Person p
LEFT JOIN Address a
on p.PersonId =a.PersonId
```

## 181 Employees Earning More Than Their Managers
```sql
select a.name as Employee
from Employee a, Employee b 
where a.ManagerId = b.Id
and a.Salary > b.Salary
```
