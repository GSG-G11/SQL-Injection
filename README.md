# Script injections / safety issues
![](https://i.imgur.com/ZGT1KzU.png)

### 1- What is code injection and What are its types?
Code Injection is an attack perpetrated by an attackers ability to inject and execute malicious code into an application.

There are mainly four types of code injections: SQL injection, Script injection, Shell injection, and Dynamic evaluation.


### 2- What is a SQL injection and how do these happen?

SQL injection is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. It generally allows an attacker to view data that they are not normally able to retrieve. This might include data belonging to other users, or any other data that the application itself is able to access. In many cases, an attacker can modify or delete this data, causing persistent changes to the application's content or behavior.

---

### 3- How would you prevent script injections?
The only sure way to prevent SQL Injection attacks is input validation and parametrized queries including prepared statements. The application code should never use the input directly. The developer must sanitize all input, not only web form inputs such as login forms. They must remove potential malicious code elements such as single quotes. It is also a good idea to turn off the visibility of database errors on your production sites. Database errors can be used with SQL Injection to gain information about your database.

 **some points to prevent the SQL injection:**

1- Limiting the use of special characters.

![](https://i.imgur.com/hj0queM.png)

2- Providing minimal strict access to the minimum necessary privileges and nothing more.

3- Controlling activity by validating user inputs through the creation of an allow-list.

4- Use of prepared statements with parameterized queries because most instances of SQL injection can be prevented by using parameterized queries, Never concatenate user input that is not validated because String concatenation is the primary point of entry for script injection.

5- Never build Transact-SQL statements directly from user input.
**example => `' OR '1'='1`**

![](https://i.imgur.com/0mDn22K.png)

---

### 4- What is the Parameterized query?
* A parameterized query is a query in which placeholders are used for parameters and the parameter values are supplied at execution time.

* Why we use Parameterized Query?
1- To avoid SQL injection attacks.
2- To increase Performance of the database.

* Example:
> ![](https://i.imgur.com/tgRe3jl.png)

1- Non-Parameterized query
```js
const name = 'Ahmed';
const location = 'Gaza';
return connection.query({
        text: `INSERT INTO users (name, location)VALUES (${name}, ${location});`});
```
    
2- Parameterized query

```js
const name = 'Ahmed';
const location = 'Gaza';
return connection.query({
        text: 'INSERT INTO users (name, location) VALUES ($1, $2);',
        values: [name, location]
    });
```
    
The variables `name` and `location` are populated before being used in the INSERT statement to create a record in database table. 
In the first code, Parameters values are passed immediately after the INSERT statement.
In the second code, The $1,$2 in the VALUES clause of the INSERT statement are placeholders for the variables at run-time.

---

### 5- Bad practices in `SQL` and some sampels codes:
#### Bad Practices:

* Developing and deploying code susceptible to SQL Injection.
* No testing.
* No security.
* No maintenance.
* No service packs.
* Unnecessary coding techniques.
* No code reviews.

#### Sample Code:

Consider a shopping application that displays products in different categories. When the user clicks on the Gifts category, their browser requests the URL:

```
https://insecure-website.com/products?category=Gifts
```

This causes the application to make an SQL query to retrieve details of the relevant products from the database:

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```
The restriction released = 1 is being used to hide products that are not released. For unreleased products, presumably released = 0.

The application doesn't implement any defenses against SQL injection attacks, so an attacker can construct an attack like:

```
https://insecure-website.com/products?category=Gifts'--
```

This results in the SQL query:

```sql
SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1
```
The key thing here is that the double-dash sequence -- is a comment indicator in SQL, and means that the rest of the query is interpreted as a comment. This effectively removes the remainder of the query, so it no longer includes AND released = 1. This means that all products are displayed, including unreleased products.

---
**References:**
- [SQL injection W3school](https://www.w3schools.com/sql/sql_injection.asp)
- [SQL injection Ports Wigger](https://portswigger.net/web-security/sql-injection)
- [Bad Habits and Best Practices](https://sqlblog.org/bad-habits)
- [SQL Server Worst Practices](https://www.mssqltips.com/sqlservertip/1707/sql-server-worst-practices/)
