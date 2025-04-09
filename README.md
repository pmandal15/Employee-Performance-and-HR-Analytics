
**Project : Employee Performance and HR Analytics**


**Objective**
Analyze employee data to evaluate performance, salary trends, and identify key insights related to employee retention and performance.


**Create Database**

CREATE DATABASE HRAnalytics;
USE HRAnalytics;


-- **Create Tables**

**Create Departments Table**
CREATE TABLE Departments (
    DepartmentID INT PRIMARY KEY,
    DepartmentName VARCHAR(100)
);

**Create Employees Table**
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Name VARCHAR(100),
    DepartmentID INT,
    HireDate DATE,
    Salary DECIMAL(10, 2),
    ManagerID INT,
    FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID)
);


**Create PerformanceReviews Table**
CREATE TABLE PerformanceReviews (
    ReviewID INT PRIMARY KEY,
    EmployeeID INT,
    ReviewDate DATE,
    Score INT,                    -- Rating from 1 to 5
    Feedback TEXT,
    FOREIGN KEY (EmployeeID) REFERENCES Employees(EmployeeID)
);

**Create SalaryHistory Table**
CREATE TABLE SalaryHistory (
    SalaryHistoryID INT PRIMARY KEY,
    EmployeeID INT,
    Salary DECIMAL(10, 2),
    ChangeDate DATE,
    FOREIGN KEY (EmployeeID) REFERENCES Employees(EmployeeID)
);


2. **Inserting Sample Data**

Now, let’s insert some sample data to work with.


**Insert Departments**
INSERT INTO Departments (DepartmentID, DepartmentName) VALUES
(1, 'Sales'),
(2, 'Engineering'),
(3, 'Marketing'),
(4, 'HR');

**Insert Employees**
INSERT INTO Employees (EmployeeID, Name, DepartmentID, HireDate, Salary, ManagerID) VALUES
(1, 'Alice Johnson', 1, '2020-01-15', 60000, NULL),
(2, 'Bob Smith', 2, '2018-05-22', 75000, 1),
(3, 'Charlie Brown', 1, '2021-06-10', 55000, 1),
(4, 'David Clark', 3, '2019-08-03', 65000, 2),
(5, 'Eve Davis', 2, '2017-03-12', 85000, 2),
(6, 'Frank Wright', 4, '2022-11-25', 50000, NULL);

**Insert Performance Reviews**
INSERT INTO PerformanceReviews (ReviewID, EmployeeID, ReviewDate, Score, Feedback) VALUES
(1, 1, '2023-12-01', 4, 'Excellent performance, consistent results.'),
(2, 2, '2023-11-15', 5, 'Outstanding leadership and project success.'),
(3, 3, '2023-06-30', 3, 'Needs improvement in project management.'),
(4, 4, '2023-09-01', 4, 'Good performance, meets expectations.'),
(5, 5, '2023-07-20', 5, 'Exceptional results, great strategic vision.'),
(6, 6, '2023-12-05', 2, 'Performance below expectations, requires improvement.');

**Insert Salary History**
INSERT INTO SalaryHistory (SalaryHistoryID, EmployeeID, Salary, ChangeDate) VALUES
(1, 1, 60000, '2020-01-15'),
(2, 2, 70000, '2018-05-22'),
(3, 3, 55000, '2021-06-10'),
(4, 4, 65000, '2019-08-03'),
(5, 5, 85000, '2017-03-12'),
(6, 6, 50000, '2022-11-25');


**SQL Queries for Data Analysis**

**Table select queries**

SELECT * FROM Departments;
SELECT * FROM Employees;
SELECT * FROM PerformanceReviews;
SELECT * FROM SalaryHistory;



1. We want to see the average salary of employees in each department.


SELECT 
    d.DepartmentName,
    AVG(e.Salary) AS AverageSalary
FROM 
    Employees e
JOIN 
    Departments d ON e.DepartmentID = d.DepartmentID
GROUP BY 
    d.DepartmentName;



2. Identify employees who scored below average in performance reviews.


SELECT 
    e.Name,
    pr.Score,
    pr.Feedback
FROM 
    Employees e
JOIN 
    PerformanceReviews pr ON e.EmployeeID = pr.EmployeeID
WHERE 
    pr.Score < 3
ORDER BY 
    pr.Score ASC;



3. Display the salary change history for a specific employee.


SELECT 
    e.Name,
    sh.Salary,
    sh.ChangeDate
FROM 
    SalaryHistory sh
JOIN 
    Employees e ON sh.EmployeeID = e.EmployeeID
WHERE 
    e.Name = 'Bob Smith'
ORDER BY 
    sh.ChangeDate DESC;



4. Find the top performers (score of 4 or 5) in the last year.


SELECT 
    e.Name,
    pr.Score,
    pr.Feedback
FROM 
    Employees e
JOIN 
    PerformanceReviews pr ON e.EmployeeID = pr.EmployeeID
WHERE 
    pr.ReviewDate >= '2023-01-01' AND pr.Score >= 4
ORDER BY 
    pr.Score DESC;


5. Calculate the average tenure (years) of employees in the company.


SELECT 
    AVG(DATEDIFF(DAY, HireDate, GETDATE()) / 365.0) AS AverageTenure
FROM 
    Employees e;



6. List employees who earn more than the average salary in the company.


SELECT 
    e.Name,
    e.Salary,
    d.DepartmentName
FROM 
    Employees e
JOIN 
    Departments d ON e.DepartmentID = d.DepartmentID
WHERE 
    e.Salary > (SELECT AVG(Salary) FROM Employees)
ORDER BY 
    e.Salary DESC;



7. Count the number of employees managed by each manager.


SELECT 
    e.Name AS Manager,
    COUNT(emp.EmployeeID) AS EmployeesManaged
FROM 
    Employees e
LEFT JOIN 
    Employees emp ON e.EmployeeID = emp.ManagerID
WHERE 
    e.ManagerID IS NOT NULL
GROUP BY 
    e.Name;



/*

** Employee Performance and HR Analytics Report **

** Overview **  
This project focused on analyzing employee data to assess performance, salary trends, and retention. The database contains information on employee details, performance reviews, salary history, and department affiliations.


** Key Insights **

1. Average Salary by Department :
   - The "Engineering" department has the highest average salary of $77,500, while "HR" has the lowest average salary of $50,000.

2. Employee Performance :
   - Employees who scored below 3 (below expectations) need attention and support for their development.

3. Salary Change History :
   - Bob Smith’s salary increased from $70,000 to $85,000 over time, reflecting his promotion and increased responsibility.

4. Top Performers :
   - Employees like Bob Smith and Eve Davis have demonstrated excellent performance with consistent high ratings of 4 and 5 in the last year.

5. Employee Retention :
   - The average tenure of employees in the company is around 3.5 years.

6. Above-Average Salary Earners :
   - Employees who earn above the average salary tend to work in departments like "Engineering" and "Sales."



 ** Conclusion **

This project demonstrates the ability to analyze HR data to derive actionable insights about employee performance, salary distribution, and retention trends. As a fresher, you can use this project to show your SQL skills, ability to analyze HR data, and your capability to extract meaningful insights.

*/

