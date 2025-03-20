# SqlMurderMistery
SqlMurderMisterySolution

SELECT * 
FROM crime_scene_report
WHERE type = 'murder'
AND date = '20180115'
AND city = 'SQL City';

--The first witness lives at the last house on "Northwestern Dr".
--The second witness, named Annabel, lives somewhere on "Franklin Ave".

SELECT id,name,address_street_name, max(address_number)
FROM person
WHERE address_street_name = 'Northwestern Dr';
--id	name	address_street_name	max(address_number)
--14887	Morty Schapiro	Northwestern Dr	4919

SELECT id, name, address_street_name, address_number
FROM person
WHERE address_street_name = 'Franklin Ave'
AND name LIKE '%Annabel%';
--id	name	address_street_name	address_number
--16371	Annabel Miller	Franklin Ave	103

SELECT *
FROM interview
WHERE person_id = '14887'
 /*"I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W"."

*/
SELECT *
FROM interview
WHERE person_id = '16371'
-- "I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th."

SELECT *
FROM get_fit_now_check_in
WHERE membership_id LIKE '48Z%'
AND check_in_date = '20180109'
/*
membership_id	check_in_date	check_in_time	check_out_time
///48Z7A	20180109	1600	1730
///48Z55	20180109	1530	1700
*/
SELECT *
FROM drivers_license
WHERE plate_number LIKE '%H42W%'
AND gender = 'male'
/id	age	height	eye_color	hair_color	gender	plate_number	car_make	car_model
423327	30	70	brown	brown	male	0H42W2	Chevrolet	Spark LS
664760	21	71	black	black	male	4H42WR	Nissan	Altima

SELECT id, name, license_id
FROM person
WHERE license_id IN (423327, 664760)
/*
id	name	license_id
51739	Tushar Chandra	664760
67318	Jeremy Bowers	423327
*/
SELECT *
FROM get_fit_now_member
WHERE person_id in (51739, 67318)
/*
id	person_id	name	membership_start_date	membership_status
48Z55	67318	Jeremy Bowers	20160101	gold
*/
SELECT *
FROM interview
WHERE person_id = 67318  
/*I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017.
*/
SELECT P.name, P.id, L.age, L.gender, L.height, P.license_id, L.hair_color, L.car_model, L.car_make,
F.event_name, F.date, I.ssn, I.annual_income
 FROM person P
 INNER JOIN drivers_license L ON L.id = P.license_id
 INNER JOIN facebook_event_checkin F ON P.id = F.person_id
 INNER JOIN income I ON I.ssn = P.ssn
 WHERE L.gender = 'female'
 AND L.height BETWEEN 65 AND 67
 AND L.hair_color = 'red'
 AND L.car_model = 'Model S'
 AND L.car_make = 'Tesla'
 AND F.event_name Like 'SQL Symphony Concert'
 AND F.date between '20171201' AND '20171231'
 GROUP BY P.id
 HAVING COUNT(F.date) = 3;
 /*
--name	id	age	gender	height	license_id	hair_color	car_model	event_name	date	annual_income
--Miranda Priestly	99716	68	female	66	202298	red	Model S	SQL Symphony Concert	20171229	310000

Congrats, you found the brains behind the murder! Everyone in SQL City hails you as the greatest SQL detective of all time. Time to break out the champagne!
*/
