---
Title: Journal Entry 1 
Date: 2025-01-07
---

Journal Entry 1 
Basics of SQL filtering, joining, selecting. 

1. SELECT
    - SELECT DISTINCT 
    - COUNT() 

2. Filtering Data : WHERE 
    - WHERE 
    - AND 
    - OR 
    - BETWEEN 
    - LIKE 
    - NOT LIKE 
    - WHERE IN 
    - IS NULL 
    - IS NOT NULL 

3. Aggregate functions 
    - ROUND() 

4. Sorting results 
    - GROUP BY 
    - HAVING 

5. Joining tables 
    - INNER JOIN 
       - ON 
      - USING 
    - LEFT JOIN 
    - RIGHT JOIN 
    - FULL JOIN 
    - CROSS JOIN 
    - Self Joins 

6. Set theory 
    - UNION vs UNION ALL
    - INTERSECT
    - EXCEPT 

7. Sub queries 
    - Multiple WHERE clauses
    - Semi join
    - Sub queries inside WHERE and SELECT statements
    - Sub queries inside FROM 

8. CASE statements 
    - CASE WHEN _____ THEN ____ 
        WHEN _____   THEN ____ 
        ELSE _____ END AS column_name 
    - CASE statments with Aggregate functions 
      ```
      COUNT(CASE WHEN _____ THEN _____
                 WHEN _____ THEN ____ 
                 ELSE ____ END) AS column_name
      ```
        - Returns the count of everything that the CASE statement returns
      ```
      SUM(CASE WHEN _____ THEN _____
                 WHEN _____ THEN ____ 
                 ELSE ____ END) AS column_name
      ```
      ```
      AVG(CASE WHEN ____ THEN ____
               ELSE ____ END) AS column_name
      ```

9. Subqueries
       -  Can be in any part of the query (SELECT, FROM, WHERE, GROUP BY)
       - Can return a variety of informations
         - Scalar quantities
         - A list
         - A table
       - You can create multiple subqueries inside one FROM statement
         - Make sure you alias them individually, and JOIN them
       - SELECT subqueries
         - subquery must return a single value 


