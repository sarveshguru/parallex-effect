# parallex-effect

1. List the name, employee code and designation code of each employee of the office
SELECT empname, empcode, desigcode FROM emp;

2. List all the departments and the budgets
SELECT deptname, budget FROM dept;

3. List the employees and their respective department names
SELECT e.empname, d.deptname FROM emp e JOIN dept d ON e.deptcode = d.deptcode;

4. List the employees who are not having any superior to work under
SELECT empname FROM emp WHERE supcode IS NULL;

5. List the employees who are working directly under the superior most employee of the office
SELECT e1.empname FROM emp e1 WHERE e1.supcode = (SELECT empcode FROM emp WHERE supcode IS NULL);

6. List the employee(s) who is senior most in the office
SELECT empname FROM emp ORDER BY joindate ASC LIMIT 1;

7. List the employees who will retire from the office next
SELECT empname FROM emp ORDER BY birthdate ASC LIMIT 1;

8. List the departments with the respective department managers
SELECT d.deptname, e.empname FROM dept d JOIN emp e ON d.deptcode = e.deptcode WHERE e.desigcode = 'Manager';

9. List the number of employees working for either ‘accounts’ or ‘personal’ or ‘purchase’ departments
SELECT COUNT(*) FROM emp WHERE deptcode IN (SELECT deptcode FROM dept WHERE deptname IN ('accounts', 'personal', 'purchase'));

10. List the employees working for ‘accounts’ or ‘personal’ department
SELECT empname FROM emp WHERE deptcode IN (SELECT deptcode FROM dept WHERE deptname IN ('accounts', 'personal'));

11. List the employees working for ‘accounts’ and ‘personal’ department
SELECT empname FROM emp WHERE deptcode IN (SELECT deptcode FROM dept WHERE deptname = 'accounts') AND empcode IN (SELECT empcode FROM emp WHERE deptcode IN (SELECT deptcode FROM dept WHERE deptname = 'personal'));

12. List the employees working for ‘accounts’ but not for ‘personal’ department
SELECT empname FROM emp WHERE deptcode IN (SELECT deptcode FROM dept WHERE deptname = 'accounts') AND empcode NOT IN (SELECT empcode FROM emp WHERE deptcode IN (SELECT deptcode FROM dept WHERE deptname = 'personal'));

13. List the youngest employee of the office
SELECT empname FROM emp ORDER BY birthdate DESC LIMIT 1;

14. List the employees who are drawing basic pay not equal to 12400
SELECT empname FROM emp WHERE basicpay <> 12400;

15. List the employees who are drawing basic salary between 11000 and 12000
SELECT empname FROM emp WHERE basicpay BETWEEN 11000 AND 12000;

16. List the employees who are drawing basic salary not between 11000 and 12000
SELECT empname FROM emp WHERE basicpay NOT BETWEEN 11000 AND 12000;

17. List the employees who got salary allowance between Rs.1000 to Rs.1500 in the month of January 2012
SELECT empcode FROM salary WHERE salmonth = '2012-01-01' AND allow BETWEEN 1000 AND 1500;

18. List the employees whose name ends with ‘i’ or ‘y’
SELECT empname FROM emp WHERE empname LIKE '%i' OR empname LIKE '%y';

19. List the employees who have at least 25 years of experience
SELECT empname FROM emp WHERE DATEDIFF(CURDATE(), joindate) / 365 >= 25;

20. List the ‘Salesmen’ who have minimum 30 to 20 years of experience
SELECT empname FROM emp WHERE desigcode = 'Salesman' AND DATEDIFF(CURDATE(), joindate) / 365 BETWEEN 20 AND 30;

21. List the basic salary and half of the basic salary for each employee
SELECT empname, basicpay, basicpay / 2 AS half_salary FROM emp;

22. List the employees and the latest take-home-pay of each employee
SELECT e.empname, (s.basic + s.allow - s.deduct) AS take_home_pay FROM emp e JOIN salary s ON e.empcode = s.empcode ORDER BY s.salmonth DESC;

23. List the employees and the latest take-home-pay of each employee of ‘Accounts’ department
SELECT e.empname, (s.basic + s.allow - s.deduct) AS take_home_pay FROM emp e JOIN salary s ON e.empcode = s.empcode WHERE e.deptcode IN (SELECT deptcode FROM dept WHERE deptname = 'accounts') ORDER BY s.salmonth DESC;

24. List employees and their respective ages
SELECT empname, TIMESTAMPDIFF(YEAR, birthdate, CURDATE()) AS age FROM emp;

25. List all the ‘Accounts’ department employees, first ordered by their age and then by their names
SELECT empname, TIMESTAMPDIFF(YEAR, birthdate, CURDATE()) AS age FROM emp WHERE deptcode IN (SELECT deptcode FROM dept WHERE deptname = 'accounts') ORDER BY age ASC, empname ASC;

