
Modules are small components within the Metasploit framework that are built to perform a specific task, such as exploiting a vulnerability, scanning a target, or performing a brute-force attack.

### Auxiliary

Any supporting module, such as *scanners*, *crawlers* and *fuzzers*, can be found here.

```bash
$ tree -L 1 /usr/share/metasploit-framework/modules/auxiliary 
/usr/share/metasploit-framework/modules/auxiliary
├── admin
├── analyze
├── bnat
├── client
├── cloud
├── crawler
├── docx
├── dos
├── example.py
├── example.rb
├── fileformat
├── fuzzers
├── gather
├── parser
├── pdf
├── scanner
├── server
├── sniffer
├── spoof
├── sqli
├── voip
└── vsploit

21 directories, 2 files
```

### Encoders

Encoders will allow you to encode the exploit and payload in the hope that a *signature-based antivirus solution may miss* them.

Signature-based antivirus and security solutions have a database of known threats. They detect threats by comparing suspicious files to this database and raise an alert if there is a match. Thus encoders can have a limited success rate as antivirus solutions can perform additional checks.

```bash
$ tree -L 1 /usr/share/metasploit-framework/modules/encoders 
/usr/share/metasploit-framework/modules/encoders
├── cmd
├── generic
├── mipsbe
├── mipsle
├── php
├── ppc
├── ruby
├── sparc
├── x64
└── x86

11 directories, 0 files
```

### Evasion

While encoders will encode the payload, they should not be considered a direct *attempt to evade antivirus software*. On the other hand, “evasion” modules will try that, with more or less success.

```bash
$ tree -L 2 /usr/share/metasploit-framework/modules/evasion
/usr/share/metasploit-framework/modules/evasion
└── windows
    ├── applocker_evasion_install_util.rb
    ├── applocker_evasion_msbuild.rb
    ├── applocker_evasion_presentationhost.rb
    ├── applocker_evasion_regasm_regsvcs.rb
    ├── applocker_evasion_workflow_compiler.rb
    ├── process_herpaderping.rb
    ├── syscall_inject.rb
    ├── windows_defender_exe.rb
    └── windows_defender_js_hta.rb

2 directories, 9 files
```

### Exploits

Exploits, neatly organised by target system.

```bash
$ tree -L 1 /usr/share/metasploit-framework/modules/exploits 
/usr/share/metasploit-framework/modules/exploits
├── aix
├── android
├── apple_ios
├── bsd
├── bsdi
├── dialup
├── example.py
├── example.rb
├── example_linux_priv_esc.rb
├── example_webapp.rb
├── firefox
├── freebsd
├── hpux
├── irix
├── linux
├── mainframe
├── multi
├── netware
├── openbsd
├── osx
├── qnx
├── solaris
├── unix
└── windows

21 directories, 4 files
```

### NOPs

NOPs (No OPeration) do nothing, literally. They are represented in the Intel *x86 CPU family* with 0x90, following which the *CPU will do nothing for one cycle*. They are often used as a **buffer to achieve consistent payload sizes**.

```bash
$ tree -L 1 /usr/share/metasploit-framework/modules/nops    
/usr/share/metasploit-framework/modules/nops
├── aarch64
├── armle
├── cmd
├── mipsbe
├── php
├── ppc
├── sparc
├── tty
├── x64
└── x86

11 directories, 0 files
```

### Payloads

Payloads are *codes that will run on the target system*.

Exploits will leverage a vulnerability on the target system, but to achieve the desired result, we will need a payload. Examples could be; getting a shell, loading a malware or backdoor to the target system, running a command, or launching calc.exe as a proof of concept to add to the penetration test report. 

Running command on the target system is already an important step but *having an interactive connection* that allows you to type commands that will be executed on the target system is better. Such an interactive command line is called a "**shell**". Metasploit offers the ability to send different payloads that can *open shells on the target system*.

```bash
$ tree -L 1 /usr/share/metasploit-framework/modules/payloads 
/usr/share/metasploit-framework/modules/payloads
├── adapters
├── singles
├── stagers
└── stages

5 directories, 0 files
```

We see four different directories under payloads: adapters, singles, stagers and stages.

- **Adapters**: An adapter wraps single payloads to *convert them into different formats*. For example, a normal single payload can be wrapped inside a Powershell adapter, which will make a single powershell command that will execute the payload.
  
- **Singles**: *Self-contained payloads* (add user, launch notepad.exe, etc.) that do not need to download an additional component to run.
  
- **Stagers**: Responsible for *setting up a connection channel* between Metasploit and the target system. Useful when working with staged payloads. “Staged payloads” will *first upload a stager* on the target system *then download the rest* of the payload (stage). This provides some advantages as the initial size of the payload will be relatively small compared to the full payload sent at once.
  
- **Stages**: Downloaded by the stager. This will allow you to *use larger sized payloads*.

Metasploit has a subtle way to help you identify single (also called “inline”) payloads and staged payloads. The both modules below are reverse Windows shells. 

- `generic/shell_reverse_tcp`
- `windows/x64/shell/reverse_tcp`

The former is an *inline* (or single) payload, as indicated by the “**\_**” between “shell” and “reverse”. 
While the latter is a *staged* payload, as indicated by the “**/**” between “shell” and “reverse”.

### Post

Post modules will be useful on the final stage of the penetration testing process listed above, *post-exploitation*.

```bash
$ tree -L 1 /usr/share/metasploit-framework/modules/post                 
/usr/share/metasploit-framework/modules/post
├── aix
├── android
├── apple_ios
├── bsd
├── firefox
├── hardware
├── linux
├── multi
├── networking
├── osx
├── solaris
└── windows

13 directories, 0 files
```

