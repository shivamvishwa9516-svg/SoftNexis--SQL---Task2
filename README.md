-- Task 2: Department wise Average Salary
-- Name: Tera Naam
-- Roll No: Tera Roll No
-- Date: 2026-05-26

-- Schema SQL
CREATE TABLE employees (
    emp_no INT NOT NULL,
    birth_date DATE NOT NULL,
    first_name VARCHAR(14) NOT NULL,
    last_name VARCHAR(16) NOT NULL,
    gender ENUM('M','F') NOT NULL,
    hire_date DATE NOT NULL,
    PRIMARY KEY (emp_no)
);

CREATE TABLE departments (
    dept_no CHAR(4) NOT NULL,
    dept_name VARCHAR(40) NOT NULL,
    PRIMARY KEY (dept_no),
    UNIQUE KEY (dept_name)
);

CREATE TABLE dept_emp (
    emp_no INT NOT NULL,
    dept_no CHAR(4) NOT NULL,
    from_date DATE NOT NULL,
    to_date DATE NOT NULL,
    FOREIGN KEY (emp_no) REFERENCES employees (emp_no) ON DELETE CASCADE,
    FOREIGN KEY (dept_no) REFERENCES departments (dept_no) ON DELETE CASCADE,
    PRIMARY KEY (emp_no,dept_no)
);

CREATE TABLE salaries (
    emp_no INT NOT NULL,
    salary INT NOT NULL,
    from_date DATE NOT NULL,
    to_date DATE NOT NULL,
    FOREIGN KEY (emp_no) REFERENCES employees (emp_no) ON DELETE CASCADE,
    PRIMARY KEY (emp_no, from_date)
);

INSERT INTO employees VALUES (10001,'1953-09-02','Georgi','Facello','M','1986-06-26');
INSERT INTO employees VALUES (10002,'1964-06-02','Bezalel','Simmel','F','1985-11-21');
INSERT INTO employees VALUES (10003,'1959-12-03','Parto','Bamford','M','1986-08-28');

INSERT INTO departments VALUES ('d001','Marketing');
INSERT INTO departments VALUES ('d002','Finance');
INSERT INTO departments VALUES ('d003','Human Resources');
INSERT INTO departments VALUES ('d004','Production');
INSERT INTO departments VALUES ('d005','Development');
INSERT INTO departments VALUES ('d006','Quality Management');
INSERT INTO departments VALUES ('d007','Sales');
INSERT INTO departments VALUES ('d008','Research');
INSERT INTO departments VALUES ('d009','Customer Service');

INSERT INTO dept_emp VALUES (10001,'d005','1986-06-26','9999-01-01');
INSERT INTO dept_emp VALUES (10002,'d007','1996-08-03','9999-01-01');
INSERT INTO dept_emp VALUES (10003,'d004','1995-12-03','9999-01-01');

INSERT INTO salaries VALUES (10001,60117,'1986-06-26','1987-06-26');
INSERT INTO salaries VALUES (10001,62102,'1987-06-26','1988-06-25');
INSERT INTO salaries VALUES (10002,65828,'1996-08-03','1997-08-03');
INSERT INTO salaries VALUES (10003,40006,'1995-12-03','1996-12-02');

-- Query SQL
SELECT 
    d.dept_name AS Department,
    COUNT(DISTINCT e.emp_no) AS Employees,
    ROUND(AVG(s.salary), 2) AS AvgSalary
FROM departments d
JOIN dept_emp de ON d.dept_no = de.dept_no
JOIN employees e ON de.emp_no = e.emp_no
JOIN salaries s ON e.emp_no = s.emp_no
WHERE de.to_date = '9999-01-01'
GROUP BY d.dept_name
ORDER BY AvgSalary DESC;

-- Output:
-- Department   | Employees | AvgSalary
-- Sales        | 1         | 65828.00
-- Development  | 1         | 61109.50
-- Production   | 1         | 40006.00
