# SQL Murder Mystery 🕵️‍♂️

https://mystery.knightlab.com/
![alt text](image.png)

## My Solutions Below:

### Find information about witnesses in crime scene report table
### Murder took place on 15 Jan 2018 in SQL City
```sql
SELECT *
FROM crime_scene_report
WHERE 
	city = 'SQL City'
	AND date = 20180115
	AND type = 'murder';
```
Output tells us there are two witnesses
Witness 1 lives at THE LAST house of "Northwestern Dr"
Witness 2 is named Annabel, lives somewhere on "Franklin Ave"

### Use witness (person) table to gather more info
### Witness 1
```sql
SELECT *
FROM person
WHERE address_street_name LIKE 'North*'
ORDER BY address_number DESC;
```
-Morty Schapiro, id = 14887

### Witness 2
```sql
SELECT *
FROM person
WHERE 
	name LIKE 'Annabel%'
	AND
	address_street_name LIKE 'Franklin Ave%';
```
-Annabel Miller, id = 16371

### Lookup both witness' interviews
```sql
SELECT *
FROM interview
WHERE 
	person_id = 14887
	OR
	person_id = 16371;
```

### Use interview details to get more information about killer
### Had a Get Fit Now Gym bag which only gold members have
### Membership # starts with "48Z"
### Licence plate has "H42W" somewhere in it
### Worked out at the gym on 9th of Jan
```sql
SELECT 
	membership_id
	,person_id
	,p.name
FROM get_fit_now_check_in as gc
JOIN get_fit_now_member as gfm
	ON gc.membership_id = gfm.id
JOIN person as p
	ON p.id = gfm.person_id
JOIN drivers_license as dl
	ON dl.id = p.license_id
WHERE 
	membership_id LIKE '48Z%'
	AND membership_status = 'gold'
	AND plate_number LIKE '%H42W%';
```
-Output: Jeremy Bowers, person_id = 67318

### Lookup killer's interview
```sql
SELECT *
FROM interview
WHERE person_id = 67318;
```

### Use interview details to get information about 'real mastermind'
### Woman has lots of money, between 65" and 67"
### Red hair, drives Tesla Model S
### Attended SQL Symphony Concert 3x in December 2017
```sql
SELECT
	name
	,p.id
FROM drivers_license as dl
JOIN person as p
	ON p.license_id = dl.id
JOIN facebook_event_checkin as fb
	ON fb.person_id = p.id
WHERE 
	car_model = 'Model S'
	AND car_make = 'Tesla'
	AND gender = 'female'
	AND height BETWEEN 65 AND 67;
```
-Output: Miranda Priestley

### Congrats, you found the brains behind the murder! 
### Everyone in SQL City hails you as the greatest SQL detective of all time. 
### Time to break out the champagne!
