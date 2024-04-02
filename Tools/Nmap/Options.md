
### Address Ranges

- `10.11.12.15-20` will scan 6 IP addresses, from `10.11.12.15` to `10.11.12.20`
- `10.11.12.15/30` will scan 4 IP addresses, from `10.11.12.12` to `10.11.12.15`

### Target Specification

`-iL <inputfilename>`: Input from list of hosts/networks
`-iR <num hosts>`: Choose random targets
`--exclude <host1[,host2][,host3],...>`: Exclude hosts/networks
`--excludefile <exclude_file>`: Exclude list from file

### HOST DISCOVERY

`-sL`: List Scan - simply list targets to scan
`-sn`: Ping Scan - disable port scan
`-Pn`: Treat all hosts as online -- skip host discovery
`-PS/PA/PU/PY[portlist]`: TCP SYN/ACK, UDP or SCTP discovery to given ports
`-PE/PP/PM`: ICMP echo, timestamp, and netmask request discovery probes
`-PO[protocol list]`: IP Protocol Ping
`-n/-R`: Never do DNS resolution/Always resolve [default: sometimes]
`--dns-servers <serv1[,serv2],...>`: Specify custom DNS servers
`--system-dns`: Use OS's DNS resolver
`--traceroute`: Trace hop path to each host

### SCAN TECHNIQUES

`-sS/sT/sA/sW/sM`: TCP SYN/Connect()/ACK/Window/Maimon scans
`-sU`: UDP Scan
`-sN/sF/sX`: TCP Null, FIN, and Xmas scans
`--scanflags <flags>`: Customize TCP scan flags
`-sI <zombie host[:probeport]>`: Idle scan
`-sY/sZ`: SCTP INIT/COOKIE-ECHO scans
`-sO`: IP protocol scan
`-b <FTP relay host>`: FTP bounce scan

