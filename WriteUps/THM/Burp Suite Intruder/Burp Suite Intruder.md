---
URL: https://tryhackme.com/room/burpsuiteintruder
---
## Logging into the Support Page

Our initial goal is to find a way to get access to the `http://10.10.181.83/support/login` page. When we visit the page from the browser, we are greeted with the following page. 

![[Support Login.png |]]
![[Support HTML.png]]

We can see that there are no protective measure implemented in the form. Hence we can take a brute force approach to crack the login page.

However, due to a data-leak the usernames and passwords of the company was leaked few months ago and this leaked list can be downloaded through:
`wget http://10.10.181.83:9999/Credentials/BastionHostingCreds.zip`

![[Credentials.png]]

After unzipping the file, we have access to the list of usernames and passwords of the users. After the breach most users should have changed their passwords. However, we can try and see whether at least 1 of the users did not change their password by performing a **Credential Stuffing** attack.

By capturing a form request with Burp suite proxy, we get the following output. 

![[Form request.png]]

We can now use the `Burp Suite Intruder` with a `Pitchfork` attack to enumerate through the lists we have. We provide the `usernames.txt` as payload1 and `passwords.txt` as payload2 and start the attack.

Since all responses are 302 redirects, we need to monitor the response length to distinguish a successful attack. The request with the lowest response length indicates a successful login attempt. 

![[Attack Results.png]]

In this case the username is `m.rivera` and the password is `letmein1`. By entering those credentials, we successfully log in to the support page.

![[Login Success.png]]

## Support Ticket IDOR

After accessing the support page, we can then inspect the tickets available for the user. We can see that the tickets URLs are formatted in the following manner:

`http://10.10.181.83/support/ticket/<NUMBER>`

This indicates two scenarios:

- **Access control** - The endpoint maybe properly configured that we can only retrieve the tickets assigned for the user
- **IDOR Vulnerability** - The endpoint lack access controls leading to a [[Insecure Direct Object Reference]], where we can retrieve any ticket by changing the `<NUMBER>`

We can use the Burp Suite **Sniper** attack to check all ticket numbers from `1-100`. We just need to intercept the request with the proxy and forward it to the **Intruder** again.

![[Intruder Sniper Login.png]]

> [!tip] Tip
> The ip address changes from this point onwards since the initial machine got terminated

In this case, we can use a numbers payload to initiate the attack. After enumerating through the numbers, we get the following successful responses. 

![[Ticket IDOR.png|350]] ![[Flag 1.png|300]]

Visiting the page for ticker number 83 provides us with the flag: `THM{MTMxNTg5NTUzMWM0OWRlYzUzMDVjMzJl}` 

## Admin Login

After capturing a request to `http://10.10.245.211/admin/login/` and reviewing the response we can see the following:

```HTTP
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Date: Fri, 20 Aug 2021 22:31:16 GMT
Content-Type: text/html; charset=utf-8
Connection: close
Set-Cookie: session=eyJ0b2tlbklEIjoiMzUyNTQ5ZjgxZDRhOTM5YjVlMTNlMjIzNmI0ZDlkOGEifQ.YSA-mQ.ZaKKsUnNsIb47sjlyux_LN8Qst0; HttpOnly; Path=/
Vary: Cookie
Front-End-Https: on
Content-Length: 3922
---

<form method="POST">
    <div class="form-floating mb-3">
        <input class="form-control" type="text" name=username  placeholder="Username" required>
        <label for="username">Username</label>
    </div>
    <div class="form-floating mb-3">
        <input class="form-control" type="password" name=password  placeholder="Password" required>
        <label for="password">Password</label>
    </div>
    <input type="hidden" name="loginToken" value="84c6358bbf1bd8000b6b63ab1bd77c5e">
    <div class="d-grid"><button class="btn btn-warning btn-lg" type="submit">Login!</button></div>
</form>
```

We notice that along the username and password, there is now a *session cookie set*, as well as a **CSRF** (Cross-Site Request Forgery) **token** in the form as a hidden field.

Refreshing the page indicates that both fields change with each request. Which means, for every login attempt, we need to extract valid values for both these fields.

We can accomplish this using the **Burp Macros**. A macro can be used to define a set of action to be performed before a request. This macro will extract unique values for the session cookie and loginToken, replacing them in every subsequent request of our attack.

Same as before, we capture a form post to the server using the Burp Proxy. We can use Pitchfork to enumerate the username and password fields. However, we need to setup a macro to populate the cookie and loginToken field with the correct values from the server.

![[Admin Login Request.png]]

### Setting up Macro

Click on the settings icon on the top right and navigate to sessions. Scroll down and select the Macros pane and click add. We are prompted with the following pane:

![[Adding a Macro.png]]

- Select the GET request to the `/admin/login/` page and click ok and ok again to add the Macro. We can give it a name if we want as well.
- Now scroll to the top and under `Session Handling Rules` and click Add.
- Give the rule a description if needed and click on Add under `Rule actions`. Then select run a macro from the drop down menu.
- Select the Macro we created before and change the options below to update only the `loginToken` parameter and the `session` cookie as below.

![[Session Handling Action.png]]

- Click ok and click on the sub-tab `Scope`. Since we only want this action to be applied within the intruder, we can deselect the rest under `Tools Scope`.
- Under `URL Scope`, select the option `Use suite scope [defined in Target tab]` and make sure the host IP address `10.10.245.211` is selected as the scope.

Now the macro is set. We can start a **Pitchfork** attack with the `username.txt` and `password.txt `files for the admin login form.

However, unlike the support page, since all response codes are `302 (Redirected)`, we will be getting response with different lengths. Still, we can look for one with the lowest length to test for a credential match.

![[Admin Credential Stuffing.png]]

We see that the username `o.bennett` and the password `bella1` has a length less than rest of the responses. Providing them to the form grants us with access to the admin home!

![[Admin Access.png]]
