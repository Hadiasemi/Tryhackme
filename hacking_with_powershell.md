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

```
Get-ChildItem | Sort-Object
```

```
Get-ChildItem -r -Include *interst*
```

