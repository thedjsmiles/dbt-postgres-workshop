# Document

## When in doubt, DOCUMENT!

Adding documentation to your project allows you to describe your models in rich detail, and share that information with your team. Here, we're going to add some basic documentation to our project.

Update your `models/schema.yml` file to include some descriptions, such as those below.

``` yaml title="models/schema.yml"

version: 2

models:
  - name: customers
    description: One record per customer
    columns:
      - name: customer_id
        description: Primary key
        tests:
          - unique
          - not_null
      - name: first_order_date
        description: NULL when a customer has not yet placed an order.

  - name: stg_customers
    description: This model cleans up customer data
    columns:
      - name: customer_id
        description: Primary key
        tests:
          - unique
          - not_null

  - name: stg_orders
    description: This model cleans up order data
    columns:
      - name: order_id
        description: Primary key
        tests:
          - unique
          - not_null
      - name: status
        tests:
          - accepted_values:
              values: ['placed', 'shipped', 'completed', 'return_pending', 'returned']

```

Once done, run the following: 

``` bash
dbt docs 
```

This command will  generate the documentation for your project. dbt inspects your project and your warehouse to generate a JSON file with details on your project.

``` bash 
dbt docs serve
```