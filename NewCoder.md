## 1. 查找最晚入职员工的所有信息

```sql
select *
from employees
where hire_date in (select max(hire_date) from employees)
```

## 2. 查找入职员工时间排名倒数第三的员工所有信息
### 需要考虑可能有多个员工入职时间相同
```sql
select *
from employees
where hire_date in (select distinct hire_date from employees order by hire_date desc limit 2,1)
```

## 3. 查找各个部门当前领导当前薪水详情以及其对应部门编号
```sql
elect s.emp_no, s.salary, s.from_date, s.to_date, d.dept_no
from salaries s, dept_manager d 
where s.emp_no = d.emp_no
and d.to_date = '9999-01-01'
and s.to_date = '9999-01-01'
```

## 4. 查找所有已经分配部门的员工的last_name和first_name以及dept_no
```sql
select e.last_name, e.first_name, d.dept_no
from employees e, dept_emp d 
where e.emp_no = d.emp_no
```

## 5. 查找所有员工的last_name和first_name以及对应部门编号dept_no，也包括暂时没有分配具体部门的员工
```sql
select e.last_name, e.first_name, d.dept_no
from employees e left join dept_emp d
on d.emp_no = e.emp_no
```

## 6.
