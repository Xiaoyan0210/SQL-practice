## https://www.nowcoder.com/ta/sql
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
```sql
select emp_no, salary
from salaries
where to_date='9999-01-01'
and salary in (select distinct salary from salaries order by salary desc limit 1,1)
#防止出现多个薪水最多的人 eg:有3人拿相同最高工资，若不distinct，则limit 1,1会筛选出第2个拥有最高工资的人
```

## 18. 查找当前薪水(to_date='9999-01-01')排名第二多的员工编号emp_no、薪水salary、last_name以及first_name，你可以不使用order by完成吗
```sql
select e.emp_no, max(s.salary) as salary, e.last_name, e.first_name
from employees e left join salaries s 
on e.emp_no = s.emp_no
where salary not in (select max(salary) from salaries where to_date = '9999-01-01')
and s.to_date = '9999-01-01'
```

## 19. 查找所有员工的last_name和first_name以及对应的dept_name，也包括暂时没有分配部门的员工
```sql
select e.last_name, e.first_name, de.dept_name
from employees e left join dept_emp d on e.emp_no = d.emp_no
left join departments de on d.dept_no = de.dept_no
```

## 20. 查找员工编号emp_no为10001其自入职以来的薪水salary涨幅(总共涨了多少)growth(可能有多次涨薪，没有降薪)
```sql
select (max(salary) - min(salary)) as growth
from salaries
where emp_no = '10001'
```

## 21. 
```sql


```

## 22. 统计各个部门的工资记录数，给出部门编码dept_no、部门名称dept_name以及部门在salaries表里面有多少条记录sum
```sql
select de.dept_no, dp.dept_name, count(s.salary) as sum 
from dept_emp de inner join departments dp on de.dept_no = dp.dept_no
inner join salaries s on de.emp_no = s.emp_no
group by de.dept_no
```

## 23. 


## 24. 获取所有非manager员工当前的薪水情况，给出dept_no、emp_no以及salary ，当前表示to_date='9999-01-01'
```sql
select de.dept_no, e.emp_no, s.salary
from employees e inner join dept_emp de on e.emp_no = de.emp_no
inner join salaries s on e.emp_no = s.emp_no
where e.emp_no not in (select emp_no from dept_manager)
and s.to_date = '9999-01-01'
```

## 25. 获取员工其当前的薪水比其manager当前薪水还高的相关信息，当前表示to_date='9999-01-01',结果第一列给出员工的emp_no，第二列给出其manager的manager_no，第三列给出该员工当前的薪水emp_salary,第四列给该员工对应的manager当前的薪水manager_salary
```sql
select emp.emp_no as emp_no, dep.emp_no as manager_no, emp.salary as emp_salary, dep.salary as manager_salary
from (select e.emp_no, s.salary, e.dept_no from dept_emp e inner join salaries s 
      on e.emp_no = s.emp_no where s.to_date = '9999-01-01') as emp,
(select d.emp_no, s.salary, d.dept_no from dept_manager d inner join salaries s 
 on d.emp_no = s.emp_no where s.to_date = '9999-01-01') as dep
where emp.dept_no = dep.dept_no
and emp.salary > dep.salary

```

## 26. 汇总各个部门当前员工的title类型的分配数目，即结果给出部门编号dept_no、dept_name、其部门下所有的当前(dept_emp.to_date = '9999-01-01')员工的当前(titles.to_date = '9999-01-01')title以及该类型title对应的数目count(注：因为员工可能有离职，所有dept_emp里面to_date不为'9999-01-01'就已经离职了，不计入统计，而且员工可能有晋升，所以如果titles.to_date 不为 '9999-01-01'，那么这个可能是员工之前的职位信息，也不计入统计)
```sql
select de.dept_no, de.dept_name, t.title, count(title) as title from dept_emp d left join departments de on d.dept_no = de.dept_no
left join titles t on d.emp_no = t.emp_no
where d.to_date = '9999-01-01'
and t.to_date = '9999-01-01'
group by de.dept_name, t.title
order by de.dept_no, de.dept_name
```

## 27. 给出每个员工每年薪水涨幅超过5000的员工编号emp_no、薪水变更开始日期from_date以及薪水涨幅值salary_growth，并按照salary_growth逆序排列。提示：在sqlite中获取datetime时间对应的年份函数为strftime('%Y', to_date)(数据保证每个员工的每条薪水记录to_date-from_date=1年，而且同一员工的下一条薪水记录from_data=上一条薪水记录的to_data)
```sql
select b.emp_no, b.from_date, (b.salary - a.salary) as salary_growth
from salaries as a, salaries as b 
where a.emp_no = b.emp_no
and salary_growth > 5000
and ((strftime('%Y', b.to_date) - strftime('%Y', a.to_date) = 1) 
OR (strftime('%Y', b.from_date) - strftime('%Y', a.from_date) = 1))
order by salary_growth desc
```

## 28. 查找描述信息(film.description)中包含robot的电影对应的分类名称(category.name)以及电影数目(count(film.film_id))，而且还需要该分类包含电影总数量(count(film_category.category_id))>=5部 
```sql
select c.name, count(f.film_id)
from film f,
(select category_id, count(film_id) as category_num from film_category  
group by category_id having count(film_id)>=5) as new, category c, film_category fc
where new.category_id = c.category_id
and f.film_id = fc.film_id
and c.category_id = fc.category_id
and f.description like '%robot%'
```

## 29. 使用join查询方式找出没有分类的电影id以及名称
```sql
select f.film_id, f.title
from film f left join film_category fc on f.film_id = fc.film_id
where fc.category_id is NULL
```

## 30. 
