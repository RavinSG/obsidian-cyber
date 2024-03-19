
Reflected XSS happens when user-supplied data in an HTTP request is included in the webpage source without any validation.

Example Scenario:

A website where if you enter incorrect input, an error message is displayed. The content of the error message gets taken from the error parameter in the query string and is built directly into the page source

![[Reflected XSS error parameter.png]]
![[Reflected XSS HTML.png]]

The application doesn't check the contents of the error parameter, which allows the attacker to insert malicious code.

![[Reflected XSS attack.png]]

