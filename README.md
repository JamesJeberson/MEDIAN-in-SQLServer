# Median-in-SQLServer
SQL Server does not have MEDIAN() Keyword hence below SQL Querry helps to find Median in a Dataset

```sql
DECLARE @total_rows INT
SET @total_rows = (SELECT COUNT(*) FROM Numbers);

WITH rownum_cte AS (
    SELECT 
        ROW_NUMBER() OVER(ORDER BY Number ASC) as row_num,
        LAT_N
    FROM Numbers
)
SELECT TOP 1 
        CASE 
            WHEN (@total_rows % 2 = 1) THEN (SELECT Number FROM rownum_cte WHERE row_num = (@total_rows / 2 + 1))
            ELSE (SELECT (((CASE WHEN row_num = @total_rows/2 THEN Number END) +
                           (CASE WHEN row_num = @total_rows/2 + 1 THEN Number END)) / 2))
        END
FROM rownum_cte
```
