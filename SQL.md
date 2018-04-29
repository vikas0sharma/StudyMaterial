## Select nth highest salary
#### METHOD 1:
```sql
SELECT TOP 1 Salary
FROM
(
  SELECT DISTINCT TOP n SALARY
  FROM
  EMPLOYEES
  ORDER BY Salary DESC
) AS 'RESULT'
ORDER BY Salary
```
#### METHOD 2:
```sql
With [Result] as
(
	SELECT Salary, DENSE_RANK() Over (Order by Salary Desc) AS 'Rank'
	FROM 
	Employess
)
SELECT top 1 Salary 
FROM
Result
WHERE Rank = 3;
```
## Delete duplicate records
```sql
WITH cte as
(
	SELECT *, ROW_NUMBER() Over ( Partition by ID Order By ID ) as 'Rownumber'
	FROM
	Employees
)
DELETE From cte
WHERE Rownumber > 1;

```
## Number of employees in each department
```sql
Select Count(e.BusinessEntityID) 'NoOfEmp', d.Name
From HumanResources.EmployeeDepartmentHistory e 
Join HumanResources.Department d
on e.DepartmentID = d.DepartmentID
group by d.Name
order by NoOfEmp desc
```
## Names starts with a character without using LIKE operator
```sql
Select d.Name
from HumanResources.Department d
where LEFT(d.Name,1)='P'
```
