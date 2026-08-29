-- Create a sample employees table
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    department VARCHAR(50),
    salary NUMERIC(10,2),
    hire_date DATE
);

-- Insert 10 dummy records
INSERT INTO employees (first_name, last_name, email, department, salary, hire_date) VALUES
('John', 'Doe', 'john.doe@email.com', 'IT', 65000.00, '2020-01-15'),
('Jane', 'Smith', 'jane.smith@email.com', 'HR', 55000.00, '2019-03-22'),
('Mike', 'Johnson', 'mike.johnson@email.com', 'Finance', 70000.00, '2021-05-10'),
('Sarah', 'Wilson', 'sarah.wilson@email.com', 'IT', 62000.00, '2020-07-08'),
('David', 'Brown', 'david.brown@email.com', 'Marketing', 58000.00, '2018-11-30'),
('Emily', 'Davis', 'emily.davis@email.com', 'HR', 56000.00, '2022-02-14'),
('Chris', 'Taylor', 'chris.taylor@email.com', 'Finance', 72000.00, '2019-09-05'),
('Anna', 'Martinez', 'anna.martinez@email.com', 'IT', 68000.00, '2021-04-20'),
('Robert', 'Anderson', 'robert.anderson@email.com', 'Marketing', 60000.00, '2020-12-01'),
('Lisa', 'Thomas', 'lisa.thomas@email.com', 'Sales', 59000.00, '2023-01-12');



-- 5 more dummy INSERT statements for employees table
INSERT INTO employees (first_name, last_name, email, department, salary, hire_date) VALUES
('James', 'Garcia', 'james.garcia@email.com', 'Sales', 61000.00, '2021-08-17'),
('Maria', 'Rodriguez', 'maria.rodriguez@email.com', 'IT', 67000.00, '2022-06-25'),
('Daniel', 'Lee', 'daniel.lee@email.com', 'Finance', 74000.00, '2019-12-03'),
('Jennifer', 'White', 'jennifer.white@email.com', 'Marketing', 57000.00, '2023-03-11'),
('Kevin', 'Harris', 'kevin.harris@email.com', 'HR', 54000.00, '2020-10-28');



-- 5 more dummy INSERT statements for employees table
INSERT INTO employees (first_name, last_name, email, department, salary, hire_date) VALUES
('Amanda', 'Lewis', 'amanda.lewis@email.com', 'Sales', 63000.00, '2021-11-09'),
('Steven', 'Walker', 'steven.walker@email.com', 'IT', 69000.00, '2022-09-18'),
('Rachel', 'Hall', 'rachel.hall@email.com', 'Finance', 71000.00, '2020-04-22'),
('Mark', 'Allen', 'mark.allen@email.com', 'Marketing', 59000.00, '2023-05-30'),
('Patricia', 'Young', 'patricia.young@email.com', 'HR', 53000.00, '2019-07-15');
