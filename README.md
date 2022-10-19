# Plato's-Pizza-Maven-analytics

## Datasets preview
```
Select * from portfolioProject..order_details;
select * from portfolioProject..pizza_types;
select * from portfolioProject..pizzas;
select * from portfolioProject..orders;
```

## Number of variety of pizza served.
```
select count(*) as types_of_pizza from portfolioProject..pizza_types;
```
### 32

## Veg and Non-veg pizza count
```
Select count(category) as number_of_category,category
from portfolioProject..pizza_types
group by category
```
### 6-chicken,8-classic,9-supreme,9-veggie

## Adding Veg or non-veg column
```
select name,
(case when category = 'Chicken' then 'Non-Veg'
     else 'veg'
	 end ) as type
from portfolioProject..pizza_types 
```
## Number of total orders
```
select count(*) as total_orders
from portfolioProject..orders
```
### 21K

## Adding day column
```
select count(*), date, datename(WEEKDAY,date) as day_of_the_month
from portfolioProject..orders 
group by date
order by 1 desc
```
## Busiest day of the week
```
with cte as
(select date, datename(WEEKDAY,date) as days
from portfolioProject..orders)
select count(*) as Number_of_orders, days
from cte
group by days
order by 1 desc
```
### Friday,Thursday and Saturday

## Busiest Hour of the day
```
with cte as
(select *, DATEPART(HOUR,[time]) as hr_of_the_day
from portfolioProject..orders)
select sum(quantity) as pizza_sold, hr_of_the_day
from portfolioProject..order_details od
join cte c
on c.order_id = od.order_id
group by hr_of_the_day
order by 1 desc
```
### Our busiest time slots are between 12PM to 2PM ( Lunch Time ) and between 5PM to 8PM (Evening).


## Adding month column
```
select count(*), date, datename(Month,date) as  month_of_year
from portfolioProject..orders 
group by date
order by 1 desc
```
## Busiest Month of the year
```
with cte as
(select date, datename(Month,date) as  month_of_year
from portfolioProject..orders)
select count(*) as Number_of_orders, month_of_year
from cte
group by month_of_year
order by 1 desc
```
### July


## Total Number of pizza sold 
```
select sum(quantity) as num_of_pizza_sold
from portfolioProject..order_details
```
### 49574 pizzas sold

## Total revenue
```
select Round(sum(od.quantity*p.price),2) as total_revenue
from portfolioProject..order_details od
join portfolioProject..pizzas P
ON od.pizza_id= P.pizza_id
```
### $817860.05 ;

## Average order per day
```
with cte as
(select count(order_id) as num, date
from portfolioProject..orders
group by date)
select AVG(num) as average_order_per_day
from cte
```
### 59 Average orders per day

## Most prefered size
```
select count(quantity) as pizza_sold, size
from portfolioProject..pizzas p 
join portfolioProject..order_details od
on od.pizza_id = p.pizza_id
group by p.size
```
### Large is the most preferred size
### XL and XXL is the least preferred by the people.

## Top 10 Best seller
```
select top 10 count(od.quantity) as pizza_sold,pt.name
from portfolioProject..order_details od
join portfolioProject..pizzas p
on od.pizza_id = p.pizza_id
join portfolioProject..pizza_types pt
on pt.pizza_type_id = p.pizza_type_id
group by pt.name
order by 1 desc
```
### The classic duluxe pizza is the best selling pizza - 2416

## Least popular pizza 
```
select top 10 count(od.quantity) as pizza_sold,pt.name
from portfolioProject..order_details od
join portfolioProject..pizzas p
on od.pizza_id = p.pizza_id
join portfolioProject..pizza_types pt
on pt.pizza_type_id = p.pizza_type_id
group by pt.name
order by 1 asc;
```
### The brie carre pizza is the least selling - 480

## AVG ORDER VALUE
```
select
(select Round(sum(od.quantity*p.price),2) as total_revenue
from portfolioProject..order_details od
join portfolioProject..pizzas P
ON od.pizza_id= P.pizza_id)/
(select count(*) as total_orders
from portfolioProject..orders o
) as avg_order_value
```
### $38.30


