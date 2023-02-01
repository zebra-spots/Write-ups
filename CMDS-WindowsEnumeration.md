# Windows Enumeration

```getuid```

Get user information

```getpid```

Get process id

```ps```

List processes - look at processes that are running on the box

```sysinfo```

Get system information, architecture etc systeminfo in shell

```net time \\<hostname>```

Get time on host – downgrade to shell first

```netstat -ano```

Look at active connections, network interfaces and open connections

```ipconfig /all```

Learn about the network and nic's (maybe drop to shell to get more info)

```arp -a```

View the arp table to learn about other hosts the machine knows about

```net users```

Local user list net user /domain for domain

```net users <username>```

Find more info about users

```net accounts /domain```

Find more info about domain users

```net localgroup```

Find out about local groups

```net group /domain```

Find out about domain groups 
net group “groupname” /domain
net group “Domain Computers” /domain

```net view```

See computers that the box can see 
net view \\COMPUTERNAME /all 

```net view /domain```

Find out about domain

```ping <hostname> -4 -n 1```

IP4 ping boxes found above to gather IP addresses with 1 ping request

```route print```

Find network info

```ipconfig /displaydns```

Get DNS info if present

```type C:\windows\system32\drivers\etc\hosts```

Look at hosts file to see any local static DNS records

```net share```

Local at local shares

```net session```

Look for sessions

## Look for areas for Priv Esc

 
```schtasks```

Look for scheduled tasks

```schtasks /query | findstr “Running”```

View running scheduled tasks

```schtasks /query | findstr “Ready”```

View ready scheduled tasks

```schtasks /query /TN \Microsoft\WIndows\Wininet\cachetask /v /fo list```

Query a specific scheduled task verbose output formatted as a list

```sc query```

View all services

```sc qc <servicename>```

Query a particular service

```wmic service get name,pathname,displayname,startmode | findstr /i auto | findstr /i /v "C:\Windows\\" | findstr /i /v """
```
 

Look for unquoted service paths

```Icacls C:\Windows\system32\sppsvc.exe```

## View permissions of files to see if you can replace a binary 

```path > C:\users\imluser\documents\path.txt```

Finds environment variable paths and output to a readable .txt file
