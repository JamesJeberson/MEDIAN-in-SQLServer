# Median-in-SQLServer
SQL Server does not have MEDIAN() Function builtin hence below SQL querry helps to find Median in a dataset (say Numbers and column Number)

```sql
DECLARE @total_rows INT;
SET @total_rows = (SELECT COUNT(*) FROM Numbers);

WITH rownum_cte AS (
    SELECT 
        ROW_NUMBER() OVER (ORDER BY Number ASC) AS row_num,
        Number
    FROM Numbers
)
SELECT 
    CASE
        -- For odd row count, return the middle value
        WHEN @total_rows % 2 = 1 THEN 
            (SELECT Number FROM rownum_cte WHERE row_num = (@total_rows / 2 + 1))
        -- For even row count, return the average of the two middle values
        ELSE 
            CAST((
                (SELECT Number FROM rownum_cte WHERE row_num = @total_rows / 2) + 
                (SELECT Number FROM rownum_cte WHERE row_num = @total_rows / 2 + 1)
            ) / 2.0 AS DECIMAL(15, 4))
    END AS Median;

```
