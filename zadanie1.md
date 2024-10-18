## Tworzenie Bazy

CREATE TABLE REGIONS (
    region_id NUMBER PRIMARY KEY,
    region_name VARCHAR2(25)
);

CREATE TABLE COUNTRIES (
    country_id CHAR(2) PRIMARY KEY,
    country_name VARCHAR2(40),
    region_id NUMBER,
    FOREIGN KEY (region_id) REFERENCES REGIONS(region_id)
);

CREATE TABLE LOCATIONS (
    location_id NUMBER PRIMARY KEY,
    street_address VARCHAR2(40),
    postal_code VARCHAR2(12),
    city VARCHAR2(30),
    state_province VARCHAR2(25),
    country_id CHAR(2),
    FOREIGN KEY (country_id) REFERENCES COUNTRIES(country_id)
);

CREATE TABLE DEPARTMENTS (
    department_id NUMBER PRIMARY KEY,
    department_name VARCHAR2(30) NOT NULL,
    manager_id NUMBER,
    location_id NUMBER,
    FOREIGN KEY (location_id) REFERENCES LOCATIONS(location_id)
);

CREATE TABLE JOBS (
    job_id VARCHAR2(10) PRIMARY KEY,
    job_title VARCHAR2(35),
    min_salary NUMBER,
    max_salary NUMBER,
    CHECK (min_salary + 2000 <= max_salary)
);

CREATE TABLE EMPLOYEES (
    employee_id NUMBER PRIMARY KEY,
    first_name VARCHAR2(20),
    last_name VARCHAR2(25) NOT NULL,
    email VARCHAR2(25) NOT NULL,
    phone_number VARCHAR2(20),
    hire_date DATE NOT NULL,
    job_id VARCHAR2(10),
    salary NUMBER,
    commission_pct NUMBER,
    manager_id NUMBER,
    department_id NUMBER,
    FOREIGN KEY (job_id) REFERENCES JOBS(job_id),
    FOREIGN KEY (department_id) REFERENCES DEPARTMENTS(department_id)
);

CREATE TABLE JOB_HISTORY (
    employee_id NUMBER,
    start_date DATE,
    end_date DATE,
    job_id VARCHAR2(10),
    department_id NUMBER,
    PRIMARY KEY (employee_id, start_date),
    FOREIGN KEY (employee_id) REFERENCES EMPLOYEES(employee_id),
    FOREIGN KEY (job_id) REFERENCES JOBS(job_id),
    FOREIGN KEY (department_id) REFERENCES DEPARTMENTS(department_id)
);
## Usuwanie Tabeli
SELECT constraint_name
FROM user_constraints
WHERE table_name='COUNTRIES'
    AND constraint_type = 'R';

ALTER TABLE COUNTRIES
DROP CONSTRAINT SYS_C00194813;

DROP TABLE REGIONS;
## Odzyskiwanie Tabeli
FLASHBACK TABLE REGIONS TO BEFORE DROP;

ALTER TABLE COUNTRIES
ADD CONSTRAINT fk_region
FOREIGN KEY (region_id) REFERENCES REGIONS(region_id);
