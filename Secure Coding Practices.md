
One of the best resources for secure coding practices is the Open Web Application Security Project ([[Open Worldwide Application Security Project|OWASP]]). OWASP provides a *regularly updated list of proactive controls* that is useful to review not only as a set of useful best practices, but also as a way to see how web application security threats change from year to year.

Here are OWASP’s top proactive controls with brief descriptions:

- **Define Security Requirements** Implement security throughout the development process.
  
- **Leverage Security Frameworks and Libraries** Preexisting security capabilities can make securing applications easier.
  
- **Secure Database Access** Prebuilt SQL queries to prevent injection and configure databases for secure access. 
  
- **Encode and Escape Data** Remove special characters. 
  
- **Validate All Inputs** Treat user input as untrusted and filter appropriately.
  
- **Implement Digital Identity** Use multi-factor authentication, secure password storage and recovery, and session handling.
  
- **Enforce Access Controls** Require all requests to go through access control checks, deny by default, and apply the principle of least privilege.
  
- **Protect Data Everywhere** Use encryption in transit and at rest.
  
- **Implement Security Logging and Monitoring** This helps detect problems and allows investigation after the fact.
  
- **Handle all Errors and Exceptions** Errors should not provide sensitive data, and applications should be tested to ensure that they handle problems gracefully.