YEAR(changedate) = 1991;


26. List the number of employees directly reporting to â€˜Reddyâ€™:
SELECT COUNT(*) FROM emp WHERE supcode = (SELECT empcode FROM emp WHERE empname = 'Reddy');


27. List employees who have at least one subordinate and their number of subordinates, ordered by highest:
SELECT e1.empname, COUNT(e2.empcode) AS subordinates FROM emp e1 JOIN emp e2 ON e1.empcode = e2.supcode GROUP BY e1.empname ORDER BY subordinates DESC;


28. List employees who have minimum 3 employees under them:
SELECT e1.empname FROM emp e1 JOIN emp e2 ON e1.empcode = e2.supcode GROUP BY e1.empname HAVING COUNT(e2.empcode) >= 3;


29. List the minimum and maximum salaries in each grade code:
SELECT gradecode, MIN(basicpay) AS min_salary, MAX(basicpay) AS max_salary FROM emp GROUP BY gradecode;


30. List employees and their supervisors:
SELECT e1.empname AS employee, e2.empname AS supervisor FROM emp e1 LEFT JOIN emp e2 ON e1.supcode = e2.empcode;


31. List number of officers reporting to each supervisor with more than 3 subordinates:
SELECT supcode, COUNT(empcode) FROM emp GROUP BY supcode HAVING COUNT(empcode) > 3;


32. List employees who have never been promoted:
SELECT empname FROM emp WHERE empcode NOT IN (SELECT empcode FROM history);


33. List employee with max promotions:
SELECT empcode, COUNT(*) AS promotions FROM history GROUP BY empcode ORDER BY promotions DESC LIMIT 1;


34. List employees promoted in 1991:
SELECT empcode FROM history WHERE YEAR(changedate) = 1991;
35. List department budget and total salary:
SELECT deptcode, SUM(basicpay) FROM emp GROUP BY deptcode;


36. Display employee names in uppercase:
SELECT UPPER(empname) FROM emp;


37. List employees earning more than â€˜Jainâ€™:
SELECT empname FROM emp WHERE basicpay > (SELECT basicpay FROM emp WHERE empname = 'Jain');


38. List employees earning more than all in 11000-12000 range:
SELECT empname FROM emp WHERE basicpay > ALL (SELECT basicpay FROM emp WHERE basicpay BETWEEN 11000 AND 12000);


39. List employees with greater than average pay in ascending order:
SELECT empname FROM emp WHERE basicpay > (SELECT AVG(basicpay) FROM emp) ORDER BY basicpay;


40. List highest paid employees:
SELECT empname FROM emp WHERE basicpay = (SELECT MAX(basicpay) FROM emp);


41. List all employees except highest-paid:
SELECT empname FROM emp WHERE basicpay < (SELECT MAX(basicpay) FROM emp);

-- 42. List the employees who draw the highest salary in each department
SELECT e.empname, e.deptcode, s.basic + s.allow - s.deduct AS salary
FROM emp e
JOIN salary s ON e.empcode = s.empcode
WHERE (e.deptcode, s.basic + s.allow - s.deduct) IN (
    SELECT deptcode, MAX(basic + allow - deduct)
    FROM emp e2
    JOIN salary s2 ON e2.empcode = s2.empcode
    GROUP BY deptcode
);

-- 43. List the employee(s) getting second highest salary
SELECT empname, empcode, total_salary
FROM (
    SELECT e.empname, e.empcode, s.basic + s.allow - s.deduct AS total_salary,
           DENSE_RANK() OVER (ORDER BY s.basic + s.allow - s.deduct DESC) AS salary_rank
    FROM emp e
    JOIN salary s ON e.empcode = s.empcode
) ranked
WHERE salary_rank = 2;

-- 44. List the employee(s) who are getting the fifth highest salary.
SELECT empname, empcode, total_salary
FROM (
    SELECT e.empname, e.empcode, s.basic + s.allow - s.deduct AS total_salary,
           DENSE_RANK() OVER (ORDER BY s.basic + s.allow - s.deduct DESC) AS salary_rank
    FROM emp e
    JOIN salary s ON e.empcode = s.empcode
) ranked
WHERE salary_rank = 5;

-- 45. List the department name of the female employee who draws the highest salary higher than any other female employee
SELECT d.deptname
FROM emp e
JOIN salary s ON e.empcode = s.empcode
JOIN dept d ON e.deptcode = d.deptcode
WHERE e.sex = 'F'
ORDER BY (s.basic + s.allow - s.deduct) DESC
LIMIT 1;

