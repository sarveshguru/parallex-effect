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
