
Applications that allow user input should perform validation of that input to *reduce the likelihood that it contains an attack*. Improper input handling practices can expose applications to injection attacks, cross-site scripting attacks, and other exploits.

The *most effective* form of input validation uses input **allow listing**, in which the developer describes the exact type of input that is expected from the user and then verifies that the input matches that specification before passing the input to other processes or servers.

> [!danger] Warning
> When performing input validation, it is very important to ensure that **validation occurs server-side** rather than within the client’s browser. Client-side validation is useful for providing users with feedback on their input, but it should never be relied on as a security control. It’s *easy* for hackers and penetration testers to *bypass browser-based input validation*.


It is often difficult to perform input whitelisting because of the nature of many fields that allow user input such as text boxes for descriptions. In such cases, developers might use input **deny listing** to control user input.

### Parameter Pollution

Parameter pollution works by *sending a web application more than one value for the same input variable*. For example, a web application might have a variable named `account` that is specified in a URL like this:

```http
http://www.mycompany.com/status.php?account=12345
```

An attacker might try to exploit this application by injecting SQL code into the application:

```http
http://www.mycompany.com/status.php?account=12345' OR 1=1;--
```

However, this string looks quite suspicious to a web application *firewall* and *would likely be blocked*. An attacker seeking to obscure the attack and bypass content filtering mechanisms might instead send a command with two different values for account:

```http
http://www.mycompany.com/status.php?account=1235&account=12345' OR 1=1;--
```

This approach relies on the premise that the web platform won’t handle this URL properly. It might *perform input validation on only the first* argument but then execute the second argument, allowing the injection attack to slip through the filtering technology.