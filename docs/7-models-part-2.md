# Models Pt. 2

## Mo' Money, Mo' Models
As a best practice in SQL, you should separate logic that cleans up your data from logic that transforms your data. You have already started doing this in the existing query by using common table expressions (CTEs).Now we can experiment by separating the logic out into separate models and using the ref function to build models on top of other models!

To start, create a new SQL file, `models/stg_customers.sql`, with the SQL from the customers CTE in our original query.

Create a second new SQL file, `models/stg_orders.sql`, with the SQL from the orders CTE in our original query.

``` sql title="models\stg_customers.sql"
SELECT
  "ID" AS customer_id
  ,"FIRST_NAME" AS first_name
  ,"LAST_NAME" AS last_name
FROM raw.jaffle_shop_customers
```

``` sql title="models\stg_orders.sql"
SELECT
  "ID" AS order_id,
  "USER_ID" AS customer_id,
  "ORDER_DATE" AS order_date,
  "STATUS" AS status
FROM raw.jaffle_shop_orders
```

Edit the SQL in your `models/customers.sql` file as follows:

``` sql title="models\customers.sql"

{{ config(materialized='table') }}

WITH customers AS (

  SELECT * FROM {{ ref('stg_customers') }}

),

orders AS (

  SELECT * FROM {{ ref('stg_orders') }}

),

customer_orders AS (

  SELECT
    customer_id,
    min(order_date) AS first_order_date,
    max(order_date) AS most_recent_order_date,
    count(order_id) AS number_of_orders
  FROM orders
  GROUP BY 1

),

final as (

  SELECT
    customers.customer_id,
    customers.first_name,
    customers.last_name,
    customer_orders.first_order_date,
    customer_orders.most_recent_order_date,
    coalesce(customer_orders.number_of_orders, 0) as number_of_orders
  FROM customers
  LEFT JOIN customer_orders USING (customer_id)

)

SELECT * FROM final
```

Once done, run `dbt run` and your output should look something like the following!

```
Running with dbt=1.5.0
Unable to do partial parsing because a project config has changed
Found 3 models, 0 tests, 0 snapshots, 0 analyses, 307 macros, 0 operations, 3 seed files, 0 sources, 0 exposures, 0 metrics, 0 groups

Concurrency: 3 threads (target='prod')

1 of 3 START sql view model raw.stg_customers .................................. [RUN]
2 of 3 START sql view model raw.stg_orders ..................................... [RUN]
1 of 3 OK created sql view model raw.stg_customers ............................. [CREATE VIEW in 0.28s]
2 of 3 OK created sql view model raw.stg_orders ................................ [CREATE VIEW in 0.28s]
3 of 3 START sql table model raw.customers ..................................... [RUN]
3 of 3 OK created sql table model raw.customers ................................ [SELECT 100 in 0.18s]

Finished running 2 view models, 1 table model in 0 hours 0 minutes and 0.68 seconds (0.68s).

Completed successfully

Done. PASS=3 WARN=0 ERROR=0 SKIP=0 TOTAL=3
```