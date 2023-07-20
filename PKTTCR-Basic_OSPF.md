## Design:

![image](https://github.com/zebra-spots/Write-ups/assets/88425510/0e65a132-21ec-46ee-9c7d-4ec4a942c3f5)


## Topology:

![image](https://github.com/zebra-spots/Write-ups/assets/88425510/ac6f6f8b-0e65-4855-aa6a-411e4def14b6)

# Switch configs

SW1
~~~~~~
enable
configure terminal
!
hostname SW1
ip domain-name praclab.com
!
enable secret password1
!
line console 0
password password1
logging synchronous
login local
exit
!
crypto key generate rsa
1024
!
username admin secret password1
!
line vty 0 15
transport input ssh
login local
exit
!
interface vlan 1
ip address 192.168.1.5 255.255.255.0
no shutdown
exit
!
service password-encryption
!
exit
!
copy running-config start-up config
~~~~~~

SW2
~~~~~~
enable
configure terminal
!
hostname SW2
ip domain-name praclab.com
!
enable secret password1
!
line console 0
password password1
logging synchronous
login local
exit
!
crypto key generate rsa
1024
!
username admin secret password1
!
line vty 0 15
transport input ssh
login local
exit
!
interface vlan 1
ip address 192.168.2.5 255.255.255.0
no shutdown
exit
!
service password-encryption
!
exit
!
copy running-config start-up config
~~~~~~

~~~~~~
## To test ssh
put pc onto same subnet and open terminal
ssh -l admin 192.168.x.x
~~~~~~

RTR1
~~~~~~
enable
configure terminal
!
interface fa0/0
ip address 192.168.1.1 255.255.255.0
no shut
exit
!
interface serial 0/1/0
ip address 10.0.0.1 255.255.255.252
no shut
exit
!
interface serial 0/1/1
ip address 10.0.0.5 255.255.255.252
no shut
exit
!
hostname R1
ip domain-name praclab.com
!
enable secret password1
!
line console 0 
password password1
login
!
crypto key generate rsa
1024
!
username admin privilege 15 secret password1
!
line vty 0 15
transport input ssh
login local
ip ssh version 2
!
banner motd #
WARNING: Unauthorized access is prohibited!
#
!
end
write
~~~~~~

RTR2
~~~~~~
enable
configure terminal
!
interface fa0/0
ip address 192.168.2.1 255.255.255.0
no shut
exit
!
interface serial 0/1/0
ip address 10.0.0.2 255.255.255.252
no shut
exit
!
interface serial 0/1/1
ip address 10.0.0.10 255.255.255.252
no shut
exit
!
hostname R2
ip domain-name praclab.com
!
enable secret password1
!
line console 0 
password password1
login
!
crypto key generate rsa
1024
!
username admin privilege 15 secret password1
!
line vty 0 15
transport input ssh
login local
ip ssh version 2
!
banner motd #
WARNING: Unauthorized access is prohibited!
#
!
end
write
~~~~~~

RTR3
~~~~~~
enable
configure terminal
!
interface serial 0/1/0
ip address 10.0.0.6 255.255.255.252
no shut
exit
!
interface serial 0/1/1
ip address 10.0.0.9 255.255.255.252
no shut
exit
!
hostname R3
ip domain-name praclab.com
!
enable secret password1
!
line console 0 
password password1
login
!
crypto key generate rsa
1024
!
username admin privilege 15 secret password1
!
line vty 0 15
transport input ssh
login local
ip ssh version 2
!
banner motd #
WARNING: Unauthorized access is prohibited!
#
!
end
write
~~~~~~

Router 1 OSPF config
~~~~~~
enable
configure terminal
router ospf 1
network 10.0.0.0  0.0.0.3  area 0
network 10.0.0.4  0.0.0.3  area 0
network 10.0.0.8  0.0.0.3  area 0
network 192.168.1.0  0.255.255.255  area 0
exit
exit
copy running-config startup-config
~~~~~~

Router 2 OSPF config
~~~~~~
enable
configure terminal
router ospf 1
network 10.0.0.0  0.0.0.3  area 0
network 10.0.0.4  0.0.0.3  area 0
network 10.0.0.8  0.0.0.3  area 0
network 192.168.2.0  0.255.255.255  area 0
exit
exit
copy running-config startup-config
~~~~~~

Router 3 OSPF config
~~~~~~
enable
configure terminal
router ospf 1
network 10.0.0.0  0.0.0.3  area 0
network 10.0.0.4  0.0.0.3  area 0
network 10.0.0.8  0.0.0.3  area 0
exit
exit
copy running-config startup-config
~~~~~~

## Check OSPF Info

![image](https://github.com/zebra-spots/Write-ups/assets/88425510/0900eea8-157a-4ca1-b50a-31e5b1627e55)

## Show IP route

![image](https://github.com/zebra-spots/Write-ups/assets/88425510/5959ed5c-19bc-4804-a845-ae6667289d5e)


## Subnet table

![image](https://github.com/zebra-spots/Write-ups/assets/88425510/3674d186-2391-40c4-86c7-04b9aa2caba8)

## Wildcard masks
~~~~~~
Slash 	Netmask 	Wildcard mask
/32 	255.255.255.255 	0.0.0.0
/31 	255.255.255.254 	0.0.0.1
/30 	255.255.255.252 	0.0.0.3
/29 	255.255.255.248 	0.0.0.7
/28 	255.255.255.240 	0.0.0.15
/27 	255.255.255.224 	0.0.0.31
/26 	255.255.255.192 	0.0.0.63
/25 	255.255.255.128 	0.0.0.127
/24 	255.255.255.0    	0.0.0.255
  
~~~~~~

## Final thoughts and notes

The switches and routers in this simple network only have basic configurations.
The SSH key being only 1024 bits would not be sufficient in a real network.
The main drive of this exercise was to configure OSPF on 3 routers with LANs configured on two of them.
