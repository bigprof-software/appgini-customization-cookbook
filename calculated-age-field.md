# Calculated age field (given date of birth)

## Purpose of this recipe
You have a table personal info of interest (for example, employees in a company, students in a school, patients in a hospital... etc).
Among the info stored is the date of birth. You'd like to use this to calculate the current age of the person.

This can be achieved by using a [calculated field](https://bigprof.com/appgini/help/calculated-fields), and entering an SQL query similar to the one below.

## Files to edit
This SQL query below can be added in AppGini: Add an `age` field of type `Integer` to your table, and in the Calculated field tab, add the following SQL query:

```
SELECT FLOOR(DATEDIFF(NOW(), `BirthDate`) / 365)
FROM `%TABLENAME%` 
WHERE `%PKFIELD%`='%ID%'
```
In the above query, replace `BirthDate` with the actual name of the birth date field.
Whenever you visit the table in AppGini, the age field would be automatically updated to reflect the current age of each person.
