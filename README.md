# my_first_project
 -- SQL sales analysis-p1
 create  database sql_project_1;
 

USE SQL_PROJECT_1;
 -- create a table
 USE SQL_PROJECT_1;
 create table retail_sales (
		         transactions_id INT PRIMARY KEY,
				 sale_date	DATE,
                 sale_time	TIME,
				 customer_id INT,	
				 gender	VARCHAR(15),
				 age INT,	
				 category VARCHAR(15),
				 quantiy INT,
				 price_per_unit	FLOAT,
				 cogs FLOAT,
				 total_sale FLOAT
 );


 -- 01 altering table
 ALTER TABLE retails_sales
RENAME COLUMN `ï»¿transactions_id` TO transactions_id;


-- 02 categorys retails_sales
SELECT DISTINCT category FROM retails_sales;

-- 03 checking the null values
select * from retails_sales
where transactions_id is null;
--
SELECT * FROM retails_sales
WHERE 
    sale_date IS NULL OR 
    sale_time IS NULL OR 
    customer_id IS NULL OR 
    gender IS NULL OR
    age IS NULL OR 
    category IS NULL OR 
    quantity IS NULL OR 
    price_per_unit IS NULL OR 
    cogs IS NULL;
    
-- altering the table   
alter table retails_sales 
rename column quantiy to quantity ;

-- data exploration
-- 04 how many sales we have
select count(*) as total_sales 
from retails_sales;

-- 05 how many uniq customers we have
select count(distinct customer_id) as total_uniq_customers
from retails_sales;

-- 06  how many uniq customers we have
select distinct category from retails_sales;

-- 07 Write a SQL query to retrieve all columns for sales made on '2022-11-05':
SELECT *
FROM retailS_sales
WHERE sale_date = '05-11-2022';

-- 08  Write a SQL query to retrieve all transactions where the category is 
-- 'Clothing' and the quantity sold is more than 10 in the month of Nov-2022:
SELECT *
FROM retails_sales
WHERE category ='Clothing'
and quantity >= 4
AND sale_date BETWEEN '2022-11-01' AND '2022-11-30';

-- 09 tatal sales eac cateory
SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retails_sales
GROUP BY 1;

-- 10 average age of customers who purchased items from the 'Beauty' category.
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retails_sales
WHERE category = 'Beauty';

 -- 11 find all transactions where the total_sale is greater than 1000.
 SELECT * FROM retails_sales
WHERE total_sale > 1000;

-- 12 find the total number of transactions (transaction_id) made by each gender in each category.
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retails_sales
GROUP BY category,gender
ORDER BY 1;

-- 13 top 5 customer based highest total sales;
 SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retails_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;

-- 14 find the number of unique customers who purchased items from each category
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retails_sales
GROUP BY category;

-- 15 each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17):
WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retails_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift
