# AdventureWorks for Postgres

This project provides the scripts necessary to set up the OLTP part of the go-to database used in
training classes and for sample apps on the Microsoft stack. The result is 68 tables containing HR,
sales, product, and purchasing data organized across 5 schemas. It represents a fictitious bicycle
parts wholesaler with a hierarchy of nearly 300 employees, 500 products, 20000 customers, and 31000
sales each having an average of 4 line items. So it's big enough to be interesting, but not
unwieldy. In addition to being a well-rounded OLTP sample, it is also a good choice to demonstrate
ETL into a data warehouse.

Provided is a ruby file to convert CSVs available on CodePlex into a format usable by Postgres, as
well as a Postgres script to create the tables, load the data, convert the hierarchyid columns, add
primary and foreign keys, and create some of the views used by Adventureworks.

## How to set up the database:

Download [Adventure Works 2014 OLTP Script](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks-oltp-install-script.zip).

Extract the .zip and copy all of the CSV files into the same folder, also containing update_csvs.rb file and install.sql.

Modify the CSVs to work with Postgres by running:
```
ruby update_csvs.rb
```
Create the database and tables, import the data, and set up the views and keys with:
```
psql -c "CREATE DATABASE \"Adventureworks\";"
psql -d Adventureworks < install.sql
```
(If you do not have a database created for your user account then you may need to also add:  `-U postgres`  to the above two commands.)

All 68 tables are properly set up, and 11 of the 20 views are established.  The ones not built are those that rely on XML functions like value and ref.  To see a list of tables, open psql, and then connect to the database and show all the tables with these two commands:
```
\c "Adventureworks"
\dt (humanresources|person|production|purchasing|sales).*
```

## Using with Docker

You can spin up a new database using **Docker** with `docker-compose up`.

_You will need to rename the Adventure Works 2014 OLTP Script archive to **adventure_works_2014_OLTP_script.zip** to get this to work!_