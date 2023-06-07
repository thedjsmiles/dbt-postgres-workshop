# DBT Setup

## Let's Configure DBT

Create dbt project by running: 

``` bash
dbt init dbt-postgres
```

Opening up your `profiles.yml` go ahead and configure it accordingly:

``` yaml
dbt-postgres:
  target: prod
  outputs:
    prod:
      type: postgres
      threads: 3
      host: localhost
      port: 5432
      user: dbt
      pass: tester
      dbname: sample_db
      schema: raw
      keepalives_idle: 0 # default 0, indicating the system default
```

Be sure to continuously run throughout this process `dbt debug` to troubleshoot any new details/information you are submitting.

----

## Seeds

Seeds are CSV files in your dbt project that dbt can load into your data warehouse.

Download the copies of the files below and add them to the `/seeds` directory.

* [jaffle_shop_customers.csv](https://dbt-tutorial-public.s3-us-west-2.amazonaws.com/jaffle_shop_customers.csv)
* [jaffle_shop_orders.csv](https://dbt-tutorial-public.s3-us-west-2.amazonaws.com/jaffle_shop_orders.csv)
* [stripe_payments.csv](https://dbt-tutorial-public.s3-us-west-2.amazonaws.com/stripe_payments.csv)

Then go into your `dbt_profile.yml` and double check the seed configuration.

``` yaml
# This setting configures which "profile" dbt uses for this project.
profile: 'dbt-postgres'

# These configurations specify where dbt should look for different types of files.
# The `model-paths` config, for example, states that models in this project can be
# found in the "models/" directory. You probably won't need to change these!
model-paths: ["models"]
analysis-paths: ["analyses"]
test-paths: ["tests"]
seed-paths: ["seeds"]
macro-paths: ["macros"]
snapshot-paths: ["snapshots"]
```
This is where the fun starts! You should be able to now upload the files into your PostgreSQL instance!

``` bash
dbt seed
```

Now moving over to the command line. Log in to your postgres instance and run: 

``` sql
SELECT * FROM raw.jaffle_shop_customers;
```