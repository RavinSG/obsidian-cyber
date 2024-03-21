
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

Boolean-based SQL Injection refers to the response we receive from our injection attempts, which could be a *true/false, yes/no, on/off, 1/0* or any response that can only have two outcomes. That outcome confirms that our SQL Injection payload was either successful or not.

We're presented with a mock browser with the following URL:

```HTTP
https://website.thm/checkuser?username=admin
```

The browser body contains  `{"taken":true}`. This API endpoint replicates a common feature found on many signup forms, which checks whether a username has already been registered to prompt the user to choose a different username. 

Because the taken value is set to true, we can assume the username admin is already registered. We can confirm this by changing the username in the mock browser's address bar from admin to `admin123`, and upon pressing enter, we'll see the value taken has now changed to `false`.

The SQL query that is processed looks like the following:

```SQL
select * from users where username = '%username%' LIMIT 1;
```

The *only input we have control over is the username* in the query string, and we'll have to use this to perform our SQL injection. Keeping the username as admin123, we can start appending to this to try and make the database confirm true things, changing the state of the taken field from false to true.

Like in previous levels, our first task is to establish the number of columns in the users' table, which we can achieve by using the UNION statement. Change the username value to the following:

```
admin123' UNION SELECT 1;-- 
```

As the web application has responded with the value taken as `false`, *we can confirm this is the incorrect value of columns*. Keep on adding more columns until we have a taken value of true.

Then we can work on the enumeration of the database. Our first task is to discover the database name. We can do this by using the built-in `database()` method and then using the like operator to try and find results that will return a true status.

```SQL
admin123' UNION SELECT 1,2,3 where database() like '%';--
```

We get a `true` response because, in the like operator, we just have the value of `%`, which will *match anything* as it's the wildcard value. 

If we change the wildcard operator to `a%`, we'll see the response goes back to `false`, which confirms that the database name *does not begin with the letter a.* We can cycle through all the letters, numbers and characters such as - and _ until we discover a match.

```SQL
admin123' UNION SELECT 1,2,3 where database() like 's%';--
```

The above provides a match. Now we move on to the next character of the database name until we find another true response, for example, `'sa%', 'sb%', 'sc%'`, etc. Keep on with this process until we discover all the characters of the database name, which is `sqli_three`.

We've established the database name, which we can now use to enumerate table names using a similar method by utilising the `information_schema` database. Try setting the username to the following value:

```SQL
admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'a%';--
```

This query looks for results in the `information_schema` database in the tables table where the database name matches `sqli_three`, and the *table name begins with the letter a*. As the above query results in a `false` response, we can confirm that there are *no tables* in the `sqli_three` database that *begin with the letter a*.

From brute force we find there is a table named `users` in the database.

```SQL
admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name='users';--
```

Lastly, we now need to *enumerate the column names* in the users table so we can properly search it for login credentials. Again, we can use the `information_schema` database and the information we've already gained to query it for column names. Using the payload below, we search the columns table where the database is equal to `sqli_three`, the table name is users, and the column name begins with the letter a.

```SQL
admin123' UNION SELECT 1,2,3 FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='sqli_three' and TABLE_NAME='users' and COLUMN_NAME like 'a%';
```

Again, we'll need to cycle through letters, numbers and characters until we find a match. As we're looking for multiple results, we'll have to add this to our payload each time we find a new column name to avoid discovering the same one. 

For example, once we've found the column named `id`, we'll append that to our original payload:

```SQL
admin123' UNION SELECT 1,2,3 FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='sqli_three' and TABLE_NAME='users' and COLUMN_NAME like 'a%' and COLUMN_NAME !='id';
```

Repeating this process three times will enable us to discover the columns' `id`, `username` and `password`. Which now we can use to query the users table for login credentials

```SQL
admin123' UNION SELECT 1,2,3 from users where username='admin' and password like 'a%
```

Cycling through all the characters, we'll discover the password is **3845**.

### Time Based

A time-based blind SQL injection is very similar to the above boolean-based one in that the same requests are sent, but *there is no visual indicator* of your queries being wrong or right this time. 

Instead, your indicator of a correct query is *based on the time the query takes to complete*. This time delay is introduced using built-in methods such as `SLEEP(x)` alongside the `UNION` statement. The `SLEEP()` method will only ever get executed upon a successful `UNION SELECT` statement. 

So, for example, when trying to establish the number of columns in a table, we would use the following query:

```SQL
admin123' UNION SELECT SLEEP(5);--
```

*If there was no pause* in the response time, we know that the query was *unsuccessful*, so like on previous tasks, we add another column:

```SQL
admin123' UNION SELECT SLEEP(5),2;--
```

This payload produced a 5-second delay, confirming the successful execution of the UNION statement and that there are two columns.

Now we can repeat the enumeration process from the Boolean-based SQL injection, adding the SLEEP() method to the UNION SELECT statement.

```SQL
admin123' UNION SELECT SLEEP(0.5), 2 FROM information_schema.columns WHERE table_schema = 'sqli_four' and table_name = 'users' and column_name like 'password';--
```

