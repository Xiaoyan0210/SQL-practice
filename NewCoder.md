## 1. 查找最晚入职员工的所有信息

```sql
select *
from employees
where hire_date in (select max(hire_date) from employees)
```
