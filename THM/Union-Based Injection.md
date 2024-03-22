
This type of Injection utilises the *SQL UNION operator alongside a SELECT* statement to return additional results to the page. This method is the most common way of extracting large amounts of data via an SQL Injection vulnerability.

The key to discovering error-based SQL Injection is to break the code's SQL query by trying certain characters until an error message is produced; these are most commonly single apostrophes ( `'` ) or a quotation mark (`"`).

![[THM Union SQL1.png]]

 Typing an apostrophe ( `'` ) after the id=1 and pressing enter returns an SQL error informing us of an error in our syntax. The fact that we've received this **error message confirms the existence of an SQL Injection vulnerability**. We can now exploit this vulnerability and use the error messages to learn more about the database structure.

Firstly, we'll try the `UNION` operator so we can receive an extra result if we choose it. Try setting the mock browsers id parameter to:

```SQL
1 UNION SELECT 1
```

We get the error:

```SQL
SQLSTATE[21000]: Cardinality violation: 1222 The used SELECT statements have a different number of columns
```

By increasing the number of columns after the union, we are successful with the following query:

``` SQL
1 UNION SELECT 1,2,3
```

But now we want to *display our data instead of the article*. The article is displayed because it takes the first returned result somewhere in the website's code and shows that. To get around that, *we need the first query to produce no results*. This can simply be done by changing the article ID from 1 to 0.

![[THM Union SQL 2.png]]

We now see the article is just made up of the result from the UNION select, returning the column values 1, 2, and 3.

We can start using these returned values to retrieve more useful information. First, we'll get the database name that we have access to:

```SQL
0 UNION SELECT 1,2,database()
```

We now see where the number 3 was previously displayed; it now shows the name of the database, which is `sqli_one`.

Our next query will gather a list of tables that are in this database.

```SQL
0 UNION SELECT 1,2,t FROM information_schema.tables WHERE table_schema = 'sqli_one'
```

The method `group_concat()` gets the specified column (in this case, table_name) from multiple returned rows and puts it into one string separated by commas. 

Every user of the database has access to the `information_schema` database, and it *contains information about all the databases and tables the user has access to*. 

In this particular query, we're interested in *listing all the tables* in the `sqli_one` database, which is `article` and `staff_users`. 

We can utilise the `information_schema` database again to find the structure of this table using the below query.

```SQL
0 UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'staff_users'
```

The information we want to retrieve has changed from `table_name` to `column_name`, the table we are querying in the `information_schema` database has changed from `tables` to `columns`, and we're searching for any rows where the `table_name` column has a value of `staff_users`.

![[THM Union SQL 3.png]]

We can use the username and password columns for our following query to retrieve the user's information.

```SQL
0 UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') FROM staff_users
```

We've also added ,'`:`', to split the username and password from each other. Instead of being separated by a comma, we've chosen the HTML `<br>` tag that *forces each result to be on a separate line* to make for easier reading.
