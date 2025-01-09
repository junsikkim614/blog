---
Title: Journal Entry 2
Date: 2025-01-08
---

Journal Entry 2 

1. Correlated Subqueries
   |Simple Subquery|Correlated subqueries|
   |----|----|
   |Can be run independently from main query|Dependent on main query to execute|
   |Evaluated once in the whole query|Evaluated in loops (signficantly slows down query runtime|
2. Nested Subqueries
   - EX :
   ```sql
   SELECT
     EXTRACT(MONTH FROM date) AS month,
     SUM(m.home_goal + m.away_goal) AS total_goals,
     SUM(m.home_goal + m.away_goal) -
       (SELECT AVG(goals)
         FROM (SELECT
               EXTRACT(MONTH FROM date) AS month,
               SUM(home_goal + away_goal) AS goals
               FROM match
               GROUP BY month) AS s) AS diff
   FROM match AS n
   GROUP BY month;
   ```
     - Most inner subquery calculates the sum of all goals scored by the month
     - This is subtracted from the sum of each months' total goal scored and aliased as diff
     - Resulting table shows the month, goals in that month, and the difference against the average goals scored per month.
     - Subqueries doesn't have to be correlated necessarily
   - EX Nested subquery in FROM statement
   ```sql
   SELECT
    	c.name AS country,
        -- Calculate the average matches per season
    	AVG(outer_s.matches) AS avg_seasonal_high_scores
    FROM country AS c
    -- Left join outer_s to country
    LEFT JOIN (
      SELECT country_id, season,
             COUNT(id) AS matches
      FROM (
        SELECT country_id, season, id
    	FROM match
    	WHERE home_goal >= 5 OR away_goal >= 5) AS inner_s
      -- Close parentheses and alias the subquery
      GROUP BY country_id, season) AS outer_s
    ON c.id = outer_s.country_id
    GROUP BY country;
    ```
    - Subquery nested in LEFT JOIN FROM statement chooses when a home or away goal scored more than 5 goals as
      inner_s. Grouping it by country and season is then saved as outer_s. This is then called in the main query
       as avg_seasonal_high_scores because it records the outlier games that had unusually high amounts of goals
       for each country for each season. 
  3. 3. Common Table Expressions
   - Created before query for easier access to information
     ```sql
     WITH cte AS (
         SELECT col1, col2
         FROM table)
     SELECT
       AVG(col1) AS avg_col
     FROM cte;
     ```
   - Create multiple CTEs with commas in between declaring each CTE
   - CTEs are only executed once then stored in memory
     - Improves query performance
   - Improves organization of queries
   - CTE can reference other CTE that was declared before
     ```sql
     WITH match_list AS (
         SELECT 
             country_id,
             (home_goal + away_goal) AS goals
         FROM match
         -- Create a list of match IDs to filter data in the CTE
         WHERE id IN (
             SELECT id
             FROM match
             WHERE season = '2013/2014' AND EXTRACT(MONTH FROM date) = 08)
     )
     -- Select the league name and average of goals in the CTE
     SELECT 
         l.name,
         AVG(goals)
     FROM league AS l
     -- Join the CTE onto the league table
     LEFT JOIN match_list ON l.id = match_list.country_id
     GROUP BY l.name;
     ```
   - Creates a CTE named `match_list` which calculates goals in every match for the 2013/2014 season in August.
   - Calculates the average goal for each league during that time period by referring and `LEFT JOIN`ing with the `league` table.
  4. Differentiating Techniques


| **Joins**                                | **Correlated Subqueries**                          | **Multiple/Nested Subqueries**                  | **CTE**                          |
|------------------------------------------|---------------------------------------------------|------------------------------------------------|----------------------------------|
| Combine 2+ tables (simple operations/aggregations) | Match subqueries & tables (Avoid limits of joins, high processing time) | Multi-step transformations (improved accuracy and reproducibility) | Organize subqueries sequentially |





     
