# Pickle Rick

## Questions:

What is the first ingredient Rick needs?

What's the second ingredient Rick needs?

What's the final ingredient Rick needs?


## Recon:

Ping tgt to see it is online

![image](https://user-images.githubusercontent.com/88425510/215357779-80ccca17-2611-497e-b313-2116a00845da.png)


### Nmap scan

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


### Gobuster scan:

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

![image](https://user-images.githubusercontent.com/88425510/215358049-34d068b3-d8a1-4e6c-a3a1-4d6b139f227b.png)

![image](https://user-images.githubusercontent.com/88425510/215358059-248eee40-c2ba-4fa0-9209-ad04a5602134.png)

cat is disabled so we need to read files in a different way

![image](https://user-images.githubusercontent.com/88425510/215358077-caaad498-0826-45aa-bb6c-84a57a0dd372.png)


Test for ways we can upload a reverse shell:

It has python3 installed on the box

![image](https://user-images.githubusercontent.com/88425510/215358115-1ec9c9c9-98e0-4db9-9009-b58e6145c245.png)

Find a python reverse shell on pentestmonkey

![image](https://user-images.githubusercontent.com/88425510/215358135-d6e827f0-d124-4bb8-bc14-654b29a4b45e.png)

Make sure to edit the reverse shell to python3, add your attack box IP and choose the port you will listen on.

![image](https://user-images.githubusercontent.com/88425510/215358180-21d77f76-a301-4e0f-98cc-0c7c098d6364.png)


### Create a netcat listener

![image](https://user-images.githubusercontent.com/88425510/215358192-76c5a7a3-660f-4f76-8c67-6f6f9a28ac56.png)


Paste the python command into the target machine and your shell will pop

![image](https://user-images.githubusercontent.com/88425510/215358204-cb28d28e-6b8b-45f5-892d-148ef6584690.png)

Look around for ingredients 

![image](https://user-images.githubusercontent.com/88425510/215358232-9a0fbca8-aba5-489f-b94b-5ee4b0f4d3ee.png)

## Priv esc:

Checking the sudo list for your user shows that you can run sudo on anything

![image](https://user-images.githubusercontent.com/88425510/215358263-fddaeccf-209e-4503-9241-424f6d6c5f81.png)

Shell stabilisation technique if needed:

![image](https://user-images.githubusercontent.com/88425510/215358289-867dfc71-ed37-47fa-a64b-0f367ef00824.png)

