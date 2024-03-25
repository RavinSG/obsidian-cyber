
We will start by taking a look at the support form

![[Burp Support Form.png]]

When `<script>alert("Succ3ssful XSS")</script>` is typed into the "Contact Email" field, we find that there is a client-side filter in place which prevents you from adding any special characters that aren't allowed in email addresses. Which is done by the following script.

![[Burp Client-Side Filter.png]]

Fortunately for us, *client-side filters are absurdly easy to bypass*. There are a variety of ways we could disable the script or just prevent it from loading in the first place.

First, make sure that our Burp Proxy is active and that intercept is on.

Now, enter some legitimate data into the support form. For example: `pentester@example.thm` as an email address, and "Test Attack" as a query.

Submit the form — the *request should be intercepted by the proxy*.

With the request captured in the proxy, we can now change the email field to be our very simple payload from above: `<script>alert("Succ3ssful XSS")</script>`. After pasting in the payload, we need to select it, then *URL encode* it with the `Ctrl + U` shortcut to make it safe to send.
