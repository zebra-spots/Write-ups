Powerview is a post exploitation framework used for further enumeration of  a system. 
  
Git for PowerView: 
 
https://github.com/PowerShellMafia/PowerSploit/blob/dev/Recon/PowerView.ps1 
 
Cheatsheet: 
 
[PowerView - HackTricks ](https://book.hacktricks.xyz/windows/basic-powershell-for-pentesters/powerview)
  
To use PowerView the ExecutionPolicy must be unrestricted, to view the current execution policy: 

```Get-ExecutionPolicy```

```Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Unrestricted```

Then the PowerView module needs to be imported: 
  
```Import-Module <PATH>\powerview.ps1```


List the networks domain controllers, and information about them: 
  
```Get-NetDomainControllers```
  
  
List the accounts that are on the domain: 
  
```Get-NetUser```
  
  
List the properties given to the accounts which can then be queried: 
  
```Get-UserProperties```
  
  
List specific properties from users: 
  
```GetUserProperties -Properties <PROPERTY>, <PROPERTY> ```
  
  
List computers in the domain: 
  
```Get-NetComputers```
  
  
Find network shares: 
  
```Invoke-ShareFinder```
  
  
  
Find files based on filename: 
  
```Invoke-FileFinder -Terms <TERM>```
  
  
Mount a remote share so can change directory to it: 
  
```Net use \\<SHARE\LOCATION$>\ ```
```Cd \\<SHARE\LOCATION$>\ ```
  
  
Print out a text file: 
Ensure the file isn't too big to open, so you don't crash your session. 
  
```Type  <FILE LOCATION> ```
  
  
Return session information for the machine: 
  
```Get-NetSessions ```
  
  
  
List users/computers currently logged onto the domain: 
  
```Get-NetLoggedOn ```
  
  
List domain computers information and properties: 
  
```Get-NetComputers -FullData ```

