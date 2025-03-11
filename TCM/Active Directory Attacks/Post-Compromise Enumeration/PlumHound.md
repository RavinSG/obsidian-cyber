
This is more like [[BloodHound]] for Blue and Purple teams. First of all we need to change the directory to where PlumHound is installed and BloodHound should be running.

The we can run the following command to test whether PlumHound can access the relevant information.

```bash
$ sudo python3 PlumHound.py --easy -p neo4j1                

        PlumHound 1.6
        For more information: https://github.com/plumhound
        --------------------------------------
        Server: bolt://localhost:7687
        User: neo4j
        Password: *****
        Encryption: False
        Timeout: 300
        --------------------------------------
        Task: Easy
        Query Title: Domain Users
        Query Format: STDOUT
        Query Cypher: MATCH (n:User) RETURN n.name, n.displayname
        --------------------------------------
INFO    Found 1 task(s)
INFO    --------------------------------------

on 1: n.name                      n.displayname
      --------------------------  ---------------
      ADMINISTRATOR@MARVEL.LOCAL
      TSTARK@MARVEL.LOCAL         Tony Stark
      SQLSERVICE@MARVEL.LOCAL     SQL Service
      FCASTLE@MARVEL.LOCAL        Frank Castle
      SQASKUFCVL@MARVEL.LOCAL     SqaSKUfcVl
      PPARKER@MARVEL.LOCAL        Peter Parker
      KRBTGT@MARVEL.LOCAL
      GUEST@MARVEL.LOCAL
      
      NT AUTHORITY@MARVEL.LOCAL

         Executing Tasks |██████████████████████████████████████████████████| Tasks 1 / 1  in 0.0s (859.35/s) 

        Completed 1 of 1 tasks.
```

Once we have tested the connection, we can run a task using the following command. More details on what tasks are available can be found [here](https://github.com/PlumHound/PlumHound)

```bash
$ sudo python3 PlumHound.py -x tasks/default.tasks -p neo4j1

        PlumHound 1.6
        For more information: https://github.com/plumhound
        --------------------------------------
        Server: bolt://localhost:7687
        User: neo4j
        Password: *****
        Encryption: False
        Timeout: 300
        --------------------------------------
        Tasks: Task File
        TaskFile: tasks/default.tasks
        Found 119 task(s)
        --------------------------------------


on 119:         Completed Reports Archive: reports//Reports.zip
         Executing Tasks |██████████████████████████████████████████████████| Tasks 119 / 119  in 3.5s (33.61/s) 

        Completed 119 of 119 tasks.
```

The results of the executed tasks can be found under the `reports` directory. The best starting point is the `index.html` provided there. 

![[PlumHound Report.png]]