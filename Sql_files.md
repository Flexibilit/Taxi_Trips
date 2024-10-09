# Taxi_Trips
My First Work

select
  *
from
  dannys_diner.members;
select
  *
from
  pizza_runner.customer_orders;
select
  *
from
  pizza_runner.pizza_names;
select
  *
from
  pizza_runner.pizza_recipes;
select
  *
from
  pizza_runner.pizza_toppings;
select
  *
from
  pizza_runner.runners;
select
  *
from
  pizza_runner.runner_orders;
ANSWERS --1. How many pizzas were ordered?
SELECT
  SUM(pizza_id) AS total_pizzas_ordered
FROM
  pizza_runner.customer_orders;
-- 2. How many unique customer orders were made?
SELECT
  COUNT(DISTINCT customer_id) AS unique_customer_orders
FROM
  pizza_runner.customer_orders;
-- 3. How many successful orders were delivered by each runner?
SELECT
  order_id,
  COUNT(*) AS successful_orders
FROM
  pizza_runner.runner_orders
WHERE
  cancellation = 'null'
GROUP BY
  order_id;
--4. How many of each type of pizza was delivered?
SELECT
  order_id
FROM
  pizza_runner.customer_orders as o
  inner join pizza_runner.runner_orders as p ON o.order_id = p.order_id
WHERE
  o.extras = 'null'
GROUP BY
  pizza_id;
--5. How many Vegetarian and Meatlovers were ordered by each customer?
SELECT
  customer_id,
  SUM(
    CASE
      WHEN pizza_name = 'Vegetarian' THEN order_id
      ELSE 0
    END
  ) AS vegetarian_ordered,
  SUM(
    CASE
      WHEN pizza_name = 'Meatlovers' THEN order_id
      ELSE 0
    END
  ) AS meatlovers_ordered
FROM
  pizza_runner.customer_orders as o
  JOIN pizza_runner.pizza_names as p ON o.pizza_id = p.pizza_id
GROUP BY
  customer_id;
--6. What was the maximum number of pizzas delivered in a single order?
SELECT
  MAX(order_id) AS max_pizzas_in_single_order
FROM
  pizza_runner.runner_orders;
--7. For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
  --Assuming there's a changes column in the pizzas table indicating changes made:
SELECT
  customer_id,
  SUM(
    CASE
      WHEN pizza_id >= 1 THEN order_id
      ELSE 0
    END
  ) AS pizzas_with_changes,
  SUM(
    CASE
      WHEN pizza_id = 0 THEN order_id
      ELSE 0
    END
  ) AS pizzas_without_changes
FROM
  pizza_runner.customer_orders
GROUP BY
  customer_id;
-- 8. How many pizzas were delivered that had both exclusions and extras?
  -- Assuming there are columns for exclusions and extras in the pizzas table:
SELECT
  COUNT(*) AS pizzas_with_exclusions_and_extras
FROM
  pizza_runner.customer_orders
WHERE
  exclusions IS NOT NULL
  AND extras IS NOT NULL;
-- 9. What was the total volume of pizzas ordered for each hour of the day?
  -- Assuming there's a timestamp column indicating the order time:
SELECT
  EXTRACT(
    HOUR
    FROM
      order_time
  ) AS hour_of_day,
  SUM(order_id) AS total_pizzas_ordered
FROM
  pizza_runner.customer_orders
GROUP BY
  EXTRACT(
    HOUR
    FROM
      order_time
  );
-- 10. What was the volume of orders for each day of the week?
SELECT
  customer_id,
  EXTRACT(
    DOW
    FROM
      order_time
  ) AS day_of_week,
  SUM(order_id) AS total_pizzas_ordered
FROM
  pizza_runner.customer_orders
GROUP BY
  customer_id,
  EXTRACT(
    DOW
    FROM
      order_time
  );
