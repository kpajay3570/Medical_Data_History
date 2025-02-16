#                                               Medical Data History Analysis

##  Objective
The objective of this project is to analyze and extract insights from medical data history using SQL. The dataset contains patient records, diagnosis details, treatment history, and billing information. 

## Dataset Overview
The dataset includes the following tables:
- **Patients**: Contains patient demographics and contact details.
- **Diagnoses**: Stores diagnosis details, including disease names and patient IDs.
- **Treatments**: Records treatment details and associated patient information.
- **Billing**: Maintains billing and payment details.

## SQL Queries and Analysis

### 1. Show first name, last name, and gender of patients whose gender is 'M'
```sql
SELECT first_name, last_name, gender FROM patients WHERE gender='M';
```

### 2. Show first name and last name of patients who do not have allergies.
```sql
SELECT first_name, last_name FROM patients WHERE allergies IS NULL;
```

### 3. Show first name of patients that start with the letter 'C'
```sql
SELECT first_name FROM patients WHERE first_name LIKE 'C%';
```

### 4. Show first name and last name of patients whose weight is within the range of 100 to 120 (inclusive)
```sql
SELECT first_name, last_name FROM patients WHERE weight BETWEEN 100 AND 120;
```

### 5. Update the patients table for the allergies column. If the patient's allergies are null, replace it with 'NKA'
```sql
UPDATE patients SET allergies='NKA' WHERE allergies IS NULL;
```

### 6. Show first name and last name concatenated into one column as full name
```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM patients;
```

### 7. Show first name, last name, and the full province name of each patient
```sql
SELECT first_name, last_name, province_name FROM patients JOIN provinces ON patients.province_id = provinces.province_id;
```

### 8. Show how many patients have a birth_date with 2010 as the birth year.
```sql
SELECT COUNT(*) FROM patients WHERE YEAR(birth_date)='2010';
```

### 9. Show the first_name, last_name, and height of the patient with the greatest height.
```sql
SELECT first_name, last_name, height FROM patients ORDER BY height DESC LIMIT 1;
```

### 10. Show all columns for patients who have one of the following patient_ids: 1, 45, 534, 879, 1000
```sql
SELECT * FROM patients WHERE patient_id IN (1, 45, 534, 879, 1000);
```

### 11. Show the total number of admissions
```sql
SELECT COUNT(*) AS admissions FROM admissions;
```

### 12. Show all the columns from admissions where the patient was admitted and discharged on the same day.
```sql
SELECT * FROM admissions WHERE admission_date = discharge_date;
```

### 13. Show the total number of admissions for patient_id 579.
```sql
SELECT COUNT(*) AS total_admissions FROM admissions WHERE patient_id = 579;
```

### 14. Show unique cities in province_id 'NS' where patients live
```sql
SELECT DISTINCT city FROM patients WHERE province_id='NS';
```

### 15. Find first_name, last name and birth date of patients with height > 160 and weight > 70
```sql
SELECT first_name, last_name, birth_date FROM patients WHERE height > 160 AND weight > 70;
```

### 16. Show unique birth years from patients in ascending order
```sql
SELECT DISTINCT YEAR(birth_date) AS birth_year FROM patients ORDER BY birth_year ASC;
```

### 17. Show unique first names that occur only once
```sql
SELECT first_name FROM patients GROUP BY first_name HAVING COUNT(first_name) = 1;
```

### 18. Show patient_id and first_name for names that start and end with 's' and are at least 6 characters long.
```sql
SELECT patient_id, first_name FROM patients WHERE first_name LIKE 's%s' AND LENGTH(first_name) >= 6;
```

### 19. Show patient_id, first_name, last_name for patients diagnosed with 'Dementia'
```sql
SELECT p.patient_id, p.first_name, p.last_name FROM patients p INNER JOIN admissions a ON p.patient_id = a.patient_id WHERE a.diagnosis = 'Dementia';
```

### 20. Show every patient's first_name ordered by name length and then alphabetically
```sql
SELECT first_name FROM patients ORDER BY LENGTH(first_name), first_name;
```

### 21. Show the total count of male and female patients
```sql
SELECT SUM(CASE WHEN gender='M' THEN 1 ELSE 0 END) AS total_male, SUM(CASE WHEN gender='F' THEN 1 ELSE 0 END) AS total_female FROM patients;
```

