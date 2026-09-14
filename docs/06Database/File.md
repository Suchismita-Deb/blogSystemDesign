### The query to revise.

```sql
-- Show first name of patients that start with the letter 'C'.
select first_name from patients where patients is like "C%" 

-- The first name and last name of patients that weight within the range of 100 to 120 (inclusive)
select first_name, last_name from patients where weight>=100 and weight<=120
SELECT first_name, last_name FROM patients WHERE weight BETWEEN 100 AND 120;

-- Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA'
UPDATE patients SET allergies = 'NKA' WHERE allergies IS NULL;

-- Show first name and last name concatinated into one column to show their full name.
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM patients;



-- Display every patient's first_name. Order the list by the length of each name and then by alphabetically.
SELECT first_name FROM patients ORDER BY LENGTH(first_name), first_name;

-- Show first name, last name, and the full province name of each patient. Example: 'Ontario' instead of 'ON'
SELECT patients.first_name, patients.last_name, province_names.province_name FROM patients LEFT JOIN province_names ON patients.province_id = province_names.province_id;
SELECT p.first_name, p.last_name, pn.province_name FROM patients p LEFT JOIN province_names pn ON p.province_id = pn.province_id; -- The alias and the column name should be used with the table.

-- Show the total amount of male patients and the total amount of female patients in the patients table.
Display the two results in the same row.
-- male female
-- 100  300
SELECT 
    COUNT(CASE WHEN gender = 'M' THEN 1 END) AS male,
    COUNT(CASE WHEN gender = 'F' THEN 1 END) AS female
FROM patients;

```

```sql

-- Making new column with the value.

-- Gives meaningful names to aggregate results.

SELECT COUNT(*) AS total_patients,
       AVG(weight) AS avg_weight
FROM patients;

-- Creates a readable allergy_status column.

SELECT first_name, last_name,
CASE 
         WHEN allergies = 'NKA' THEN 'No Known Allergies'
         ELSE allergies
END AS allergy_status
FROM patients;
-- Making a new column and put the condition inside the select block.
```

```sql
-- Show unique first names from the patients table which only occurs once in the list.
-- For example, if two or more people are named 'John' in the first_name column then don't include their name in the output list. If only 1 person is named 'Leo' then include them in the output.

SELECT first_name
FROM patients
GROUP BY first_name
HAVING COUNT(*) = 1
ORDER BY first_name;


-- Group By to make the group of same first_name and the count as 1 and then order by name.
```

### The table structure of the database.
```sql
-- Table: province_names
CREATE TABLE province_names (
    province_id CHAR(2) PRIMARY KEY,
    province_name VARCHAR(30)
);

-- Table: doctors
CREATE TABLE doctors (
    doctor_id INTEGER PRIMARY KEY,
    first_name VARCHAR(30),
    last_name VARCHAR(30),
    specialty VARCHAR(25)
);

-- Table: patients
CREATE TABLE patients (
    patient_id INTEGER PRIMARY KEY,
    first_name VARCHAR(30),
    last_name VARCHAR(30),
    gender CHAR(1),
    birth_date DATE,
    city VARCHAR(30),
    province_id CHAR(2),
    allergies VARCHAR(80),
    height DECIMAL(3,0),
    weight DECIMAL(4,0),
    FOREIGN KEY (province_id) REFERENCES province_names(province_id)
);

-- Table: admissions
CREATE TABLE admissions (
    patient_id INT,
    admission_date DATE,
    discharge_date DATE,
    diagnosis VARCHAR(50),
    attending_doctor_id INT,
    PRIMARY KEY (patient_id, admission_date),
    FOREIGN KEY (patient_id) REFERENCES patients(patient_id),
    FOREIGN KEY (attending_doctor_id) REFERENCES doctors(doctor_id)
);

```

![](../images/Database/SchemaRelation.png)

Adding sample data to the tables.

