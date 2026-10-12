SW3#sh run
Building configuration...

Current configuration : 1494 bytes
!
! Last configuration change at 20:02:55 UTC Sat Sep 12 2026
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname SW3
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
!
!
!
!
!
ip cef
no ipv6 cef
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
spanning-tree vlan 10,20 priority 24576
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
interface Ethernet0/0
 description -=SW1;e0/0=-
 switchport trunk allowed vlan 10,20,30,99
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport nonegotiate
!
interface Ethernet0/1
 description -=SW4;e0/1=-
 switchport trunk allowed vlan 10,20,30,99
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport nonegotiate
!
interface Ethernet0/2
 description -=PC-1;access=-
 switchport access vlan 10
 switchport mode access
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security mac-address sticky 0050.7966.681f
 switchport port-security
 spanning-tree portfast edge
 spanning-tree bpduguard enable
 spanning-tree guard root
!
interface Ethernet0/3
!
ip forward-protocol nd
!
ip http server
ip http secure-server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
!
!
!
!
control-plane
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
!
!
!
end
