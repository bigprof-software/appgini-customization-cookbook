# Working days between 2 dates
## Purpose of this recipe
You have a table storing some kind of business task that has a start date and an end date.
You want to calculate the duration of this task as working days, that is, **_excluding weekends_**.
This can be achieved using [calculated fields](https://bigprof.com/appgini/help/calculated-fields).
We'll create a `duration` field in AppGini, with its data type set to `Integer` and set it as
a calculated field, with the query below.

## Files to edit
In AppGini, add the following SQL code to the query box in the calculated field tab for the `duration` field:

```
SELECT FLOOR(
    ABS(DATEDIFF(end_date, start_date)) + 1
    - ABS(DATEDIFF(ADDDATE(end_date, INTERVAL 1 - DAYOFWEEK(end_date) DAY),
                   ADDDATE(start_date, INTERVAL 1 - DAYOFWEEK(start_date) DAY))) / 7 * 2
    - (DAYOFWEEK(IF(start_date < end_date, start_date, end_date)) = 1)
    - (DAYOFWEEK(IF(start_date > end_date, start_date, end_date)) = 7)
) FROM `tablename` WHERE `id`='%ID%'
```
In the above code, replace `start_date` and `end_date` with the actual name of the start  and end date fields, respectively.
Also, replace `tablename` with the actual table name, and `id` with the actual name of the primary key field.
