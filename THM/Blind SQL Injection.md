
Unlike In-Band SQL injection, where we can see the results of our attack directly on the screen, blind SQLi is when *we get little to no feedback* to confirm whether our injected queries were, in fact, successful or not, this is because the error messages have been disabled, but the injection still works regardless. 

### Authentication Bypass

One of the most straightforward Blind SQL Injection techniques is when bypassing authentication methods such as login forms. 

Login forms that are connected to a database of users are often developed in such a way that the web application isn't interested in the content of the username and password *but more in whether the two make a matching pair in the users table*.

Taking the above information into account, it's unnecessary to enumerate a valid username/password pair. We just need to *create a database query that replies with a yes/true*.

![[THM Blind SQLi 1.png]]

We can see in the box labelled "SQL Query" that the query to the database is the following:

```SQL
select * from users where username='%username%' and password='%password%' LIMIT 1;
```

To make this into a query that always returns as true, we can enter the following into the username or password field:

```SQL
' OR 1=1;--
```

Which turns the SQL query into the following:

```SQL
select * from users where username='' and password='' OR 1=1;
```

Because 1=1 is a true statement and we've used an `OR` operator, this will *always cause the query to return as true*.

### Boolean Based
