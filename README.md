# Restaurant-Sales-Data-Analysis-Using-SQL

---------------------------------------------------------------objective 1--------------------------------------------------------------------------

--1. view the menu_items table

SELECT * FROM menu_items

--2. Find the number of itmes on the menu

SELECT
	count(*) as no_of_menu_items
FROM menu_items

--3. what are the LEAST and most expensive items on the menu?

--least expensive

SELECT * FROM menu_items
ORDER by price ASC
limit 1

--most expensive

SELECT * FROM menu_items
ORDER by price DESC
limit 1

--4.how many italian dishes are on the menu?

SELECT * FROM menu_items
WHERE
	category = 'Italian'

--5. what are the LEAST and most expensive italian dishes on the menu?

--least expensive italian dishes

SELECT * FROM menu_items
where
	category = 'Italian'
order by price ASC
limit 1

--most expensive italian dishes

SELECT * FROM menu_items
WHERE
	category = 'Italian'
order by price DESC
limit 1

--6. how many dishes are in each category?

SELECT
	category,
	count(*) as no_of_dishes
FROM menu_items
group by 1

--7. what is average dish price in each category

SELECT
	category,
	avg(price) as avg_dish_price
FROM menu_items
group by 1

---------------------------------------------------------------objective 2--------------------------------------------------------------------------

--1. view the order_details table.

SELECT * FROM order_details

--2. what is the date range of the table?

SELECT 
	min(order_date),
	max(order_date)
FROM order_details

--3. how many orders were made within this date range?

SELECT
	count(distinct order_id)
FROM order_details

--4. how many items were ordered within this date range?

SELECT
	count(*)
FROM order_details

--5. which orders has the most numbers of items?

SELECT
	order_id,
	count(item_id) as no_of_items
FROM order_details
group by 1
order by 2 desc

--6. how many orders have more than 12 items?

WITH cte as
(
SELECT
	order_id,
	count(item_id) as no_of_items
FROM order_details
group by 1
having count(item_id) > 12
)
select
	count(*) as num_orders
FROM cte

---------------------------------------------------------------objective 3--------------------------------------------------------------------------

--1. combine the menu_items and order_details tables into a single table

SELECT * FROM order_details as od
LEFT JOIN menu_items as mi
ON od.item_id = mi.menu_item_id

--2. what were the most and least ordered items? what categories were they in?

--least expensive items

SELECT
	mi.item_name,
	mi.category,
	count(od.order_details_id) as num_ordered
FROM order_details as od
LEFT JOIN menu_items as mi
on od.item_id = mi.menu_item_id
group by 1,2
order by 3 ASC

--most expensive items

SELECT
	mi.item_name,
	mi.category,
	count(od.order_details_id) as num_ordered
FROM order_details as od
LEFT JOIN menu_items as mi
on od.item_id = mi.menu_item_id
group by 1,2
order by 3 DESC

--3. what were the top 5 orders that spent the most money?

SELECT
	order_id,
	sum(price) as total_spend
FROM order_details as od
LEFT JOIN menu_items as mi
on od.item_id = mi.menu_item_id
GROUP by 1
HAVING sum(price) <> 0
ORDER by 2 DESC
LIMIT 5

--4. view the details of highest spend order. what insights can you gather from the result

SELECT
	category,
	count(item_id) as num_items
FROM order_details as od
LEFT JOIN menu_items as mi
ON od.item_id = mi.menu_item_id
WHERE
	order_id = 440
group by category

--5. view the details of top 5 highest spend order. what insights can you gather from the result

SELECT
	order_id,
	category,
	count(item_id) as num_items
FROM order_details as od
LEFT JOIN menu_items as mi
ON od.item_id = mi.menu_item_id
WHERE
	order_id in (440,2075,1957,330,2675)
group by order_id,category
