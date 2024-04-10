![image](https://github.com/zebra-spots/Write-ups/assets/88425510/27214c6f-565a-41dd-9434-8ecf0ab292cd)

## Router config
~~~~~
en
conf t
hostname Internet
!
int g0/1
ip address 8.8.8.1 255.255.255.0
no shut
!
int g0/0
ip address 10.1.1.2 255.255.255.252
no shut
!
exit
exit
!
wr
!
~~~~~
## Firewall config
~~~~~
en
# Note there is no password, so press enter

conf t
hostname ASA
!
enable password cisco
!
int g1/1
ip address 10.1.1.1 255.255.255.252
nameif outside
security-level 0
no shut
!
int g1/2
ip address 192.168.1.1 255.255.255.0
nameif inside
security-level 100
no shut
!
dhcp address 192.168.1.10-192.168.1.20 inside
dhcp dns 8.8.8.8
dhcp option 3 ip 192.168.1.1
dhcp enable inside
!
route outside 0.0.0.0 0.0.0.0 10.1.1.2
object network INSIDE-NET
subnet 192.168.1.0 255.255.255.0
nat (inside,outside) dynamic interface 
exit
!
~~~~~
