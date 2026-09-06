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