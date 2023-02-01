# Linux Enumeration

```
cat /etc/issue
```

Distribution

```
uname -r
```

Get name and information about current kernel

```
cat /etc/*-release
```

kernel version

```
lscpu
```

Check architecture

```
apt-check -p- --human-readable
```

Check patch status

```
yum updateinfo list cves
```

Alternative patch status check

```
cat /etc/passwd
```

Get user list

```
getent passwd | grep admin
```

Search for users (pipe specific user is optional)

```
netstat -a
```

Check network connections

```
netstat -I
```

View network interfaces

```
netstat -r
```

Display IP routing table

```
ip a
```

Get IP info

```
/sbin/route
```

View routing tables

```
ps -aux
```

Display services (+) = running (-) = stopped (?) = unknown	service --status-all
Display processes (-a = all processes, -u = process owner, -x = show processes not attached to a terminal)

```find / -perm -u=s -type f 2>/dev/null```

Print a list of executables with the SUID bit set.

SUID is a feature that allows a file to be executed with the permissions of a specified user. 
If a program requires root permission to run and does have the SUID bit set, that could be valuable to privilege escalation.


```
grep -r -i password .
```

check for plaintext usernames and passwords (-r = recursiovely -I = ignore case) full stop indicates current directory, this can be replaced with any specified directory


```
HISTFILE
```

Stop linux command history being recorded	unset

