![image](https://github.com/zebra-spots/Write-ups/assets/88425510/e61e1008-3f9d-4adc-9915-babdf7dd0320)## Topology:

![image](https://github.com/zebra-spots/Write-ups/assets/88425510/ac6f6f8b-0e65-4855-aa6a-411e4def14b6)

# Switch configs

#############
SW1
#############
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
To test ssh
put pc onto same subnet and open terminal
ssh -l admin 192.168.1.5
~~~~~~


#############
SW2
#############
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
To test ssh
put pc onto same subnet and open terminal
ssh -l admin 192.168.2.5
~~~~~~
