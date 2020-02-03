# Tables

# Problem-1
Write a query to select all rows from person. 
If the person row has a value in preferred_first_name, select the preferred name instead of the value in first name.  Alias the column as REPORTING_NAME.
~~~sql
SELECT
	COALESCE(preferred_first_name, first_name) AS REPORTING_NAME
FROM
	PERSON
