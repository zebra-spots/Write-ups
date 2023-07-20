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




