
Blind XSS is similar to a stored XSS in that your payload gets stored on the website for another user to view, but in this instance, you can't see the payload working or be able to test it against yourself first.

Example Scenario:

A website has a contact form where you can message a member of staff. The message content doesn't get checked for any malicious code, which allows the attacker to enter anything they wish. These messages then get turned into support tickets which staff view on a private web portal.

Potential Impact:

Using the correct payload, the attacker's JavaScript could make calls back to an attacker's website, *revealing the staff portal URL, the staff member's cookies, and even the contents of the portal page* that is being viewed. Now the attacker could potentially hijack the staff member's session and have access to the private portal.