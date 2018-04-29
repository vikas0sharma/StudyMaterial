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

