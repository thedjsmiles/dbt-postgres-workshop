# Tests

## Lets Test!
Adding tests to a project helps validate that your models are working correctly. So, lets do that!

Create a new YAML file in the models directory, named models/schema.yml

Add the following contents to the file:

``` yaml title="models/schema.yml"
version: 2

models:
  - name: customers
    columns:
      - name: customer_id
        tests:
          - unique
          - not_null

  - name: stg_customers
    columns:
      - name: customer_id
        tests:
          - unique
          - not_null

  - name: stg_orders
    columns:
      - name: order_id
        tests:
          - unique
          - not_null
      - name: status
        tests:
          - accepted_values:
              values: ['placed', 'shipped', 'completed', 'return_pending', 'returned']
      - name: customer_id
        tests:
          - not_null
          - relationships:
              to: ref('stg_customers')
              field: customer_id

```

Your output should look something like the following! 

```
Running with dbt=1.5.0
Found 3 models, 9 tests, 0 snapshots, 0 analyses, 307 macros, 0 operations, 3 seed files, 0 sources, 0 exposures, 0 metrics, 0 groups

Concurrency: 3 threads (target='prod')

1 of 9 START test accepted_values_stg_orders_status__placed__shipped__completed__return_pending__returned  [RUN]
2 of 9 START test not_null_customers_customer_id ............................... [RUN]
3 of 9 START test not_null_stg_customers_customer_id ........................... [RUN]
1 of 9 PASS accepted_values_stg_orders_status__placed__shipped__completed__return_pending__returned  [PASS in 0.17s]
2 of 9 PASS not_null_customers_customer_id ..................................... [PASS in 0.18s]
4 of 9 START test not_null_stg_orders_customer_id .............................. [RUN]
3 of 9 PASS not_null_stg_customers_customer_id ................................. [PASS in 0.18s]
5 of 9 START test not_null_stg_orders_order_id ................................. [RUN]
6 of 9 START test relationships_stg_orders_customer_id__customer_id__ref_stg_customers_  [RUN]
5 of 9 PASS not_null_stg_orders_order_id ....................................... [PASS in 0.11s]
4 of 9 PASS not_null_stg_orders_customer_id .................................... [PASS in 0.12s]
6 of 9 PASS relationships_stg_orders_customer_id__customer_id__ref_stg_customers_  [PASS in 0.09s]
7 of 9 START test unique_customers_customer_id ................................. [RUN]
8 of 9 START test unique_stg_customers_customer_id ............................. [RUN]
9 of 9 START test unique_stg_orders_order_id ................................... [RUN]
7 of 9 PASS unique_customers_customer_id ....................................... [PASS in 0.10s]
8 of 9 PASS unique_stg_customers_customer_id ................................... [PASS in 0.11s]
9 of 9 PASS unique_stg_orders_order_id ......................................... [PASS in 0.11s]

Finished running 9 tests in 0 hours 0 minutes and 0.64 seconds (0.64s).

Completed successfully

Done. PASS=9 WARN=0 ERROR=0 SKIP=0 TOTAL=9
```

When running `dbt test`, dbt iterates through your YAML files, and constructs a query for each test. Each query will return the number of records that fail the test. If this number is 0, then the test is successful!