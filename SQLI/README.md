# SQL Injection - THM Room

### Useful SQLi Commands

Input into a password field as this will always return true - ' OR 1=1;--

### Remediation

Prepared Statements (With Parameterized Queries):

In a prepared query, the first thing a developer writes is the SQL query and then any user inputs are added as a parameter afterwards. Writing prepared statements ensures that the SQL code structure doesn't change and the database can distinguish between the query and the data. As a benefit, it also makes your code look a lot cleaner and easier to read.


Input Validation:

Input validation can go a long way to protecting what gets put into an SQL query. Employing an allow list can restrict input to only certain strings, or a string replacement method in the programming language can filter the characters you wish to allow or disallow. 


Escaping User Input:

Allowing user input containing characters such as ' " $ \ can cause SQL Queries to break or, even worse, as we've learnt, open them up for injection attacks. Escaping user input is the method of prepending a backslash (\) to these characters, which then causes them to be parsed just as a regular string and not a special character.