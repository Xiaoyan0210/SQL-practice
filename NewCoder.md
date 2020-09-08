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

## 7. 查找薪水变动超过15次的员工号emp_no以及其对应的变动次数t
```sql
select emp_no, count(emp_no) as t
from salaries
group by emp_no
having count(emp_no) > 15
```

## 8. 找出所有员工当前(to_date='9999-01-01')具体的薪水salary情况，对于相同的薪水只显示一次,并按照逆序显示
```sql
select distinct salary
from salaries
where to_date = '9999-01-01'
order by salary desc
```

## 9. 获取所有部门当前(dept_manager.to_date='9999-01-01')manager的当前(salaries.to_date='9999-01-01')薪水情况，给出dept_no, emp_no以及salary(请注意，同一个人可能有多条薪水情况记录)
```sql
select d.dept_no, d.emp_no, s.salary
from dept_manager d left join salaries s
on d.emp_no = s.emp_no
where d.to_date='9999-01-01'
and s.to_date='9999-01-01'
```

## 10. 获取所有非manager的员工emp_no
```sql
select emp_no
from employees
where emp_no not in (select emp_no from dept_manager)
```

## 11. 获取所有员工当前的(dept_manager.to_date='9999-01-01')manager，如果员工是manager的话不显示(也就是如果当前的manager是自己的话结果不显示)。输出结果第一列给出当前员工的emp_no,第二列给出其manager对应的emp_no
```sql
select de.emp_no,dm.emp_no as manager_no
from dept_emp de left join dept_manager dm
on de.dept_no = dm.dept_no
where de.emp_no != dm.emp_no
and dm.to_date = '9999-01-01'
```

## ※12. 获取所有部门中当前(dept_emp.to_date = '9999-01-01')员工当前(salaries.to_date='9999-01-01')薪水最高的相关信息，给出dept_no, emp_no以及其对应的salary，按照部门升序排列。
```sql

```

## 13. 从titles表获取按照title进行分组，每组个数大于等于2，给出title以及对应的数目t。
```sql
select title, count(title) as t
from titles
group by title
having count(title) >= 2
```

## 14. 从titles表获取按照title进行分组，每组个数大于等于2，给出title以及对应的数目t。注意对于重复的emp_no进行忽略(即emp_no重复的title不计算，title对应的数目t不增加)。
```sql
select title, count(distinct emp_no) as t
from titles
group by title
having count(distinct emp_no) >= 2
```

## 15. 查找employees表所有emp_no为奇数，且last_name不为Mary(注意大小写)的员工信息，并按照hire_date逆序排列(题目不能使用mod函数)
```sql
select *
from  employees
where (emp_no %2) != 0
and last_name != 'Mary'
order by hire_date desc
```

## 16. 统计出当前(titles.to_date='9999-01-01')各个title类型对应的员工当前(salaries.to_date='9999-01-01')薪水对应的平均工资。结果给出title以及平均工资avg。
```sql
select t.title, avg(s.salary) as avg
from titles t left join salaries s 
on t.emp_no = s.emp_no
where t.to_date='9999-01-01'
and s.to_date='9999-01-01'
group by t.title
```

## 17. 获取当前（to_date='9999-01-01'）薪水第二多的员工的emp_no以及其对应的薪水salary
