# Hacking with Powrshell:

Common verbs to use include:
 - Get
 - Start
 - Stop
 - Read
 - Write
 - New
 - Out


The Get-Help with -Example gives the help page for the command that we are looing
at.

 ```powershell
 PS C:\Users\Administrator> Get-Help Get-Command -Examples

NAME
    Get-Command

SYNOPSIS
Gets all commands.

Example 1: Get cmdlets, functions, and aliases
```

Get-Member shows the methods of the functions
```powershell
Get-Command | Get-Member -MemberType Method
```

 The Get-ChildItem shows the dircetory and Select-Object can manipulate the 
 properties of the item.

```powershell
 Get-ChildItem | Select-Object -Property Mode, Name
 ```

## Filtering Objects:

When retrieving output objects, you may want to select objects that match a very
specific value. You can do this using the Where-Object to filter based on the value 
of properties.

The general format for using this cmdlet is

Verb-Noun | Where-Object -Property PropertyName -operator Value

Verb-Noun | Where-Object {$_.PropertyName -operator Value}

The second version uses the $_ operator to iterate through every object passed to the Where-Object cmdlet.

Powershell is quite sensitive, so don't put quotes around the command!

Where -operator is a list of the following operators:

 -Contains: if any item in the property value is an exact match for the specified value
 -EQ: if the property value is the same as the specified value
 -GT: if the property value is greater than the specified value

```powershell
Get-Service | Where-Object -Property Status -eq Stopped
```

## Sort Objects:

```powershell
Get-ChildItem | Sort-Object
```

## Section 3

What is the location of the file "interesting-file.txt"?
```powershell
Get-ChildItem -r -Include *interst* -File -Recurse -ErrorAction SilentlyContinue
```


Specify the contents of this file?
```powershell
Get-Content -Path 'C:\Program Files\interesting-file.txt.txt'
```


How many cmdlets are installed on the system(only cmdlets, not functions and aliases)?
```powershell
Get-Command | Where-Object -Property CommandType -eq Cmdlet | measure
```


Get the MD5 hash of interesting-file.txt
```powershell
Get-FileHash 'C:\Program Files\interesting-file.txt.txt' -Algorithm MD5
```
What is the command to get the current working directory?
```powershell
Get-Location
```

Does the path "C:\Users\Administrator\Documents\Passwords" Exist (Y/N)?
```powershell 
Get-Location "C:\Users\Administrator\Documents\Passwords"
```

What command would you use to make a request to a web server?
```powershell
Invoke-WebRequest -URI https://www.bing.com/search?q=how+many+feet+in+a+mile
```

Base64 decode the file b64.txt on Windows.
```powershell
$data = Get-Content 'b64.txt'
[System.Text.Encoding]::ASCII.GetString([System.Convert]::FromBase64String($data))
```

## Section 4:


How many users are there on the machine?
```powershell
Get-LocalUser
```

Which local user does this SID(S-1-5-21-1394777289-3961777894-1791813945-501) belong to?
```powershell
Get-LocalUser -SID "S-1-5-21-1394777289-3961777894-1791813945-501"
```

How many users have their password required values set to False?
```powershell 
Get-LocalUser | Where-Object -Property PasswordRequired -Match false
```

How many local groups exist?
```powershell
Get-LocalGroup | measure
```

What command did you use to get the IP address info?
```powershell
Get-NetIPAddress
```

How many ports are listed as listening?
```powershell
Get-NetTCPconnection -State Listen | measure
```

What is the remote address of the local port listening on port 445?
```powershell
Get-NetTCPconnection -State Listen -LocalPort 445
```

How many patches have been applied?
```powershell
Get-HotFix | measure
```

When was the patch with ID KB4023834 installed?
```powershell
Get-HotFix -Id KB4023834
```


Find the contents of a backup file.
```powershell
Get-ChildItem -r -Include *.bak* -File 
```

Search for all files containing API_KEY
```powershell
Git-ChildItem -r | Select-String -pattern API_KEY
```

What command do you do to list all the running processes?
```powershell
Get-Process
```

What is the path of the scheduled task called new-sched-task?
```powershell
Get-Scheduledtask -TaskName new-sched-task
```

GET-ACL is the answer you need.
```powershell
Get-Acl C:\
```

# Section 5:

For defining the variable we can use `$variable_name = value`. For interation 
we can use `foreach($new_var in $existing_var){}`.

```powershell
$system_ports = Get-NetTCPConnection -State Listen

$text_port = Get-Content -Path C:\Users\Administrator\Desktop\ports.txt

foreach($port in $text_port){

    if($port -in $system_ports.LocalPort){
        echo $port
     }

}
```

What file contains the password?
```powershell
$path = 'C:\Users\Administrator\Desktop\emails\*'
$magic_word = 'password'
$exec = Get-ChildItem $path -recurse | Select-String -pattern $magic_word
echo $exec
```

What files contains an HTTPS link?
```powershell
$path = 'C:\Users\Administrator\Desktop\emails\*'
$magic_word = 'https://'
$exec = Get-ChildItem $path -recurse | Select-String -pattern $magic_word
echo $exec
```

# Section 6:

Open ports on localhost between 130-140?
```powershell
foreach ($port in 1..1024) {
  $result = Test-NetConnection -ComputerName "www.example.com" -Port $port -WarningAction SilentlyContinue
  if ($result.TcpTestSucceeded -or $result.PingSucceeded) {
      Write-Output "TCP port $port is open!"
  }
}
```