-- 46. List all male employees who draw salary greater than at least one female employee
SELECT e1.empname, e1.empcode, s1.basic + s1.allow - s1.deduct AS salary
FROM emp e1
JOIN salary s1 ON e1.empcode = s1.empcode
WHERE e1.sex = 'M'
AND (s1.basic + s1.allow - s1.deduct) > (
    SELECT MIN(s2.basic + s2.allow - s2.deduct)
    FROM emp e2
    JOIN salary s2 ON e2.empcode = s2.empcode
    WHERE e2.sex = 'F'
);

-- 47. List the departments in which the average salary of employees is more than the average salary of the company
SELECT d.deptname
FROM emp e
JOIN salary s ON e.empcode = s.empcode
JOIN dept d ON e.deptcode = d.deptcode
GROUP BY d.deptname
HAVING AVG(s.basic + s.allow - s.deduct) > (
    SELECT AVG(basic + allow - deduct)
    FROM salary
);

-- 48. List the employees drawing salary lesser than the average salary of employees working for ‘Accounts’ department
SELECT e.empname, s.basic + s.allow - s.deduct AS salary
FROM emp e
JOIN salary s ON e.empcode = s.empcode
WHERE (s.basic + s.allow - s.deduct) < (
    SELECT AVG(s2.basic + s2.allow - s2.deduct)
    FROM emp e2
    JOIN salary s2 ON e2.empcode = s2.empcode
    WHERE e2.deptcode = (SELECT deptcode FROM dept WHERE deptname = 'Accounts')
);
_-------+-------++++++--+--+--


1. Suppose you are given the following requirements for a simple database for the  
National Football League (NFL):  
â€¢ The NFL has many teams.  
â€¢ Each team has a name, a city, a coach, a captain, and a set of players.  
â€¢ Each player belongs to only one team.  
â€¢ Each player has a name, a position (such as left wing or goalie), a skill level, and a set of injury records.  
â€¢ A team captain is also a player.  
â€¢ A game is played between two teams (referred to as host_team and guest_team) and has a date (such as May 11th, 2021) and a score (such as 4 to 2).  
Perform the following activities:  
a. Construct a clean and concise ER diagram for the NFL database.  
b. Construct appropriate tables for the above ER Diagram.  

-- SQL Queries for NFL Database  
CREATE TABLE Team (  
    team_id INT PRIMARY KEY,  
    name VARCHAR(255),  
    city VARCHAR(255),  
    coach VARCHAR(255),  
    captain_id INT  
);  

CREATE TABLE Player (  
    player_id INT PRIMARY KEY,  
    name VARCHAR(255),  
    position VARCHAR(50),  
    skill_level VARCHAR(50),  
    team_id INT,  
    FOREIGN KEY (team_id) REFERENCES Team(team_id)  
);  

CREATE TABLE Injury_Record (  
    injury_id INT PRIMARY KEY,  
    player_id INT,  
    injury_details TEXT,  
    injury_date DATE,  
    FOREIGN KEY (player_id) REFERENCES Player(player_id)  
);  

CREATE TABLE Game (  
    game_id INT PRIMARY KEY,  
    host_team_id INT,  
    guest_team_id INT,  
    game_date DATE,  
    host_team_score INT,  
    guest_team_score INT,  
    FOREIGN KEY (host_team_id) REFERENCES Team(team_id),  
    FOREIGN KEY (guest_team_id) REFERENCES Team(team_id)  
);  

INSERT INTO Team VALUES (1, 'Eagles', 'Philadelphia', 'Doug Pederson', 3);  
INSERT INTO Team VALUES (2, 'Chiefs', 'Kansas City', 'Andy Reid', 5);  

INSERT INTO Player VALUES (1, 'John Doe', 'Left Wing', 'Expert', 1);  
INSERT INTO Player VALUES (2, 'Tom Smith', 'Goalie', 'Advanced', 2);  

INSERT INTO Injury_Record VALUES (1, 1, 'Sprained Ankle', '2021-05-11');  

INSERT INTO Game VALUES (1, 1, 2, '2021-05-12', 4, 2);  

-----------------------------------------------------------  

2. A university registrarâ€™s office maintains data about the following entities:  
â€¢ Courses, including number, title, credits, syllabus, and prerequisites.  
â€¢ Course offerings, including course number, year, semester, section number, instructor(s), timings, and classroom.  
â€¢ Students, including student-id, name, and program.  
â€¢ Instructors, including identification number, name, department, and title.  
â€¢ Further, the enrollment of students in courses and grades awarded to students in each course they are enrolled in must be appropriately modeled.  
Perform the following activities:  
a. Construct a clean and concise ER diagram for the registrarâ€™s office.  
b. Construct appropriate tables for the above ER Diagram.  

-- SQL Queries for University Database  
CREATE TABLE Course (  
    course_id INT PRIMARY KEY,  
    number VARCHAR(50),  
    title VARCHAR(255),  
    credits INT,  
    syllabus TEXT,  
    prerequisites TEXT  
);  