### 22. Show patient_id and diagnosis for patients admitted multiple times for the same diagnosis.
```sql
SELECT patient_id, diagnosis FROM admissions GROUP BY patient_id, diagnosis HAVING COUNT(*) > 1;
```

### 23. Show the city and total patients count per city in descending order.
```sql
SELECT city, COUNT(*) AS total_patients FROM patients GROUP BY city ORDER BY total_patients DESC, city ASC;
```

### 24. Show first name, last name, and role of every person (either Patient or Doctor)
```sql
SELECT first_name, last_name, role FROM persons WHERE role IN ('Patient', 'Doctor');
```

### 25. Show all allergies ordered by popularity, excluding NULL values
```sql
SELECT allergies, COUNT(*) AS count FROM patients WHERE allergies IS NOT NULL GROUP BY allergies ORDER BY count DESC;
```

### 26. Show patients born in the 1970s, sorted by birth_date ascending
```sql
SELECT first_name, last_name, birth_date FROM patients WHERE YEAR(birth_date) BETWEEN 1970 AND 1979 ORDER BY birth_date ASC;
```

### 27. Display full name with last name in uppercase, first name in lowercase, sorted by first_name descending
```sql
SELECT CONCAT(UPPER(last_name), ',', LOWER(first_name)) AS full_name FROM patients ORDER BY LOWER(first_name) DESC;
```

### 28. Show province_id where total sum of height is >= 7000
```sql
SELECT province_id, SUM(height) AS total_height FROM patients GROUP BY province_id HAVING SUM(height) >= 7000;
```

### 29. Show the weight difference for patients with last name 'Maroni'
```sql
SELECT MAX(weight) - MIN(weight) AS weight_difference FROM patients WHERE last_name='Maroni';
```

### 30. Show the doctor_id and the total number of admissions they handled.
```sql
SELECT doctor_id, COUNT(*) AS total_admissions FROM admissions GROUP BY doctor_id ORDER BY total_admissions DESC;
```

### 31. Show the average billing amount for each patient.
```sql
SELECT patient_id, AVG(amount) AS avg_billing FROM billing GROUP BY patient_id ORDER BY avg_billing DESC;
```
### 32. Show all of the patients grouped into weight groups. Show the total amount of patients in each weight group. Order the list by the weight group descending. e.g. if they weigh 100 to 109 they are placed in the 100 weight group, 110-119 = 110 weight group, etc.
```sql
SELECT 
    CASE 
        WHEN weight BETWEEN 100 AND 109 THEN '100'
        WHEN weight BETWEEN 110 AND 119 THEN '110'
        WHEN weight BETWEEN 120 AND 129 THEN '120'
        WHEN weight BETWEEN 130 AND 139 THEN '130'
        WHEN weight BETWEEN 140 AND 149 THEN '140'
        ELSE 'Other'
    END AS weight_group,
    COUNT(*) AS total_patients
FROM patients
GROUP BY weight_group
ORDER BY weight_group DESC;
```
### 33. Show patient_id, weight, height, isObese from the patients table. Display isObese as a boolean 0 or 1. Obese is defined as weight(kg)/(height(m)). Weight is in units kg. Height is in units cm.
```sql
SELECT 
    patient_id,
    weight,
    height,
    CASE 
        WHEN (weight / ((height / 100) * (height / 100))) >= 30 THEN 1
        ELSE 0
    END AS isObese
FROM patients;
```
### 34. Show patient_id, first_name, last_name, and attending doctor's specialty. Show only the patients who have a diagnosis as 'Epilepsy' and the doctor's first name is 'Lisa'. Check patients, admissions, and doctors tables for required information.
```sql
SELECT p.patient_id, p.first_name, p.last_name, d.specialty 
FROM patients p
JOIN admissions a ON p.patient_id = a.patient_id
JOIN doctors d ON a.doctor_id = d.doctor_id
WHERE a.diagnosis = 'Epilepsy' AND d.first_name = 'Lisa';
```
### 35. All patients who have gone through admissions can see their medical documents on our site. Those patients are given a temporary password after their first admission. Show the patient_id and temp_password.
```sql
SELECT patient_id, temp_password 
FROM admissions
WHERE admission_date = (SELECT MIN(admission_date) FROM admissions GROUP BY patient_id);
```

