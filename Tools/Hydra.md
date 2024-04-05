
Hydra allows us to automated way to try the common passwords or the entries from a word list. It also supports many protocols, including **FTP**, **POP3**, **IMAP**, **SMTP**, SSH, and all methods related to **HTTP**. 

The general command-line syntax is: `hydra -l username -P wordlist.txt server service `where we specify the following options:

- `-l username`: `-l `should precede the `username`, i.e. the login name of the target.
- `-P wordlist.txt`: `-P` precedes the `wordlist.txt` file, which is a *text file containing the list of passwords* you want to try with the provided username.
- `server` is the *hostname or IP address* of the target server.
- `service` indicates the service which you are *trying to* launch the dictionary *attack*.

There are some extra optional arguments that we can add:

- `-s PORT` to *specify a non-default port* for the service in question.
- `-V` or `-vV`, for verbose, makes Hydra *show the username and password combinations* that are being *tried*. 
- `-t n` where n is the *number of parallel connections* to the target. `-t 16` will create 16 threads used to connect to the target.
- `-d`, for debugging, to *get more detailed information* about what’s going on. The debugging output can save you much frustration; for instance, if Hydra tries to connect to a closed port and timing out, `-d `will reveal this right away.