## sqllite 3.7.9

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

## 6. 查找所有员工入职时候的薪水情况，给出emp_no以及salary， 并按照emp_no进行逆序
```sql
select e.emp_no, s.salary
from employees e left join salaries s 
on e.emp_no = s.emp_no
where e.hire_date = s.from_date
order by e.emp_no desc
```

## 7.查找薪水变动超过15次的员工号emp_no以及其对应的变动次数t
```sql
select emp_no, count(emp_no) as t
from salaries
group by emp_no
having count(emp_no) > 15
```

## 8.找出所有员工当前(to_date='9999-01-01')具体的薪水salary情况，对于相同的薪水只显示一次,并按照逆序显示
```sql
select distinct salary
from salaries
where to_date = '9999-01-01'
order by salary desc
```

## 9.获取所有部门当前(dept_manager.to_date='9999-01-01')manager的当前(salaries.to_date='9999-01-01')薪水情况，给出dept_no, emp_no以及salary(请注意，同一个人可能有多条薪水情况记录)
```sql
select d.dept_no, d.emp_no, s.salary
from dept_manager d left join salaries s
on d.emp_no = s.emp_no
where d.to_date='9999-01-01'
and s.to_date='9999-01-01'
```

## 10. 
```sql

```
