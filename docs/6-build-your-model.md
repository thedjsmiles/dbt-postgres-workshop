# Models

## Build Your First Model
In the `/models` directory, create a new file named `customers.sql`. 

Copy the following SQL into the file and click Save.

``` sql
{{ config(materialized='table') }}

WITH customers AS (

  SELECT
     "ID" AS customer_id
    ,"FIRST_NAME" AS first_name
    ,"LAST_NAME" AS last_name
  FROM raw.jaffle_shop_customers

),

orders AS (

  SELECT
     "ID" AS order_id
    ,"USER_ID" AS customer_id
    ,"ORDER_DATE" AS order_date
    ,"STATUS" AS status
  FROM raw.jaffle_shop_orders

),

customer_orders AS (

  SELECT
     customer_id
    ,min(order_date) AS first_order_date
    ,max(order_date) AS most_recent_order_date
    ,count(order_id) AS number_of_orders
  FROM orders
  GROUP BY 1

),

final AS (

  SELECT
     customers.customer_id
    ,customers.first_name
    ,customers.last_name
    ,customer_orders.first_order_date
    ,customer_orders.most_recent_order_date
    ,COALESCE(customer_orders.number_of_orders, 0) AS number_of_orders
  FROM customers
  LEFT JOIN customer_orders USING (customer_id)

)

SELECT * FROM final
```

Now you can go into the command line and see your table: 
``` sql
-- See your new table!
SELECT *
FROM pg_catalog.pg_tables
WHERE 
  schemaname != 'pg_catalog' AND
  schemaname != 'information_schema';

-- Then run...
SELECT *
FROM raw.customers;
```