# MySql-Query-For-A-Company-Sales-Analysis
MySql Query for a Project on Employee Salary And Client Management
### MySql Query for a Project on Employee Salary And Client Management

```Mysql
SQL  DATABASE CODE FOR COMPANY ANALYSIS
CREATE TABLE employee (
  emp_id INT PRIMARY KEY,
  first_name VARCHAR(40),
  last_name VARCHAR(40),
  birth_day DATE,
  sex VARCHAR(1),
  salary INT,
  super_id INT,
  branch_id INT
);

CREATE TABLE branch (
  branch_id INT PRIMARY KEY,
  branch_name VARCHAR(40),
  mgr_id INT,
  mgr_start_date DATE,
  FOREIGN KEY(mgr_id) REFERENCES employee(emp_id) ON DELETE SET NULL
);

ALTER TABLE employee
ADD FOREIGN KEY(branch_id)
REFERENCES branch(branch_id)
ON DELETE SET NULL;

ALTER TABLE employee
ADD FOREIGN KEY(super_id)
REFERENCES employee(emp_id)
ON DELETE SET NULL;

CREATE TABLE client (
  client_id INT PRIMARY KEY,
  client_name VARCHAR(40),
  branch_id INT,
  FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE SET NULL
);

CREATE TABLE works_with (
  emp_id INT,
  client_id INT,
  total_sales INT,
  PRIMARY KEY(emp_id, client_id),
  FOREIGN KEY(emp_id) REFERENCES employee(emp_id) ON DELETE CASCADE,
  FOREIGN KEY(client_id) REFERENCES client(client_id) ON DELETE CASCADE
);

CREATE TABLE branch_supplier (
  branch_id INT,
  supplier_name VARCHAR(40),
  supply_type VARCHAR(40),
  PRIMARY KEY(branch_id, supplier_name),
  FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE CASCADE
);


-- -----------------------------------------------------------------------------

-- Corporate
INSERT INTO employee VALUES(100, 'David', 'Wallace', '1967-11-17', 'M', 250000, NULL, NULL);

INSERT INTO branch VALUES(1, 'Corporate', 100, '2006-02-09');

UPDATE employee
SET branch_id = 1
WHERE emp_id = 100;

INSERT INTO employee VALUES(101, 'Jan', 'Levinson', '1961-05-11', 'F', 110000, 100, 1);

-- Scranton
INSERT INTO employee VALUES(102, 'Michael', 'Scott', '1964-03-15', 'M', 75000, 100, NULL);

INSERT INTO branch VALUES(2, 'Scranton', 102, '1992-04-06');

UPDATE employee
SET branch_id = 2
WHERE emp_id = 102;

INSERT INTO employee VALUES(103, 'Angela', 'Martin', '1971-06-25', 'F', 63000, 102, 2);
INSERT INTO employee VALUES(104, 'Kelly', 'Kapoor', '1980-02-05', 'F', 55000, 102, 2);
INSERT INTO employee VALUES(105, 'Stanley', 'Hudson', '1958-02-19', 'M', 69000, 102, 2);

-- Stamford
INSERT INTO employee VALUES(106, 'Josh', 'Porter', '1969-09-05', 'M', 78000, 100, NULL);

INSERT INTO branch VALUES(3, 'Stamford', 106, '1998-02-13');

UPDATE employee
SET branch_id = 3
WHERE emp_id = 106;

INSERT INTO employee VALUES(107, 'Andy', 'Bernard', '1973-07-22', 'M', 65000, 106, 3);
INSERT INTO employee VALUES(108, 'Jim', 'Halpert', '1978-10-01', 'M', 71000, 106, 3);


-- BRANCH SUPPLIER
INSERT INTO branch_supplier VALUES(2, 'Hammer Mill', 'Paper');
INSERT INTO branch_supplier VALUES(2, 'Uni-ball', 'Writing Utensils');
INSERT INTO branch_supplier VALUES(3, 'Patriot Paper', 'Paper');
INSERT INTO branch_supplier VALUES(2, 'J.T. Forms & Labels', 'Custom Forms');
INSERT INTO branch_supplier VALUES(3, 'Uni-ball', 'Writing Utensils');
INSERT INTO branch_supplier VALUES(3, 'Hammer Mill', 'Paper');
INSERT INTO branch_supplier VALUES(3, 'Stamford Lables', 'Custom Forms');

-- CLIENT
INSERT INTO client VALUES(400, 'Dunmore Highschool', 2);
INSERT INTO client VALUES(401, 'Lackawana Country', 2);
INSERT INTO client VALUES(402, 'FedEx', 3);
INSERT INTO client VALUES(403, 'John Daly Law, LLC', 3);
INSERT INTO client VALUES(404, 'Scranton Whitepages', 2);
INSERT INTO client VALUES(405, 'Times Newspaper', 3);
INSERT INTO client VALUES(406, 'FedEx', 2);

-- WORKS_WITH
INSERT INTO works_with VALUES(105, 400, 55000);
INSERT INTO works_with VALUES(102, 401, 267000);
INSERT INTO works_with VALUES(108, 402, 22500);
INSERT INTO works_with VALUES(107, 403, 5000);
INSERT INTO works_with VALUES(108, 403, 12000);
INSERT INTO works_with VALUES(105, 404, 33000);
INSERT INTO works_with VALUES(107, 405, 26000);
INSERT INTO works_with VALUES(102, 406, 15000);
INSERT INTO works_with VALUES(105, 406, 130000);



```

