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
## 1179 Reformat Department Table
```sql
select id, sum(case when month = 'Jan' then revenue end) as Jan_Revenue,
sum(case when month = 'Feb' then revenue end) as Feb_Revenue,
sum(case when month = 'Mar' then revenue end) as Mar_Revenue,
sum(case when month = 'Apr' then revenue end) as Apr_Revenue,
sum(case when month = 'May' then revenue end) as May_Revenue,
sum(case when month = 'Jun' then revenue end) as Jun_Revenue,
sum(case when month = 'Jul' then revenue end) as Jul_Revenue,
sum(case when month = 'Aug' then revenue end) as Aug_Revenue,
sum(case when month = 'Sep' then revenue end) as Sep_Revenue,
sum(case when month = 'Oct' then revenue end) as Oct_Revenue,
sum(case when month = 'Nov' then revenue end) as Nov_Revenue,
sum(case when month = 'Dec' then revenue end) as Dec_Revenue
from Department
group by id
```
## 595. Big Countries
```sql
select name, population, area
from World
where area >3000000 or population >25000000
```

## 182. Duplicate Emails
```sql
select Distinct a.Email
from Person a, Person b
where a.Email = b.Email
and a.Id != b.Id
```

## 183. Customers Who Never Order
```sql
select name as Customers
from customers
where Id not in (select CustomerId from orders)
```

## ※196. Delete Duplicate Emails
### https://leetcode.com/problems/delete-duplicate-emails/discuss/55553/Simple-Solution
```sql
DELETE p1
FROM Person p1, Person p2
WHERE p1.Email = p2.Email AND
p1.Id > p2.Id
```
