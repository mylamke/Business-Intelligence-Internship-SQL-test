# Business-Intelligence-Internship-SQL-test

#1. The company has employees who manages portfolios of advertisers (table A). These advertisers generate revenue for X on a daily basis (table B). Write a query that shows the total revenue by employee_id

SELECT
    A.string_field_1 AS employee_id,
    SUM(B.revenue) AS total_daily_revenue
FROM
    glass-standard-378403.Test_SQL.q1_tablea AS A
LEFT JOIN
    glass-standard-378403.Test_SQL.q1_tableb AS B 
    ON A.string_field_0 = B.advertiser_id
GROUP BY
    A.string_field_1
ORDER BY
    A.string_field_1;
    
#2. The company categorises all revenue within 90 days of an advertisers launch date as "new business" , and anything stricly over 90 days as "existing business". Write a query that shows the total new business and existing business revenue.

SELECT
    CASE
        WHEN DATE_DIFF(B.day, A.launch_date, DAY) <= 90 THEN 'New Business'
        ELSE 'Existing Business'
    END AS business_category,
    SUM(B.revenue) AS total_revenue
FROM
    glass-standard-378403.Test_SQL.q2_tablea AS A
RIGHT JOIN
    glass-standard-378403.Test_SQL.q2_tableb AS B 
    ON A.advertiser_id = B.advertiser_id
GROUP BY
    business_category;
    
#3. The company has 2 products where revenue by advertiser is logged in separate tables (table A for product A and table B for product B). Write a query that shows the revenue, by advertiser, and by product.
SELECT
    advertiser_id,
    product_name,
    SUM(revenue) AS total_revenue
FROM
    (
        SELECT
            advertiser_id,
            'Product A' AS product_name,
            revenue
        FROM
            glass-standard-378403.Test_SQL.q3_tablea

        UNION ALL

        SELECT
            advertiser_id,
            'Product B' AS product_name,
            revenue
        FROM
            glass-standard-378403.Test_SQL.q3_tableb
    ) CombinedData
GROUP BY
    advertiser_id, product_name;


#4. The company is rolling out a new customer segmentation called customer type to better understand business trends with the following rules:
- An advertiser is considered a Key Account if it is flagged as a Key Account or is Segment 1
- An advertiser is considered a Core Account if it is Segment 2 or 3
- An advertiser is considered a Tail Account if it is Segment 4
- Write a query that shows the revenue by customer type
  SELECT
    CASE
        WHEN A.segment = 1 OR B.key_account = TRUE THEN 'Key Account'
        WHEN A.segment IN (2, 3) THEN 'Core Account'
        WHEN A.segment = 4 THEN 'Tail Account'
        ELSE 'Other' 
    END AS customer_type,
    SUM(C.revenue) AS total_revenue
FROM
    glass-standard-378403.Test_SQL.q4_tablea AS A
LEFT JOIN
   glass-standard-378403.Test_SQL.q4_tableb AS B 
   ON A.advertiser_id = B.advertiser_id
LEFT JOIN
    glass-standard-378403.Test_SQL.q4_tablec AS C 
    ON A.advertiser_id = C.advertiser_id
GROUP BY
    customer_type;
