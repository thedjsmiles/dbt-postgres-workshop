# Setup PostgreSQL via Homebrew

## Install Instructions
Assuming you have homebrew installed on your system, you can run the command below on your terminal to quickly install PostgreSQL:

``` bash
brew install postgresql
```

Once done, to verify you've got PostgreSQL installed, run the following command to check your PostgreSQL version. 

``` bash
postgres --version
```

***NOTE*** - In the event that you don't have Homebrew, you can install PostgreSQL via this [link](https://www.postgresql.org/download/)

----

## Setup Instructions

If installed via Homebrew, PostgreSQL can be setup via the following command:

``` bash
brew services start postgresql@14
```

To administer PostgreSQL from the command line use the psql utility, by running the command below: 

``` bash
psql postgres
```

Once done, your command line should look like this: 

``` bash
postgres=#
```

Run the command below to list all databases: 

``` bash
\list
# or
\l
```

----

## Create Database

To create a database, you use the create database command. In the example below, we’ll create a database named `sample_aws`.

Run the following command: 

``` bash
CREATE DATABASE sample_aws;
```

***NOTE*** - Do not forget the semi-colon. Otherwise Postgres will wait for you to terminate the statement.

----

## Create User

To create a user, you use the create user command. In the example below, we’ll create a user named `test_user`.

When you create a user, the message shown is `CREATE ROLE`. 
Users are roles with login rights. You’ll also notice that the `Attributes` column is empty for the user `test_user`. This means that the user `test_user` has no administrative permissions. They can only read data and cannot create another user or database.

You can set a password for your user. To a set password for an existing user, you need use the `\password` command below:

``` bash
create user test_user with login password 'qwerty';
```

----