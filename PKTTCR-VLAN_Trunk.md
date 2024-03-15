## Topology:

![image](https://github.com/zebra-spots/Write-ups/assets/88425510/e12f5cfb-bef0-442c-8f58-1ca5a299628e)

Note, PC's are assigned the number in their name as their final octet, apart from PC1 which has .10



# Switch configs



Configuring VLANS:
Switch 1:

Switch>en
Switch#conf t
!
Switch(config)#hostname SW1
SW1(config)#vlan 110
SW1(config-vlan)#exit
SW1(config)#vlan 120
SW1(config-vlan)#exit
!
SW1(config)#interface range fastEthernet0/1-2
SW1(config-if-range)#switchport mode access 
SW1(config-if-range)#switchport access vlan 110
SW1(config-if-range)#exit
!
SW1(config)#interface range fastEthernet0/3-4
SW1(config-if-range)#switchport mode access 
SW1(config-if-range)#switchport access vlan 120
SW1(config-if-range)#exit

Switch 2:

Switch>en
Switch#conf t
!
Switch(config)#hostname SW2
SW1(config)#vlan 110
SW1(config-vlan)#exit
SW1(config)#vlan 120
SW1(config-vlan)#exit
!
SW1(config)#interface range fastEthernet0/1-2
SW1(config-if-range)#switchport mode access 
SW1(config-if-range)#switchport access vlan 110
SW1(config-if-range)#exit
!
SW1(config)#interface range fastEthernet0/3-4
SW1(config-if-range)#switchport mode access 
SW1(config-if-range)#switchport access vlan 120
SW1(config-if-range)#exit



## Check VLAN Info

![image](https://github.com/zebra-spots/Write-ups/assets/88425510/b8d56191-bed8-4f0f-aca6-e6ff52abe133)



Configuring trunk:
Switch 1:

SW1#en
SW1#conf terminal 
SW1(config)#interface fastEthernet 0/10
SW1(config-if)#switchport mode trunk 
SW1(config-if)#switchport trunk allowed vlan 110
SW1(config-if)#switchport trunk allowed vlan 120

Switch 2:

SW2#en
SW2#conf terminal 
SW2(config)#interface fastEthernet 0/10
SW2(config-if)#switchport mode trunk 
SW2(config-if)#switchport trunk allowed vlan 110
SW2(config-if)#switchport trunk allowed vlan 120



## Check Trunk Info

![image](https://github.com/zebra-spots/Write-ups/assets/88425510/3808c511-a56f-411f-8d4d-555565cb8ae3)



## See VLAN and trunk effects via pinging

![image](https://github.com/zebra-spots/Write-ups/assets/88425510/c51f5d80-6224-4420-8cd8-fa2079128df3)


