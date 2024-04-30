# Pizza-Sales

- Calculate the total revenue generated from pizza sales
```
Select round(sum(orders_details.quantity * pizzas.price),2) as total_revenue
from orders_details join pizzas
on orders_details.pizza_id = pizzas.pizza_id;
```
