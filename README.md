# Tables
	**PERSON**\
	![alt text](https://github.com/bennetmathew/Getting-started-with-SQL/blob/master/tables/PERSON.PNG)\
	**ADDRESS**\
	![alt text](https://github.com/bennetmathew/Getting-started-with-SQL/blob/master/tables/ADDRESS.PNG)

# Queries
1. Write a query to select all rows from person. If the person row has a value in preferred_first_name, select the preferred name instead of the value in first name.  Alias the column as REPORTING_NAME.
~~~sql
SELECT
	COALESCE(preferred_first_name, first_name) AS REPORTING_NAME
FROM
	PERSON
~~~	

2. Write a query to select all rows from person that have a NULL occupation.
~~~sql
SELECT
	*
FROM
	PERSON
WHERE
	occupation IS NULL
~~~

3. Write a query to select all rows from person that have a date_of_birth before August 7th, 1990.
~~~sql
SELECT
	*
FROM
	PERSON
WHERE
	date_of_birth < '1990-08-07'
~~~

4. Write a query to select all rows from person that have a hire_date in the past 100 days.
~~~sql
SELECT
	*
FROM
	PERSON
WHERE
	hire_date BETWEEN DATEADD(DAY, -100, GETDATE()) AND GETDATE()
~~~

5. Write a query to select rows from person that also have a row in address with address_type = 'HOME'.
~~~sql
SELECT
	*
FROM
	PERSON P
INNER JOIN
	ADDRESS A
ON
	A.person_id = P.person_id
WHERE
	A.address_type = 'HOME'
~~~

6. Write a query to select all rows from person and only those rows from address that have a matching billing address (address_type = 'BILL'). If a matching billing address does not exist, display 'NONE' in the address_type column.
~~~sql
SELECT
	P.*,
	COALESCE(A.address_type, 'NONE') AS 'address_type'
FROM
	PERSON P
LEFT  OUTER JOIN
	ADDRESS A
ON
	A.person_id = P.person_id AND
	A.address_type = 'BILL'
~~~

7. Write a query to count the number of addresses per address type.
~~~sql
SELECT
	address_type,
	COUNT(*) AS count
FROM
	ADDRESS
GROUP BY
	address_type
~~~

8. Write a query to select data in the following format:
~~~sql
SELECT
	P.last_name,
	MAX((CASE
		WHEN A.address_type = 'HOME' THEN
			A.street_line_1
	END))AS home_address,
	MAX((CASE
		WHEN A.address_type = 'BILL' THEN
			A.street_line_1
	END)) AS billing_address
FROM
	ADDRESS A
INNER JOIN 
	PERSON P
ON
	P.person_id = A.person_id
GROUP BY
	A.person_id, P.last_name
~~~

9. Write a query to update the person.occupation column to ‘X’ for all rows that have a ‘BILL’ address in the address table.
~~~sql
UPDATE
	PERSON
SET
	occupation = 'X'
WHERE
	person_id IN (SELECT
					  person_id
				  FROM
					  ADDRESS
				  WHERE
					address_type = 'BILL')
