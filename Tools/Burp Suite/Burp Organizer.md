
The Organizer module of Burp Suite is designed to help you *store and annotate copies of HTTP requests* that you may want to revisit later. This tool can be particularly useful for organising your penetration testing workflow. Here are some of its key features:

- You can *store requests that you want to investigate later*, save requests that you've already identified as interesting, or save requests that you want to add to a report later.

- You can send HTTP requests to Burp Organizer *from other Burp Modules* such as Proxy or Repeater. Each HTTP request that you send to Organizer is a read-only copy of the original request saved at the point you sent it to Organizer.

- Requests are stored in a table, which contains columns such as the request index number, the time the request was made, workflow status, Burp tool that the request was sent from, HTTP method, server hostname, URL file path, URL query string, number of parameters in the request, HTTP status code of the response, length of the response in bytes, and any notes that you have made.

![[Burp Organizer.png]]

