
BloodHound is a tool that we can use to graphically map the domain and all relevant information. Before running BloodHound, we need to first run `neo4j`.

```bash
$ sudo neo4j console  
Directories in use:
home:         /usr/share/neo4j
config:       /usr/share/neo4j/conf
logs:         /etc/neo4j/logs
plugins:      /usr/share/neo4j/plugins
import:       /usr/share/neo4j/import
data:         /etc/neo4j/data
certificates: /usr/share/neo4j/certificates
licenses:     /usr/share/neo4j/licenses
run:          /var/lib/neo4j/run
Starting Neo4j.
2025-03-11 00:04:34.452+0000 INFO  Starting...
2025-03-11 00:04:34.856+0000 INFO  This instance is ServerId{58ec4515} (58ec4515-4b9f-4b95-aab6-9d67fc3104af)
2025-03-11 00:04:35.737+0000 INFO  ======== Neo4j 4.4.26 ========
2025-03-11 00:04:37.092+0000 INFO  Performing postInitialization step for component 'security-users' with version 3 and status CURRENT
2025-03-11 00:04:37.092+0000 INFO  Updating the initial password in component 'security-users'
2025-03-11 00:04:37.884+0000 INFO  Bolt enabled on localhost:7687.
2025-03-11 00:04:38.510+0000 INFO  Remote interface available at http://localhost:7474/
2025-03-11 00:04:38.515+0000 INFO  id: 50DDD2021CEA8A1B882BB6A33F3AD9BB571F44C37767AABA065F28FD1CDC1F05
2025-03-11 00:04:38.515+0000 INFO  name: system
2025-03-11 00:04:38.516+0000 INFO  creationDate: 2025-03-08T10:34:41.821Z
2025-03-11 00:04:38.516+0000 INFO  Started.
```

This provides us with a remote interface at `http://localhost:7474/`. When we log in for the first time, we will be asked to create a new password. Once this is set, we can now run BloodHound from another terminal.

```bash
$ sudo bloodhound
```

Use the same credentials as `neo4j` and login to BloodHound.

![[BloodHound Login.png]]

When logging for the first time, there will be no data in the database. We need to use an ingestor use to populate the database as shown below.

```bash
$ sudo bloodhound-python -d MARVEL.local -u fcastle -p Password1 -ns 192.168.23.130 -c all

INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)
INFO: Found AD domain: marvel.local
INFO: Getting TGT for user
WARNING: Failed to get Kerberos TGT. Falling back to NTLM authentication. Error: [Errno Connection error (hydra-dc.marvel.local:88)] [Errno -2] Name or service not known
INFO: Connecting to LDAP server: hydra-dc.marvel.local
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 3 computers
INFO: Connecting to LDAP server: hydra-dc.marvel.local
INFO: Found 9 users
INFO: Found 52 groups
INFO: Found 3 gpos
INFO: Found 2 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: SPIDERMAN.MARVEL.local
INFO: Querying computer: THEPUNISHER.MARVEL.local
INFO: Querying computer: HYDRA-DC.MARVEL.local
INFO: Done in 00M 01S

$ ls
20250311111342_computers.json   20250311111342_domains.json  20250311111342_groups.json  20250311111342_users.json
20250311111342_containers.json  20250311111342_gpos.json     20250311111342_ous.json
```

`-d`: The domain
`-u`: Username
`-p`: Password
`-ns`: Name Server (In this case it is our Domain Controller)
`-c`: What data to be collected

Once the `.json` files are created, we need to import them to BloodHound using *Upload Data* option in the interface. Once the upload is completed, we can check the database info to verify the details.

Now we can use the available **Analysis** options to perform different types of analysis on the domain. For example, we can find the *shortest paths to domain admins* as below.

![[BloodHound Shortest Paths.png]]

We can click on a node to gain further information about it as well. Below we see that there are **9 Reachable High Value Targets** from the *ADMINISTRATOR* user in the MARVEL domain. 

![[BloodHound Node Info.png]]

Furthermore, we can right click on nodes and mark them as **Owned** or **High Value** and use them to see which other accounts we can compromise based on the level of access we have.

![[Node Properties.png]]