CREATE TABLE Course_Offering (  
    offering_id INT PRIMARY KEY,  
    course_id INT,  
    year INT,  
    semester VARCHAR(50),  
    section_number INT,  
    instructor_id INT,  
    timings VARCHAR(100),  
    classroom VARCHAR(50),  
    FOREIGN KEY (course_id) REFERENCES Course(course_id)  
);  

CREATE TABLE Student (  
    student_id INT PRIMARY KEY,  
    name VARCHAR(255),  
    program VARCHAR(255)  
);  

CREATE TABLE Instructor (  
    instructor_id INT PRIMARY KEY,  
    name VARCHAR(255),  
    department VARCHAR(255),  
    title VARCHAR(255)  
);  

CREATE TABLE Enrollment (  
    student_id INT,  
    course_id INT,  
    grade CHAR(2),  
    PRIMARY KEY (student_id, course_id),  
    FOREIGN KEY (student_id) REFERENCES Student(student_id),  
    FOREIGN KEY (course_id) REFERENCES Course(course_id)  
);  

INSERT INTO Course VALUES (101, 'CS101', 'Intro to CS', 3, 'CS Basics', 'None');  
INSERT INTO Course_Offering VALUES (1, 101, 2021, 'Spring', 1, 2, '9 AM - 11 AM', 'Room 101');  
INSERT INTO Student VALUES (1001, 'Alice', 'Computer Science');  
INSERT INTO Student VALUES (1002, 'Bob', 'Electrical Engineering');  
INSERT INTO Instructor VALUES (1, 'Dr. Smith', 'Computer Science', 'Professor');  
INSERT INTO Instructor VALUES (2, 'Dr. Lee', 'Electrical Eng', 'Associate Professor');  
INSERT INTO Enrollment VALUES (1001, 101, 'A');  

-----------------------------------------------------------  

3. Construct an ER Diagram for Company having the following details:  
â€¢ Company organized into DEPARTMENT.  
â€¢ Each department has a unique name and a particular employee who manages the department.  
â€¢ Start date for the manager is recorded.  
â€¢ Department may have several locations.  
â€¢ A department controls a number of PROJECTS. Projects have a unique name, number, and a single location.  
â€¢ Companyâ€™s EMPLOYEE name, ssno, address, salary, sex, and birth date are recorded.  
â€¢ An employee is assigned to one department but may work for several projects (not necessarily controlled by their department).  
â€¢ The number of hours/week an employee works on each project is recorded.  
â€¢ The immediate supervisor for the employee is recorded.  
â€¢ Employeeâ€™s DEPENDENTS are tracked for health insurance purposes (dependent name, birthdate, relationship to the employee).  

-- SQL Queries for Company Database  
CREATE TABLE Department (  
    dept_id INT PRIMARY KEY,  
    dept_name VARCHAR(255),  
    manager_id INT,  
    start_date DATE  
);  

CREATE TABLE Employee (  
    emp_id INT PRIMARY KEY,  
    name VARCHAR(255),  
    ssno VARCHAR(20) UNIQUE,  
    address TEXT,  
    salary DECIMAL(10,2),  
    sex CHAR(1),  
    birth_date DATE,  
    dept_id INT,  
    supervisor_id INT,  
    FOREIGN KEY (dept_id) REFERENCES Department(dept_id),  
    FOREIGN KEY (supervisor_id) REFERENCES Employee(emp_id)  
);  

CREATE TABLE Project (  
    proj_id INT PRIMARY KEY,  
    proj_name VARCHAR(255),  
    proj_number INT UNIQUE,  
    location VARCHAR(255),  
    dept_id INT,  
    FOREIGN KEY (dept_id) REFERENCES Department(dept_id)  
);  

CREATE TABLE Works_On (  
    emp_id INT,  
    proj_id INT,  
    hours_per_week DECIMAL(5,2),  
    PRIMARY KEY (emp_id, proj_id),  
    FOREIGN KEY (emp_id) REFERENCES Employee(emp_id),  
    FOREIGN KEY (proj_id) REFERENCES Project(proj_id)  
);  

CREATE TABLE Dependent (  
    dependent_id INT PRIMARY KEY,  
    emp_id INT,  
    dependent_name VARCHAR(255),  
    birth_date DATE,  
    relationship VARCHAR(50),  
    FOREIGN KEY (emp_id) REFERENCES Employee(emp_id)  
);  

INSERT INTO Department VALUES (1, 'IT', 3, '2020-01-01');  
INSERT INTO Employee VALUES (1, 'John Doe', '12345', '123 St.', 60000, 'M', '1990-01-01', 1, NULL);  
INSERT INTO Project VALUES (101, 'Website Dev', 1001, 'NY', 1);  
INSERT INTO Works_On VALUES (1, 101, 40);  
INSERT INTO Dependent VALUES (1, 1, 'Jane Doe', '2015-05-05', 'Daughter');  
