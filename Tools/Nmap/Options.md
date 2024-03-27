
### Address Ranges

- `10.11.12.15-20` will scan 6 IP addresses, from `10.11.12.15` to `10.11.12.20`
- `10.11.12.15/30` will scan 4 IP addresses, from `10.11.12.12` to `10.11.12.15`

### Flags

- `-sL`: List the targets that will be scanned without scanning them
- `-n `: Do not perform reverse-DNS resolution on the targets 
- `-iL`: Read targets from the file specified
