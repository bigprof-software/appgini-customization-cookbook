# Calculated expiry date field (given production date and validity period)
## Purpose of this recipe
You have a table for handling items that expire (for example, medications, groceries, raw materials that expire, support contracts, ... etc). You want to specify the production date (or setup/install date or so), the validity period (in days, months ... etc), and have the expiry date automatically calculated.

This can be achieved by using a [calculated field](https://bigprof.com/appgini/help/calculated-fields), and entering an SQL query similar to the one below.

## Files to edit
This SQL query below can be added in AppGini: Add an `expiry_date` field of type `DATE` to your table, and in the Calculated field tab, add the following SQL query:

```
SELECT DATE_ADD(`production_date`, INTERVAL `validity_period` MONTH) FROM `tablename` WHERE `id`='%ID%'
```
In the above query, replace `production_date` with the actual name of the production/install date field, `validity_period` with the actual field name of the validity period, `tablename` with the actual table name, and `id` with the actual name of the primary key field.
If validity period field is in days, change `MONTH` to `DAY`. Other valid values include `WEEK`, `YEAR`, and [some others](https://www.w3schools.com/sql/func_mysql_date_add.asp).
