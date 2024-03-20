---
aliases:
  - SQLi
---
**SQLi**  is an attack on a web application database server that causes malicious queries to be executed.

Take the following scenario where you've come across an online blog, and each blog entry has a unique ID number. The blog entries may be either set to public or private, depending on whether they're ready for public release. The URL for each blog entry may look something like this:

```HTTP
https://website.thm/blog?id=1
```

From the URL above, you can see that the blog entry selected comes from the id parameter in the query string. The web application needs to retrieve the article from the database and may use an SQL statement that looks something like the following:

```SQL
SELECT * from blog where id=1 and private=0 LIMIT 1;
```

Let's pretend article ID 2 is still locked as private, so it cannot be viewed on the website. We could now instead call the URL:
 
```HTTP
https://website.thm/blog?id=2;--
```

Which would then, in turn, produce the SQL statement:

```SQL
SELECT * from blog where id=2;-- and private=0 LIMIT 1;
```

The semicolon in the URL signifies the end of the SQL statement, and the two dashes cause everything afterwards to be treated as a comment. By doing this, you're just, in fact, running the query:

```SQL
SELECT * from blog where id=2;--
```
