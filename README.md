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
