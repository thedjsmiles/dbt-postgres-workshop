# DBT Installation

## Install Instructions
There are a number of different ways to install DBT. For the sake of this workshop, we'll install DBT into a virtual environment.

Assuming you have python and venv installed you can run the following commands:

``` bash
# In a directory of your choosing...
python3 -m venv venv

# Once the environment has been created, run the following: 
source venv/bin/activate

# After the environment has been kickstarted, run this to test that everything is operating correctly: 
pip --version

# Your output will show that the pip version is tied to the virtual environment
# It should look something like this:

#pip 21.2.4 from /Users/sample/dbt-setup/lib/python3.9/site-packages/pip
```

Now, just to err on the side of caution we should check which version of python we're running: 

``` bash
python --version
```

If the output is greater than 3.6, be sure to install the following:

``` bash
pip install --upgrade urllib3==1.26.15
``` 
***(Source: [Github](https://github.com/googleapis/python-bigquery/issues/1565))***

----

Once done, run the install for dbt-postgres:

``` bash
pip install dbt-postgres
```

...and be sure to check your version and that the install was successful: 

``` bash
dbt --version
```
