Questions:

What is the first ingredient Rick needs?

What's the second ingredient Rick needs?

What's the final ingredient Rick needs?


Recon:

Ping tgt to see it is online

![image](https://user-images.githubusercontent.com/88425510/215357779-80ccca17-2611-497e-b313-2116a00845da.png)


Nmap scan

Nmap 10.10.90.69 -Pn -A -p- -T5

-Pn treat host as online
-A enable OS detection
-T5 fast
-oN output

![image](https://user-images.githubusercontent.com/88425510/215357787-ec99ea11-80ad-4198-bd14-cf368a0731d1.png)
![image](https://user-images.githubusercontent.com/88425510/215357796-2469a7ce-c650-4ff9-bc52-629a8980146e.png)

We see a linux machine is at this address and it has ssh and http open. We will investigate the http to see if we can get credentials to use over ssh, or on the web page.

Browse to the webpage and look around:

Note in source code:
![image](https://user-images.githubusercontent.com/88425510/215357804-05bb43fa-1ce9-4eea-be7a-b4d9b2101973.png)

Check robots.txt

![image](https://user-images.githubusercontent.com/88425510/215357832-ff0815d6-64d6-4617-a5d9-cdcc399471dc.png)


Gobuster scan:

![image](https://user-images.githubusercontent.com/88425510/215357879-5a979bc1-7e4c-41d5-99cc-f30c900e1580.png)

Have a look at the links found:

Clue.txt

![image](https://user-images.githubusercontent.com/88425510/215357994-af0d89df-56f3-4f4e-a4fd-8b2fe3a5647c.png)


assets

![image](https://user-images.githubusercontent.com/88425510/215358001-4561e4d7-569a-4881-b65a-ed2b2039a5bc.png)


Login.php

![image](https://user-images.githubusercontent.com/88425510/215358014-c1306bd9-b35b-47e2-bc91-47c214c5525f.png)


Log in to the login page using the previously found username and the password which was in the robots.txt page

Gives a command panel to look at

![image](https://user-images.githubusercontent.com/88425510/215357930-8def44de-849c-443a-9918-14034897547c.png)
