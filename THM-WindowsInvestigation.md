In this room we connect via RDP to a windows server using the administrator account. This has been compromised and we answer questions relating to the attack. 

What's the version and year of the windows machine?

This can be found in the GUI but I like to use the command line where I can.

![image](https://user-images.githubusercontent.com/88425510/215875882-793fcaa4-5dbb-4153-b4c8-0720279feacb.png)

Which user logged in last?

Administrator (my login)

![image](https://user-images.githubusercontent.com/88425510/215876056-de61330a-89a7-4072-8136-12c864624b1b.png)

When did John log onto the system last?

![image](https://user-images.githubusercontent.com/88425510/215876165-bd24ff09-d836-49fa-93b0-a8caa3ef1b95.png)

What IP does the system connect to when it first starts?

Found after googling that this can be found out in the registry editor. I also discovered if you are quick enough you can see it in the boot screen.

![image](https://user-images.githubusercontent.com/88425510/215876280-c0e86957-4c9c-4a16-b5f5-cc45d681f4b1.png)

What two accounts had administrative privileges (other than the Administrator user)?

Using powershell:
![image](https://user-images.githubusercontent.com/88425510/215876341-09eab95d-1c9f-4b90-b114-95e4503beccb.png)

Using cmd:
![image](https://user-images.githubusercontent.com/88425510/215876380-17c2992f-a9c9-4332-aa0f-6647b3ec7638.png)

What's the name of the scheduled task that is malicious.

We can see that this scheduled task is set to create a powershell netcat listener on port 1348.

![image](https://user-images.githubusercontent.com/88425510/215876427-e407ffbf-f193-4951-a8ca-5c5ef29795ed.png)

What file was the task trying to run daily?

Nc.ps1

What port did this file listen locally for?

1348

When did Jenny last logon?

![image](https://user-images.githubusercontent.com/88425510/215876496-184554e4-df75-4b6f-bdf8-a5abc6361df9.png)

At what date did the compromise take place?

Navigate to the file location and look when the file was created in properties.

![image](https://user-images.githubusercontent.com/88425510/215876564-2220a5d2-ec1d-4985-9b1a-54cd81fa3787.png)

![image](https://user-images.githubusercontent.com/88425510/215876573-763edd9b-078d-476e-b5b6-740b58fb22f9.png)

At what time did Windows first assign special privileges to a new logon?

Events can be filtered for Event ID 4672
https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4672

![image](https://user-images.githubusercontent.com/88425510/215876628-c0e9cfc0-ec75-4b37-a48a-8eb1150a22ea.png)

What tool was used to get Windows passwords?

Mimikatz left in c:\temp along with output file

![image](https://user-images.githubusercontent.com/88425510/215876686-7c82cd72-653a-4938-ae98-e0e7358ec918.png)

What was the attackers external control and command servers IP?

I initially looked at netstat to see if there was an active connection on the listening port but there was nothing there.
This is found in the hosts file. There has been a DNS poisoning attack and the C2 server is there in place of a legitimate google address.

![image](https://user-images.githubusercontent.com/88425510/215876754-414dc47f-4de5-4140-93dd-25686b9ab916.png)

What was the extension name of the shell uploaded via the servers website?

If inetpub is present in the c: drive then it is an iis server. wwwroot is a location where files can be uploaded. 
I would also expect to see it in server manager and iis services running, these were not present on this lab.

![image](https://user-images.githubusercontent.com/88425510/215876809-97cef60a-d74b-4636-b14b-4d7a295afece.png)

What was the last port the attacker opened?

Look in windows firewall for ports that shouldn’t usually be open. 

![image](https://user-images.githubusercontent.com/88425510/215876864-b7ed41e4-32b2-402e-8420-61a2ab780237.png)

Check for DNS poisoning, what site was targeted?

![image](https://user-images.githubusercontent.com/88425510/215876908-0fa7116f-0907-41f7-85f4-c0fe9054effe.png)




In this lab I practiced getting information about the system and the users on the system using command line and powershell commands. I learned to use regedit to view tasks that will run on start-up, this is useful because I know that some persistence payloads can be placed in the registry (I have done this before when working with metasploit). 
I also saw places where an attacker will be able to place files in folders that are often writable (C:\tmp and wwwroot).
I learned about searching for event id's granting special user permissions, although I would like to look more into this because I found it to be quite a long process.
I saw how you can use the hosts file to conduct a DNS poisoning attack, the attacker put their command and control IP address to a commonly browsed url in google.
