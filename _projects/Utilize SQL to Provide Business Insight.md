---
title: Utilize SQL to Provide Business Insight -Danny’s Diner
image: /assets/images/laptop.jpg
description: Analysis of specific business cases using SQL, A Weekly SQL Challange
company: 8 Week SQL Challenge
date:  2022-12-19
layout: post
---

## Background Information
Danny seriously decides to embark upon a risky venture and opens up a cute little restaurant that sells his three favourite foods: sushi, curry, and ramen. Danny’s Diner is in need of assistance to help the restaurant stay afloat — the restaurant has captured some fundamental data from its few months of operation. Still, it has no idea how to use its data to help run the business.

Three key datasets have been shared by the restaurant owners, including sales, menu, and members. Danny wants to get some insight into whether he should expand the existing customer loyalty program and generate some well-organized datasets for easily inspecting.

_"The case study information and corresponding data sources is belonging to the [“8 Weeks SQL Challenge” ](https://8weeksqlchallenge.com/)designed and credited by Danny Ma"_

![image](https://user-images.githubusercontent.com/120835197/210643142-c53d961f-5b17-422c-8443-5f34dc51bc52.png "Entity Relationship Diagram")

## Overview of the Datasets
All datasets exist within the dannys_diner the [database schema](https://8weeksqlchallenge.com/case-study-1/),  the sales table captures all ```customer_id``` level purchases with a corresponding order_date and ```product_id``` information for when and what menu items were ordered.The menu table maps the ```product_id``` to the actual ```product_name``` and price of each menu item.The final members table captures the ```join_date``` when a ```customer_id``` joined the beta version of Danny’s Diner loyalty program.

## Data Analysis
Due to the already-built database schema, the data can be directly processed for analysis after review with the ROCCC Principle and all the queries are written with the MySQL server.
#### 1. The total amount each customer spent
I use the JOIN()function to join the two tables first and use the SUM() to sum up all the purchases to calculate the money spent.
```
SELECT s.customer_id, SUM (m.price) AS customer_spent
FROM sales s
JOIN menu m
ON s.product_id=m.product_id
GROUP BY s.customer_id;
```
#### 2. Count the days of each customer visit
To be aware that Customers can make several purchases per day, the function COUNT() and DISTINCT ()need to be used simultaneously.
```
SELECT customer_id,
Count (DISTINCT(order_date)) AS number_of_visiting_days
FROM Sales
GROUP BY customer_id;
```
![image Alt Text](https://user-images.githubusercontent.com/120835197/210644216-d0df13bd-0095-4aef-b04b-502798ddba1e.png "Count the days of each customer visit") 
#### 3. The first item be purchased by each customer on their first visit
The nested SELECT() is used here (a query within a query). In the sales and menu tables, Main SELECT () is used to select the distinct customer id and product names. Following that, I added the condition where the order date is the minimum order date with the second SELECT().
```
SELECT DISTINCT(customer_id), product_name FROM sales s
JOIN menu m
ON s.product_id = m.product_id
WHERE s.order_date = ANY (
    SELECT MIN(order_date)
    FROM sales
    GROUP BY customer_id
);
```
![image](https://user-images.githubusercontent.com/120835197/210646389-2dab1ccd-43cd-4a18-9e1a-66d658dda976.png "The first item be purchased")
#### 4. Most popular food & Purchased time
I use JOIN () to join the two tables to generate the output, then, with some sorting functions such as DESC and LIMIT to get extreme value.
```
SELECT COUNT(product_name) AS most_popular_product, product_name FROM sales s
JOIN menu m
ON s.product_id=m.product_id
GROUP BY product_name
ORDER BY most_popular_product DESC
LIMIT 1;
```
![image](https://user-images.githubusercontent.com/120835197/210646566-57c0192a-aeaf-4cf8-a263-869e7e01ee3f.png "Most popular food & Purchased time")
#### 5. Each customer’s favourite food
The query gets a little bit complex here; I first developed a common table expression that ranked customers according to the number of products they had purchased; then, I selected the customer id, the product name, and the count from the expression, limiting the rank to 1.
_To be aware, Function DENSE_RANK () only applied to MySQL v8.0 or above._
```
WITH cte_ranking AS 
(
SELECT s.customer_id,m.product_name,
        COUNT(s.product_id) as order_count,
        DENSE_RANK() OVER (PARTITION BY s.customer_id ORDER BY COUNT(s.product_id) DESC) AS cte_ranking
FROM menu m 
JOIN sales s 
ON s.product_id = m.product_id
GROUP BY s.customer_id, s.product_id, m.product_name
) 
SELECT customer_id, product_name, order_count
FROM cte_ranking
WHERE cte_ranking = 1;
```
![image](https://user-images.githubusercontent.com/120835197/210646796-ad785d52-1ae2-4e30-abd0-cba273368382.png "Each customer’s favourite food")
#### 6. The first item customer purchased after they become a member
First, a ranking table will be created to rank customers by order date and filter the result to get the first line that the order date is equal to or beyond the membership date.
```
WITH ordered_became_member AS
(
  SELECT 
      s.customer_id 
      ,m.product_name 
      ,s.order_date 
      ,DENSE_RANK() OVER(
          PARTITION BY s.customer_id
          ORDER BY s.order_date
      ) AS  rank
  FROM sales s
  JOIN menu m
      ON s.product_id  = m.product_id
  JOIN members mem
      ON s.customer_id = mem.customer_id
  WHERE s.order_date >= mem.join_date
)

SELECT 
  customer_id, product_name,order_date
FROM ordered_became_member
WHERE rank = 1;
```
![image](https://user-images.githubusercontent.com/120835197/210647211-63d6a747-2bed-4ae3-96af-8a5f1cdfd222.png "The first item customer purchased after they become a member")
#### 7. The last item customer purchased before becoming the member
Only one query needs to be changed compared to question 6, which is letting order data before the membership date.
```
WITH ordered_before_member AS
(
  SELECT 
      s.customer_id 
      ,m.product_name 
      ,s.order_date 
      ,DENSE_RANK() OVER(
          PARTITION BY s.customer_id
          ORDER BY s.order_date
      ) AS  rank
  FROM sales s
  JOIN menu m
      ON s.product_id  = m.product_id
  JOIN members mem
      ON s.customer_id = mem.customer_id
  WHERE s.order_date <= mem.join_date
)

SELECT 
  customer_id, product_name
FROM ordered_before_member
WHERE rank = 1;
```
![image](https://user-images.githubusercontent.com/120835197/210647823-f242c902-100e-4355-9a3f-7d29240ff419.png "The last item customer purchased before becoming the member")
#### 8. The total number of items and amount of money spent before becoming a member
Multiple functions JOIN() are adapted by joining three tables together and selecting the order date before joining the membership.
```
SELECT s.customer_id,
       count(s.product_id) AS total_items, 
       SUM(price) AS money_spent
FROM sales s
JOIN menu m 
ON s.product_id = m.product_id
JOIN members mem
ON s.customer_id = mem.customer_id
WHERE s.order_date < mem.join_date
GROUP BY s.customer_id;
```
![image](https://user-images.githubusercontent.com/120835197/210647986-c1d59a4e-b661-4a33-a513-edfd0b28d0f8.png "The total number of purchasing & amount of money spent")
#### 9. If the membership plan is 1 dollar spent equals 10 points and order sushi has a 2x points multiplier, how many points would each customer earn?
A specific scenario is given here with multiple conditions. Therefore, a CASE() function can be used here; the CASE () function can apply the above conditions in a new column. Then, points are cumulative using the SUM() and displayed after a JOIN() function.
```
SELECT 
 s.customer_id,
    SUM(
      CASE
         WHEN m.product_name = 'sushi'
         THEN m.price*20
         ELSE m.price*10
         END
) AS Points
FROM sales s 
INNER JOIN menu m
ON s.product_id  = m.product_id
GROUP BY s.customer_id;
```
![image](https://user-images.githubusercontent.com/120835197/210648160-b8b4d59f-0b2f-47fb-9516-8d7f4c9cb415.png "Points customer earned")
#### 10. Scenario: In the first week after a customer joins the program (including their join date), they earn 2x points on all items, not just sushi — how many points do customers A and B have at the end of January?
First, a common table expression is created to show the order placed and how many days after becoming a member for the first week, then a CASE()function is used to calculate the points corresponding to the above conditions; after that, the EXTRACT function is used to get the first month (January).
```
SELECT customer_id, SUM(total_points)
FROM 
(WITH points AS
(
SELECT s.customer_id, 
 (s.order_date - mem.join_date) AS first_week,
        m.price,
        m.product_name,
        s.order_date
    FROM sales s
 JOIN menu m ON s.product_id = m.product_id
 JOIN members AS mem
 ON mem.customer_id = s.customer_id
    )
SELECT customer_id,
  CASE 
  WHEN first_week BETWEEN 0 AND 7 THEN price * 20
        WHEN (first_week > 7 OR first_week < 0) AND product_name = 'sushi' THEN price * 20
  WHEN (first_week > 7 OR first_week < 0) AND product_name != 'sushi' THEN price * 10
        END AS total_points,
        order_date
FROM points
WHERE EXTRACT(MONTH FROM order_date) = 1) as time
GROUP BY customer_id;
```
![image](https://user-images.githubusercontent.com/120835197/210648326-0917c6f4-f28d-46b6-8ef3-8baff02e4efc.png "Points customers A and B have at the end of January")

#### Bonus question
**Creating basic data tables that Danny and his team can derive insights**
While creating the new table, Function LEFT JOIN is used to pull all rows from the left table and only matching rows from the right table
```
SELECT s.customer_id,
  s.order_date,
        m.product_name,
        m.price,
        CASE 
        WHEN s.order_date >= mem.join_date THEN 'Y'
        ELSE 'N' 
        END AS member
FROM sales s
LEFT JOIN menu m ON s.product_id = m.product_id
LEFT JOIN members mem ON s.customer_id = mem.customer_id
ORDER BY customer_id, order_date, price DESC;
```
![image](https://user-images.githubusercontent.com/120835197/210648604-dd5fbe80-ac3a-444e-b7a0-5ee5d1343e28.png "A new basic data table")
**Ranking of customer products in the loyalty program**
FUNCTION CASE() is used here to replace the value of non-members with a null value, then in the new ranking column, rank by partitioning product id by descending order date to find out the product order ranking when the customer joins the membership.
```
WITH membership AS
(SELECT s.customer_id,
  s.order_date,
        m.product_name,
        m.price,
        CASE 
        WHEN s.order_date >= mem.join_date THEN 'Y'
        ELSE 'N' 
        END AS member
FROM sales s
LEFT JOIN menu m ON s.product_id = m.product_id
LEFT JOIN members mem ON s.customer_id = mem.customer_id
ORDER BY customer_id, order_date, price DESC
)
SELECT *,
CASE WHEN member="N" THEN "null"
ELSE 
RANK() OVER(PARTITION BY customer_id, member ORDER BY order_date)
END AS ranking
FROM membership;
```
![image](https://user-images.githubusercontent.com/120835197/210648879-c6dc1002-3c37-4f19-8396-212e6f7c6894.png "Ranking of customer products in the loyalty program")

### Insights
![image](https://user-images.githubusercontent.com/120835197/210649014-993bb843-bb63-42ca-990c-ed3ec6448fe7.png "https://8weeksqlchallenge.com/case-study-1/")

_The most popular product in Danny’s Diner is ramen._

_Customer B is the most frequent visitor in the month._

_Customers A and C prefer ramen more often, while Customer B did not show an obvious preference._

_Their points for Customers A, B, and C were 860, 940, and 360, respectively, during January._

_For the two customers join the membership, Customer A spent 25 dollars before joining, and Customer B spent 40 dollars before joining._

### Strategies
- Continue improving the points system with phrase rewards such as 1500 points for a considerable discount or 1000 points for free sushi.
- A new marketing strategy can be adopted; customers who spend over 30 dollars may get a chance to join the membership for free or a free sushi dish.
- Bundle ramen with the restaurant brand as the main product and continuously introduce new flavours and promotions.

That’s the end of the project. Thank you for your reading, hooyah!





