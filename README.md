# sql-data-cleaning
Using SQL to clean data

<img width="512" height="338" alt="image" src="https://github.com/user-attachments/assets/95c927fa-c006-4ffa-8574-17313cfc3b7d" />

This is an educational project on data cleaning and preparation using SQL. The original database in CSV format is located in the file club_member_info.csv. Here, we will explore the steps that need to be applied to obtain a cleansed version of the dataset.

1. View data

Use query below to view data:
```sql
SELECT * FROM club_member_info cmi
LIMIT 5;
```
Result:
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|addie lush|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|      ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|Sydel Sharvell|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|Constantin de la cruz|35||co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|  Gaylor Redhole|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|

2. Create clean table:

Use query below to create table:
```sql
CREATE TABLE club_member_info_cleaned (
	full_name VARCHAR(50),
	age INTEGER,
	martial_status VARCHAR(50),
	email VARCHAR(50),
	phone NVARCHAR(50),
	full_address NVARCHAR(50),
	job_title VARCHAR(50),
	membership_date NVARCHAR(50)
);
```
Copy data to new table
```sql
INSERT INTO club_member_info_cleaned 
SELECT * FROM club_member_info;
```
3. Remove duplicate
Use query below to review duplicate:
```sql
SELECT
	full_name,
	age,
	martial_status,
	email,
	phone,
	full_address,
	job_title,
	membership_date,
	COUNT(*) AS count_duplicate
FROM club_member_info_cleaned cmic
GROUP BY
	full_name,
	age,
	martial_status,
	email,
	phone,
	full_address,
	job_title,
	membership_date
HAVING COUNT(*)>1
```

Use query below to remove duplicate:
```sql
DELETE FROM club_member_info_cleaned
WHERE(	
	full_name
	,age
	,martial_status
	,email
	,phone
	,full_address
	,job_title
	,membership_date
)IN(SELECT
		full_name
		,age
		,martial_status
		,email
		,phone
		,full_address
		,job_title
		,membership_date
	FROM club_member_info_cleaned cmic
	GROUP BY
		full_name,
		age,
		martial_status,
		email,
		phone,
		full_address,
		job_title,
		membership_date
	HAVING COUNT(*)>1
)
```
4. Update full_name fields
Remove unnecessary blankspace
Use query below to review:
```sql
SELECT TRIM(full_name)
FROM club_member_info_cleaned
```
Use query below to update:
```sql
UPDATE club_member_info_cleaned
SET full_name = TRIM(full_name)
```
Make the name consitency
Use query below to review:
```sql
SELECT UPPER(full_name)
FROM club_member_info_cleaned
```
Use query below to update:
```sql
UPDATE club_member_info_cleaned
SET full_name = UPPER(full_name)
```
Review null value
Use query below to review:
```sql
SELECT full_name
FROM club_member_info_cleaned
WHERE full_name =''
```
No result found.

5.Update age range
Use query below to review:
```sql
SELECT age
FROM club_member_info_cleaned
WHERE age >100
```

Use query below to update:
```sql
UPDATE club_member_info_cleaned
SET age = (SELECT MODE(age) 
FROM club_member_info_cleaned)
WHERE age>100
```

6.Update martial status
Use query below to check martial status
```sql
SELECT DISTINCT martial_status 
FROM club_member_info_cleaned
```
Use query below to update martial status (blank)
```sql
UPDATE club_member_info_cleaned
SET martial_status = 'Unknow'
WHERE martial_status =''
```

Use query below to update martial status (spelling)
```sql
UPDATE club_member_info_cleaned
SET martial_status = 'divorced'
WHERE martial_status ='divored'
```
