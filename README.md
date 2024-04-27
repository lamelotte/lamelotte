# SQL

## Consecutive numbers  
In all questions about consecutive number, their solution can be concluded to that: using **id - row_number()** as signal to group each consecutive numbers.
Import table: Logs
| id | num |
|-------|-------|
| 1 | 1 |
| 2 | 1 |
| 3 | 1 |
| 4 | 2 |
| 5 | 1 |
| 6 | 1 |
| 7 | 2 |
| 8 | 2 |

Results table
| ConsecutiveNums |
|-------|
| 1 |

Solution
```python
SELECT DISTINCT Num FROM (
SELECT Num,COUNT(1) as SerialCount FROM 
(SELECT Id,Num,
row_number() over(order by id) -
ROW_NUMBER() over(partition by Num order by Id) as SerialNumberSubGroup
FROM ContinueNumber) as Sub
GROUP BY Num,SerialNumberSubGroup HAVING COUNT(1) >= 3) as Result
```

## Recursive table

```python
WITH CTE AS ( 
 SELECT UserID,ManagerID,Name,Name AS ManagerName 
 FROM dbo.Employee 
 WHERE ManagerID=-1 
 UNION ALL 
 SELECT c.UserID,c.ManagerID,c.Name,p.Name AS ManagerName 
 FROM CTE P 
 INNER JOIN dbo.Employee c ON p.UserID=c.ManagerID 
) 
 
SELECT UserID,ManagerID,Name,ManagerName 
FROM CTE
```
