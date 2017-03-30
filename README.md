# NSIC

## NAT Configuration

http://www.cisco.com/c/en/us/td/docs/ios-xml/ios/ipaddr_nat/configuration/15-mt/nat-15-mt-book/iadnat-addr-consv.html

## VLAN Configuration

```
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
```
## DHCP Configuration

```
ip dhcp excluded-address 10.0.0.1 10.0.0.2
ip dhcp excluded-address 10.0.1.1 10.0.1.2
ip dhcp excluded-address 10.0.2.1 10.0.2.2
!
ip dhcp pool IT
 network 10.0.0.0 255.255.255.240
 default-router 10.0.0.1
 dns-server 10.0.0.1
ip dhcp pool Accounting
 network 10.0.1.0 255.255.255.240
 default-router 10.0.1.1
 dns-server 10.0.1.1
ip dhcp pool Claims
 network 10.0.2.0 255.255.255.240
 default-router 10.0.2.1
 dns-server 10.0.2.1
 ```
 
## Voice Configuration

http://www.cisco.com/c/en/us/support/docs/voice/call-routing-dial-plans/14074-in-dial-peer-match.html
http://www.packettracernetwork.com/tutorials/voipconfiguration.html

 Setup Voice VLAN or Interface on Router
 ```
 interface FastEthernet0/1.6
 description Voice
 encapsulation dot1Q 6
 ip address 10.0.5.1 255.255.255.240
```

Setup DHCP configuration for the Voice VLAN on Router
```
ip dhcp pool Voice
 network 10.0.5.0 255.255.255.240
 default-router 10.0.5.1
 option 150 ip 10.0.5.1
 dns-server 10.0.5.1
!
 ip dhcp excluded-address 10.0.5.1 10.0.5.2
!
 ```
 
 Setup telephony-service on Router
 ```
 telephony-service
  max-ephones 2 #Max number of phones
  max-dn 2  #Max number of dial numbers
  ip source-address 10.0.5.1 port 2000
!
```

Setup extension for each phone
```
ephone-dn 1
 number 0001
!
ephone-dn 2
 number 0002
```

Setup dial-peer to call out to other teams
* If an external team has the extension 1001 then the pattern would be 1... and if they had 2001 then it would be 2... and so on.
* session target is the IP of the other teams router
```
dial-peer voice 1 voip
 destination-pattern 1...
 session target ipv4:192.168.0.3
```

Setup Voice VLAN on the switch
```
interface Vlan6
 description Voice
 mac-address 00e0.a34c.6105
 ip address 10.0.5.2 255.255.255.240
```

Configure phone interface on switch
```
switchport voice vlan 6
```

Plug the phone in and after it comes up go back to the Router to add the following configuration
* The ephones will automatically be created once the phone reaches the router. Just add the type and the button configuration.
```
ephone 1
 device-security-mode none
 mac-address 0060.5C50.4666
 type 7960
 button 1:1
!
ephone 2
 device-security-mode none
 mac-address 0060.2F1A.E44B
 type 7960
 button 1:2
 ```

## DHCP Snooping

Configure DHCP snooping on the switch

```
ip dhcp snooping vlan 1,2,3,4,5,6
```

Then add this command to the interface that the DHCP server is on (the port that connects to the router in this case)
```
ip dhcp snooping trust
```

## Port Security

Configure port security on the switch host ports (set the MAC address of the PCs connected to each port)
```
interface FastEthernet0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 5
 switchport port-security mac-address 0001.96A2.906C
```



## Blocking a Domain

https://www.tunnelsup.com/using-just-a-cisco-asa-to-block-specific-websites/
