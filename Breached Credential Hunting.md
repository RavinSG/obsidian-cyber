- [[Passive Reconnaissance]]

Breached credentials can be found in the dark web. Here is an example [magnet link](magnet:?xt=urn:btih:7ffbcd8cee06aba2ce6561688cf68ce2addca0a3&dn=BreachCompilation&tr=udp%3A%2F%2Ftracker.openbittorrent.com%3A80&tr=udp%3A%2F%2Ftracker.leechers-paradise.org%3A6969&tr=udp%3A%2F%2Ftracker.coppersurfer.tk%3A6969&tr=udp%3A%2F%2Fglotorrents.pw%3A6969&tr=udp%3A%2F%2Ftracker.opentrackr.org%3A1337) for such leaked credentials. A parser such as the [breach-parse](https://github.com/hmaverickadams/breach-parse) can be used to parse and find email & password combinations within the list.

Repeating offenders with similar passwords can be a good starting point to deploy a credential stuffing attack since they have a high probability to use the same password again.

### Hunting breached credentials

Websites such as [dehashed.com](https://dehashed.com) can be used to search for leaked passwords across multiple leak sources. This search ca be combined with attributes such as name, email, address, VIN, etc. to build a profile for a user.

Once a password hash is found, a hash database like [hashmob.net](https://hashmob.net/), [crackstation](https://crackstation.net), or [hashes.com](https://hashes.com/en/decrypt/hash) can be used to reverse the hash. 