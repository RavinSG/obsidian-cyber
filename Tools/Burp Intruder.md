
Intruder is Burp Suite's built-in fuzzing tool that allows for *automated request modification and repetitive testing* with variations in input values. By using a captured request (often from the Proxy module), Intruder can send multiple requests with slightly altered values based on user-defined configurations.

It serves various purposes, such as *brute-forcing login forms* by substituting username and password fields with values from a wordlist or performing fuzzing attacks *using wordlists to test subdirectories, endpoints, or virtual hosts*. Intruder's functionality is comparable to command-line tools like Wfuzz or ffuf.

![[Burp Intruder.png]]

There are four sub-tabs within Intruder:

- **Positions**: This tab allows us to *select an attack type* and configure where we want to insert our payloads in the request template.

- **Payloads**: Here we can *select values to insert into the positions defined* in the Positions tab. We have various payload options, such as loading items from a wordlist. The way these payloads are inserted into the template depends on the attack type chosen in the Positions tab. The Payloads tab also enables us to *modify Intruder's behaviour regarding payloads*, such as defining pre-processing rules for each payload (e.g., adding a prefix or suffix, performing match and replace, or skipping payloads based on a defined regex).

- **Resource Pool**: This tab is not particularly useful in the Burp Community Edition. It allows for *resource allocation among various automated tasks* in Burp Professional. Without access to these automated tasks, this tab is of limited importance. 

- **Settings**: This tab allows us to *configure attack behaviour*. It primarily deals with how Burp handles results and the attack itself. For instance, we can flag requests containing specific text or define Burp's response to redirect (3xx) responses.

### Positions

Burp Suite automatically attempts to identify the most probable positions where payloads can be inserted. These positions are highlighted in green and enclosed by section marks `(§).`

On the right-hand side of the interface, we find the following buttons: `Add §`, `Clear §`, and `Auto §`:

- The `Add §` button allows us to *define new positions* manually by highlighting them within the request editor and then clicking the button.
- The `Clear § `button *removes all defined positions*, providing a blank canvas where we can define our own positions.
- The `Auto §` button *automatically attempts to identify* the most likely positions based on the request. This feature is helpful if we previously cleared the default positions and want them back.

### Payload