```sql
-- Insert data into province_names
INSERT INTO province_names (province_id, province_name)
VALUES
('AB', 'Alberta'),
('BC', 'British Columbia'),
('MB', 'Manitoba'),
('NB', 'New Brunswick'),
('NL', 'Newfoundland and Labrador'),
('NT', 'Northwest Territories');

-- Insert data into doctors
INSERT INTO doctors (doctor_id, first_name, last_name, specialty)
VALUES
(1, 'Claude', 'Walls', 'Internist'),
(2, 'Joshua', 'Green', 'Cardiologist'),
(3, 'Miriam', 'Tregre', 'General Surgeon'),
(4, 'James', 'Russo', 'Obstetrician/Gynecologist'),
(5, 'Scott', 'Hill', 'Gastroenterologist');

-- Insert data into patients
INSERT INTO patients (patient_id, first_name, last_name, gender, birth_date, city, province_id, allergies, height, weight)
VALUES
(1, 'Donald', 'Waterfield', 'M', '1963-02-12', 'Barrie', 'ON', NULL, 156, 65),
(2, 'Mickey', 'Baasha', 'M', '1981-05-28', 'Dundas', 'ON', 'Sulfa', 185, 76),
(3, 'Jiji', 'Sharma', 'M', '1957-09-05', 'Hamilton', 'ON', 'Penicillin', 194, 106),
(4, 'Blair', 'Diaz', 'M', '1967-01-07', 'Hamilton', 'ON', NULL, 191, 104),
(5, 'Charles', 'Wolfe', 'M', '2017-11-19', 'Orillia', 'ON', 'Penicillin', 47, 10);

-- Insert data into admissions
INSERT INTO admissions (patient_id, admission_date, discharge_date, diagnosis, attending_doctor_id)
VALUES
(1, '2018-11-06', '2018-11-08', 'Ovarian Dermoid-Cyct', 21),
(1, '2018-09-20', '2018-09-20', 'Ineffective Breathin Pattern R/T Fluid Accumulatio', 24),
(3, '2019-01-24', '2019-01-29', 'Cardiac Arrest', 2),
(3, '2018-10-21', '2018-10-27', 'Congestive Heart Failure', 8),
(6, '2018-06-13', '2018-06-15', 'Asthma Exacerbation', 3);
```

| province_id | province_name |
| --- | --- |
| AB | Alberta |
| BC | British Columbia |
| MB | Manitoba |
| NB | New Brunswick |
| NL | Newfoundland and Labrador |
| NT | Northwest Territories |

| doctor_id | first_name | last_name | specialty |
| --- | --- | --- | --- |
| 1 | Claude | Walls | Internist |
| 2 | Joshua | Green | Cardiologist |
| 3 | Miriam | Tregre | General Surgeon |
| 4 | James | Russo | Obstetrician/Gynecologist |
| 5 | Scott | Hill | Gastroenterologist |

| patient_id | admission_date | discharge_date | diagnosis | attending_doctor_id |
| --- | --- | --- | --- | --- |
| 1 | 2018-11-06 | 2018-11-08 | Ovarian Dermoid-Cyct | 21 |
| 1 | 2018-09-20 | 2018-09-20 | Ineffective Breathin Pattern R/T Fluid Accumulatio | 24 |
| 3 | 2019-01-24 | 2019-01-29 | Cardiac Arrest | 2 |
| 3 | 2018-10-21 | 2018-10-27 | Congestive Heart Failure | 8 |
| 6 | 2018-06-13 | 2018-06-15 | Asthma Exacerbation | 3 |


| patient_id | admission_date | discharge_date | diagnosis | attending_doctor_id |
| --- | --- | --- | --- | --- |
| 1 | 2018-11-06 | 2018-11-08 | Ovarian Dermoid-Cyct | 21 |
| 1 | 2018-09-20 | 2018-09-20 | Ineffective Breathin Pattern R/T Fluid Accumulatio | 24 |
| 3 | 2019-01-24 | 2019-01-29 | Cardiac Arrest | 2 |
| 3 | 2018-10-21 | 2018-10-27 | Congestive Heart Failure | 8 |
| 6 | 2018-06-13 | 2018-06-15 | Asthma Exacerbation | 3 |