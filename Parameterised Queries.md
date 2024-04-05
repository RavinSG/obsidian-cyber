
In a parameterised query, the client does not directly send SQL code to the database server. Instead, the client sends arguments to the server, which then *inserts* those *arguments into a precompiled query template*. This approach protects against injection attacks and *also improves database performance*.

Stored procedures are an example of an implementation of parameterised queries used by some database platforms.