### ANALYSIS 
QUESTION 1 : The names and the sales of employees that sold more than 30000
```MYSQL
SELECT first_name,Last_name,total_sales FROM employee JOIN works_with on employee.emp_id=works_with.Emp_id WHERE total_sales>30000
SELECT first_name,Last_name,total_sales FROM employee JOIN works_with on employee.emp_id=works_with.Emp_id WHERE total_sales>30000
```
NESTED QUERIES (I can use nested queries to get this same result above)
```MYSQL
SELECT employee.first_name, employee.last_name FROM employee WHERE employee.emp_id IN ( SELECT works_with.Emp_id FROM `works_with` WHERE total_sales>30000 )
```
QUESTION 2 : Find all clients who are handled by the branch that micheal scott manages, assuming you know the Micheal’s ID 
Answering this question using the nested queries
```MYSQL
SELECT client.client_name FROM client WHERE client.branch_id="2" WHERE client.client_id IN ( SELECT branch.branch_name FROM branch WHERE branch_name = "scranton" WHERE branch.branch_name IN ( SELECT employee.first_name,employee.last_name FROM `employee` WHERE employee.emp_id="102" ))
```
Answering this question using the regular shortcut method
```MYSQL
SELECT client_name FROM client JOIN branch ON client.branch_id=branch.branch_id WHERE branch.branch_id = "2"
```
ON DELETE SET NULL
This is about deleting entries in database when they have foreign keys
For on delete set null;
Example
  FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE SET NULL
If a foreign key is deleted, it set the entire value associated to NULL. If a foreign key is deleted it set the entire values like the primary key associated with this foreign key to NULL
FOR ON DELETE CASCADE
ONCE A FOREIGN KEY IS deleted, it delete the entire row of the data.
This is useful when we are interested in deleting a primary key. Cos a primary keel cannot have a NULL value. a primary key is crucial for the values in a row.


ADDING A NEW COLUMN TO AN EXISTING TABLE IN EXCEL
The Template is 
ALTER TABLE table_name
ADD column_name data_type;
ALTER TABLE employees ADD dEPARTMENT VARCHAR(255)

To add multiple column at once
ALTER TABLE employees ADD COLUMN department VARCHAR(100), ADD COLUMN hire_date DATE;
INPUTTING DATA TO A NEW COLUMN ADDED TO AN EXISTING TABLE IN EXCEL
```MYSQL
The Template is 
UPDATE table_name SET column_name = value WHERE condition;
UPDATE employees SET Age = 25 WHERE EmployeeID = 1
UPDATE employees SET dEPARTMENT = 'Sales' WHERE EmployeeID = 1
INPUTTING DATA INTO MULTIPLE COLUMN ADDED TO AN EXISTING TABLE IN EXCEL
UPDATE employees SET Age = 30, department = 'Marketing' WHERE EmployeeID = 2
```
Addition in SQL
```MYSQL
UPDATE `employees` set Age = Age + 2 WHERE EmployeeID =2
```
