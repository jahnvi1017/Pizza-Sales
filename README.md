# Pizza-Sales

- **Calculate the total revenue generated from pizza sales**

```
SELECT 
    ROUND(SUM(orders_details.quantity * pizzas.price),
            2) AS total_revenue
FROM
    orders_details
        JOIN
    pizzas ON orders_details.pizza_id = pizzas.pizza_id;
```

**Output:**

![Screenshot 2024-04-30 210656](https://github.com/jahnvi1017/Pizza-Sales/assets/168184461/1fbbc6d2-7f2b-498c-95c3-a897e00dcdd8)

- **Identify the highest priced pizza**

```
SELECT 
    pizza_types.name, pizzas.price
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
ORDER BY pizzas.price DESC
LIMIT 1;
```
**Output:**

![Screenshot 2024-04-30 210803](https://github.com/jahnvi1017/Pizza-Sales/assets/168184461/2b9e7d23-87e1-4689-bdfd-151eb533917f)

- **Identify the most common pizza size ordered**

```
SELECT 
    pizzas.size, COUNT(orders_details.order_id) AS total_orders
FROM
    pizzas
        JOIN
    orders_details ON orders_details.pizza_id = pizzas.pizza_id
GROUP BY pizzas.size
ORDER BY total_orders DESC
LIMIT 1; 
```

**Output:**

![Screenshot 2024-04-30 210854](https://github.com/jahnvi1017/Pizza-Sales/assets/168184461/378905e4-2749-4217-8b2d-ecbcaa84a6f9)

- **List the top 5 most ordered pizza types along with there quantities**

```
SELECT 
    pizza_types.name AS Name,
    SUM(orders_details.quantity) AS Total_Quantity
FROM
    pizzas
        JOIN
    orders_details ON orders_details.pizza_id = pizzas.pizza_id
        JOIN
    pizza_types ON pizzas.pizza_type_id = pizza_types.pizza_type_id
GROUP BY pizza_types.name
ORDER BY Total_Quantity DESC
LIMIT 5; 
```

**Output:**

![Screenshot 2024-04-30 211040](https://github.com/jahnvi1017/Pizza-Sales/assets/168184461/77df0563-fa97-4750-ab6f-f8ff5eacadbd)

- **Join the necessaery tables to find the total quantity of each pizza category**

```
SELECT 
    pizza_types.category,
    SUM(orders_details.quantity) AS total_quan
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    orders_details ON orders_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category;
```

**Output:**

![Screenshot 2024-04-30 211339](https://github.com/jahnvi1017/Pizza-Sales/assets/168184461/f3921877-c7e0-41c6-acea-7a4f203e5078)

- **Determine the distribution of orders by hour of the day**

```
SELECT 
    HOUR(order_time) AS Hours, COUNT(order_id) AS Total_Orders
FROM
    orders
GROUP BY HOUR(order_time);
```

**Output:**

![Screenshot 2024-04-30 211624](https://github.com/jahnvi1017/Pizza-Sales/assets/168184461/4afdc3d7-9db7-4ef4-9687-a01db47f9663)

- **Daily trend for total orders**

```
SELECT 
    DAYNAME(order_date) AS Weekdays,
    COUNT(order_id) AS Total_Orders
FROM
    orders
GROUP BY weekdays;
```

**Output:**

![Screenshot 2024-04-30 211802](https://github.com/jahnvi1017/Pizza-Sales/assets/168184461/2ab726f4-fd98-4da5-a966-e777ce4c0783)


- **Months trend for total orders**

```
SELECT 
    MONTHNAME(order_date) AS Months,
    COUNT(order_id) AS Total_Orders
FROM
    orders
GROUP BY Months;
```

**Output:**

![image](https://github.com/jahnvi1017/Pizza-Sales/assets/168184461/0f3618d1-7eaf-4af6-bffb-497342e1a745)

-- **Find the category wise types of pizzas**

```
SELECT 
    Category, COUNT(name) AS Total_Types
FROM
    pizza_types
GROUP BY category;
```

**Output:**

![image](https://github.com/jahnvi1017/Pizza-Sales/assets/168184461/33c995bc-e6a6-4741-9e90-79c7972c2a40)


- **Group the orders by date and calculate the average number of pizzas ordered per day**

```  
SELECT 
    ROUND(AVG(sum), 0) AS Average_pizzas
FROM
    (SELECT 
        order_date, SUM(orders_details.quantity) AS sum
    FROM
        orders
    JOIN orders_details ON orders_details.order_id = orders.order_id
    GROUP BY order_date) AS order_quan;
```

**Output:**

![Screenshot 2024-04-30 221244](https://github.com/jahnvi1017/Pizza-Sales/assets/168184461/5a000c4b-9f63-4781-bf55-1eb4e087387f)

- **Determine the top 3 most ordered pizza types based on revenue**

```
SELECT 
    pizza_types.Name,
    SUM(orders_details.quantity * pizzas.price) AS Revenue
FROM
    pizzas
        JOIN
    orders_details ON orders_details.pizza_id = pizzas.pizza_id
        JOIN
    pizza_types ON pizza_types.pizza_type_id = pizzas.pizza_type_id
GROUP BY pizza_types.name
ORDER BY revenue DESC
LIMIT 3;
```

**Output:**

![Screenshot 2024-04-30 221510](https://github.com/jahnvi1017/Pizza-Sales/assets/168184461/56d4127a-7fe5-4676-8ca8-1255b2e429f0)

- **Calculate the percentage contribution of each pizza type to total revenue**

```
SELECT 
    pizza_types.Category,
    round((SUM(orders_details.quantity * pizzas.price) / (SELECT 
            ROUND(SUM(orders_details.quantity * pizzas.price),2)
        FROM
            orders_details
                JOIN
            pizzas ON orders_details.pizza_id = pizzas.pizza_id) * 100),2) 
            as Revenue
FROM
    pizzas
        JOIN
    orders_details ON orders_details.pizza_id = pizzas.pizza_id
        JOIN
    pizza_types ON pizza_types.pizza_type_id = pizzas.pizza_type_id
GROUP BY pizza_types.category
ORDER BY revenue DESC;
```

**Output:**

![image](https://github.com/jahnvi1017/Pizza-Sales/assets/168184461/ee14ec05-cf3c-4313-a7cf-a21e4138c5cd)

- **Analyse the cumulative revenue generated overtime**

```
select
      Order_Date,
      sum(revenue) over(order by order_date) as Cumulative_Revenue
   from
     (
        select
             orders.order_date,
             sum(orders_details.quantity * pizzas. price) as revenue
        from orders_details
            join pizzas
              on
                orders_details.pizza_id = pizzas.pizza_id
            join orders
               on
                orders.order_id = orders_details.order_id
     group by orders.order_date
     order by orders.order_date )
as sales;
```

**Output:**

![image](https://github.com/jahnvi1017/Pizza-Sales/assets/168184461/04f8f81f-a4a5-42fd-a3f4-dd9eb10631ab)

- **Determine the top 3 most ordered pizza types based on revenue for each pizza category**

```
select
     Name,
     Revenue,
     Ranking
   from
      (select
           category,
           name,
           revenue,
           rank() over(partition by category order by revenue desc) as Ranking
         from
             (select
                   pizza_types.category, pizza_types.name, sum(orders_details.quantity * pizzas.price) as revenue
         from pizza_types
            join
            pizzas on pizza_types.pizza_type_id = pizzas. pizza_type_id
            join
            orders_details on orders_details.pizza_id = pizzas.pizza_id
Group by pizza_types.category, pizza_types.name) as a) as b
where Ranking <=3;
```

**Output:**

![Screenshot 2024-04-30 224940](https://github.com/jahnvi1017/Pizza-Sales/assets/168184461/552e0391-d2a9-4c4a-8707-e61dc644b478)

- **Find the revenue and total orders by months and their category**

```
select *
from
(select monthname(orders.order_date) as Months, count(orders.order_id) as total_orders, any_value(pizza_types.category) as Category, round(sum(orders_details.quantity*pizzas.price),2) as Revenue from orders
join orders_details on
orders_details.order_id = orders.order_id
join pizzas on
pizzas.pizza_id = orders_details.pizza_id
join pizza_types on
pizza_types.pizza_type_id=pizzas.pizza_type_id
Group by Months) as a
Order by revenue;
```

**Output:**

![Screenshot 2024-04-30 225121](https://github.com/jahnvi1017/Pizza-Sales/assets/168184461/4539a242-3648-4047-99bb-5db4b1403bef)


