# NSIC

## NAT Configuration

http://www.cisco.com/c/en/us/td/docs/ios-xml/ios/ipaddr_nat/configuration/15-mt/nat-15-mt-book/iadnat-addr-consv.html

## VLAN Configuration

'''
interface GigabitEthernet0/1
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface GigabitEthernet0/1.1
 description IT
 encapsulation dot1Q 1 native
 ip address 10.0.0.1 255.255.255.240
!
interface GigabitEthernet0/1.2
 description Accounting
 encapsulation dot1Q 2
 ip address 10.0.1.1 255.255.255.240
!
interface GigabitEthernet0/1.3
 description Sales
 encapsulation dot1Q 3
 ip address 10.0.2.1 255.255.255.240
!
interface GigabitEthernet0/1.4
 description Claims
 encapsulation dot1Q 4
 ip address 10.0.3.1 255.255.255.240
!
interface GigabitEthernet0/1.5
 description HR
 encapsulation dot1Q 5
 ip address 10.0.4.1 255.255.255.240
!
'''
