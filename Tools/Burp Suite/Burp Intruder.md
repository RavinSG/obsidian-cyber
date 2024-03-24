
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

In the Payloads tab of Burp Suite Intruder, we can *create, assign, and configure payloads* for our attack. This sub-tab is divided into four sections:

![[Burp Intruder Payload.png]]

#### Payload Sets

- This section allows us to choose the position for which we want to configure a payload set and select the type of payload we want to use.

- When using attack types that allow only a single payload set (Sniper or Battering Ram), the "Payload Set" dropdown will have only one option, regardless of the number of defined positions.

- If we use attack types that require multiple payload sets (Pitchfork or Cluster Bomb), there will be one item in the dropdown for each position.

- Note: When assigning numbers in the "Payload Set" dropdown for multiple positions, follow a top-to-bottom, left-to-right order. For example, with two positions (`username=§pentester§&password=§Expl01ted§`), the first item in the payload set dropdown would refer to the username field, and the second item would refer to the password field.

#### Payload settings

- This section provides options specific to the selected payload type for the current payload set.

- For example, when using the "Simple list" payload type, we can manually add or remove payloads to/from the set using the Add text box, Paste lines, or Load payloads from a file. The Remove button removes the currently selected line, and the Clear button clears the entire list. Be cautious with loading huge lists, as it may cause Burp to crash.

- Each payload type will have its own set of options and functionality. Explore the options available to understand the range of possibilities.

#### Payload Processing

- In this section, we can define rules to be applied to each payload in the set before it is sent to the target.

- For example, we can capitalise every word, skip payloads that match a regex pattern, or apply other transformations or filtering.

- While you may not use this section frequently, it can be highly valuable when specific payload processing is required for your attack.

#### Payload Encoding

- The section allows us to customise the encoding options for our payloads.

- By default, Burp Suite applies URL encoding to ensure the safe transmission of payloads. However, there may be cases where we want to adjust the encoding behaviour.

- We can override the default URL encoding options by modifying the list of characters to be encoded or unchecking the "URL-encode these characters" checkbox.

### Attack Types

- **Sniper**: It cycles through the payloads, inserting one payload at a time into each position defined in the request. Sniper attacks *iterate through all the payloads in a linear fashion*, allowing for precise and focused testing.

- **Battering ram**: The Battering ram attack type differs from Sniper in that it sends all payloads simultaneously, each payload inserted into its respective position. This attack type is *useful when testing for race conditions* or when payloads need to be sent concurrently.

- **Pitchfork**: The Pitchfork attack type enables the simultaneous testing of multiple positions with different payloads. It allows the tester to define multiple payload sets, each associated with a specific position in the request. Pitchfork attacks are effective when there are *distinct parameters that need separate testing*.

- **Cluster bomb**: The Cluster bomb attack type combines the Sniper and Pitchfork approaches. It performs a Sniper-like attack on each position but simultaneously tests all payloads from each set. This attack type is *useful when multiple positions have different payloads*, and we want to test them all together.

#### Sniper
The Sniper attack type is the default and most commonly used attack type in Burp Suite Intruder. It is particularly *effective for single-position attacks*, such as password brute-force or fuzzing for API endpoints. In a Sniper attack, we provide a set of payloads, which can be a wordlist or a range of numbers, and Intruder inserts each payload into each defined position in the request.

Let's refer to the example template from below:

```HTTP
POST /support/login/ HTTP/1.1
Host: 10.10.93.36
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 37
Origin: http://10.10.93.36
Connection: close
Referer: http://10.10.93.36/support/login/
Upgrade-Insecure-Requests: 1

username=§pentester§&password=§Expl01ted§
```

In this example, we have two positions defined for the `username` and `password` body parameters. In a Sniper attack, Intruder takes each payload from the payload set and substitutes it into each defined position in turn.

Assuming we have a wordlist with three words: `burp`, `suite`, and `intruder`, Intruder would generate six requests:

	username=burp&password=Expl01ted
	username=suite&password=Expl01ted
	username=intruder&password=Expl01ted
	username=pentester&password=burp
	username=pentester&password=suite
	username=pentester&password=intruder

Observe how Intruder starts with the first position (`username`) and substitutes each payload into it, then moves to the second position (`password`) and performs the same substitution with the payloads. The total number of requests made by Intruder Sniper can be calculated as `requests = numberOfWords * numberOfPositions`

#### Battering Ram
The Battering ram attack type in Burp Suite Intruder differs from Sniper in that *it places the same payload in every position* simultaneously, rather than substituting each payload into each position in turn.

From our previous example, using the Battering Ram attack type with the same wordlist from before (`burp`, `suite`, and `intruder`), Intruder would generate three requests:

	username=burp&password=burp
	username=suite&password=suite
	username=intruder&password=intruder

The Battering Ram attack type is useful when *we want to test the same payload against multiple positions* at once without the need for sequential substitution.

#### Pitchfork
The Pitchfork attack type in Burp Suite Intruder is *similar to having multiple Sniper attacks running simultaneously*. While Sniper uses one payload set to test all positions simultaneously, Pitchfork utilises one payload set per position (up to a maximum of 20) and iterates through them all simultaneously.

To better understand Pitchfork, let us revisit our example, but this time with two wordlists:

- The first wordlist contains usernames: `joel`, `harriet`, and `alex`.
- The second wordlist contains passwords: `J03l`, `Emma1815`, and `Sk1ll`.

We can use these two lists to perform a Pitchfork attack on the login form. Each request made during the attack would look like this:

	username=joel&password=J03l
	username=harriet&password=Emma1815
	username=alex&password=Sk1ll

It's important to note that Intruder stops testing as soon as one of the lists is complete.
Therefore, in Pitchfork attacks, it is ideal for the payload sets to have the same length.

The Pitchfork attack type is especially useful when *conducting credential-stuffing attacks* or when multiple positions require separate payload sets. It allows for simultaneous testing of multiple positions with different payloads.

#### Cluster Bomb
The Cluster bomb attack type in Burp Suite Intruder allows us to choose multiple payload sets, one per position (up to a maximum of 20). Unlike Pitchfork, where all payload sets are tested simultaneously, Cluster bomb iterates through each payload set individually, *ensuring that every possible combination* of payloads is tested.

To illustrate the Cluster bomb attack type, let's use the same wordlists as before:

- Usernames: `joel`, `harriet`, and `alex`.
- Passwords: `J03l`, `Emma1815`, and `Sk1ll`.

We can use a Cluster bomb attack to try every combination of values. The request table for our username and password positions would look like this:

	username=joel&password=J03l
	username=harriet&password=J03l
	username=alex&password=J03l
	username=joel&password=Emma1815
	username=harriet&password=Emma1815
	username=alex&password=Emma1815
	username=joel&password=Sk1ll
	username=harriet&password=Sk1ll
	username=alex&password=Sk1ll

Cluster bomb attacks *can generate a significant amount of traffic* as it tests every combination. The number of requests made by a Cluster bomb attack can be calculated by multiplying the number of lines in each payload set together. 