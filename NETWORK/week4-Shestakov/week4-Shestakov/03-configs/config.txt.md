
# sw-core.txt

sw-core#sh run
Building configuration...

Current configuration : 2165 bytes
!
! Last configuration change at 20:43:30 UTC Tue Sep 29 2026
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname sw-core
!
boot-start-marker
boot-end-marker
!
!
!
username itcore privilege 15 secret 5 $1$k2ov$r7Sneg.Iyphgxt0m5r4jk.
no aaa new-model
!
!
!
!
!
!
ip multicast-routing
!
!
no ip domain-lookup
ip domain-name skills.local
ip cef
no ipv6 cef
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
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
interface Loopback0
 description RP address
 ip address 10.255.0.1 255.255.255.255
 ip pim sparse-mode
!
interface Ethernet0/0
 description -=sw-1-as;Ethernet0/0=-
 switchport trunk allowed vlan 10,30,99
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 999
 switchport mode trunk
 switchport nonegotiate
!
interface Ethernet0/1
 description -=sw-2-as;Ethernet0/0=-
 switchport trunk allowed vlan 20,99
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 999
 switchport mode trunk
 switchport nonegotiate
!
interface Ethernet0/2
 description -=r-edge;Ethernet0/0=-
 no switchport
 ip address 10.150.111.1 255.255.255.252
!
interface Ethernet0/3
!
interface Vlan10
 description gateway USERS
 ip address 10.150.104.1 255.255.255.0
 ip pim sparse-mode
!
interface Vlan20
 description gateway SERVERS
 ip address 10.150.105.1 255.255.255.0
 ip pim sparse-mode
!
interface Vlan30
 description gateway DEV
 ip address 10.150.106.1 255.255.255.0
 ip pim sparse-mode
!
interface Vlan99
 description gateway MGMT
 ip address 10.150.110.1 255.255.255.0
!
ip forward-protocol nd
!
ip pim rp-address 10.255.0.1
ip pim ssm default
ip http server
ip http secure-server
!
ip route 0.0.0.0 0.0.0.0 10.150.111.2
ip route 10.150.108.0 255.255.255.0 10.150.111.2
ip route 10.150.111.0 255.255.255.252 10.150.111.2
ip ssh version 2
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
 login local
 transport input ssh
!
!
!
end


# r-edge.txt

r-edge#sh run
Building configuration...

Current configuration : 2946 bytes
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname r-edge
!
boot-start-marker
boot-end-marker
!
aqm-register-fnf
!
vrf definition INTERNET
 !
 address-family ipv4
 exit-address-family
!
!
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
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
no ip domain lookup
ip domain name skills.local
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
!
username itcore privilege 15 secret 5 $1$cRUR$5iFC.7YbfmU4OXArU/sR9/
!
redundancy
!
!
ip ssh version 2
!
crypto keyring KR-BRANCH vrf INTERNET
  pre-shared-key address 198.51.100.1 key P@ssw0rd
!
!
!
!
crypto isakmp policy 10
 encr aes 256
 hash sha256
 authentication pre-share
 group 14
crypto isakmp profile ISA-BRANCH
   keyring KR-BRANCH
   match identity address 198.51.100.1 255.255.255.255 INTERNET
!
!
crypto ipsec transform-set TS-GRE esp-aes 256 esp-sha256-hmac
 mode transport
!
crypto ipsec profile IPSEC-PROF
 set transform-set TS-GRE
 set isakmp-profile ISA-BRANCH
!
!
!
!
!
!
!
interface Tunnel0
 description GRE to r-branch
 ip address 10.150.112.1 255.255.255.252
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source Ethernet0/1
 tunnel destination 198.51.100.1
 tunnel vrf INTERNET
 tunnel protection ipsec profile IPSEC-PROF
!
interface Ethernet0/0
 description -=fw;WAN(outside)=-
 ip address 10.150.111.6 255.255.255.252
!
interface Ethernet0/1
 description -=isp;Ethernet0/0=-
 vrf forwarding INTERNET
 ip address 203.0.113.1 255.255.255.252
!
interface Ethernet0/2
 no ip address
 shutdown
!
interface Ethernet0/3
 no ip address
 shutdown
!
router ospf 1
 router-id 10.150.112.1
 redistribute static subnets route-map RM-OFFICE
 network 10.150.112.0 0.0.0.3 area 0
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 10.150.104.0 255.255.255.0 10.150.111.5
ip route 10.150.105.0 255.255.255.0 10.150.111.5
ip route 10.150.106.0 255.255.255.0 10.150.111.5
ip route 10.150.110.0 255.255.255.0 10.150.111.5
ip route 10.150.111.0 255.255.255.252 10.150.111.5
ip route 192.0.2.100 255.255.255.255 Ethernet0/1 203.0.113.2
ip route vrf INTERNET 0.0.0.0 0.0.0.0 203.0.113.2
ip route vrf INTERNET 10.150.104.0 255.255.255.0 10.150.111.5 global
ip route vrf INTERNET 10.150.105.0 255.255.255.0 10.150.111.5 global
ip route vrf INTERNET 10.150.106.0 255.255.255.0 10.150.111.5 global
ip route vrf INTERNET 10.150.110.0 255.255.255.0 10.150.111.5 global
!
!
ip prefix-list OFFICE-NETS seq 5 permit 10.150.104.0/24
ip prefix-list OFFICE-NETS seq 10 permit 10.150.105.0/24
ip prefix-list OFFICE-NETS seq 15 permit 10.150.106.0/24
ip prefix-list OFFICE-NETS seq 20 permit 10.150.110.0/24
!
route-map RM-OFFICE permit 10
 match ip address prefix-list OFFICE-NETS
!
!
!
control-plane
!
!
!
!
!
!
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login local
 transport input ssh
!
!
end



# r-branch.txt

r-branch#sh run
Building configuration...

Current configuration : 2065 bytes
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname r-branch
!
boot-start-marker
boot-end-marker
!
aqm-register-fnf
!
vrf definition INTERNET
 !
 address-family ipv4
 exit-address-family
!
!
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
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
no ip domain lookup
ip domain name skills.local
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
!
username itcore privilege 15 secret 5 $1$bOez$miQXQDbdzv7xJdtOyODO00
!
redundancy
!
!
ip ssh version 2
!
crypto keyring KR-EDGE vrf INTERNET
  pre-shared-key address 203.0.113.1 key P@ssw0rd
!
!
!
!
crypto isakmp policy 10
 encr aes 256
 hash sha256
 authentication pre-share
 group 14
crypto isakmp profile ISA-EDGE
   keyring KR-EDGE
   match identity address 203.0.113.1 255.255.255.255 INTERNET
!
!
crypto ipsec transform-set TS-GRE esp-aes 256 esp-sha256-hmac
 mode transport
!
crypto ipsec profile IPSEC-PROF
 set transform-set TS-GRE
 set isakmp-profile ISA-EDGE
!
!
!
!
!
!
!
interface Tunnel0
 description GRE to r-edge
 ip address 10.150.112.2 255.255.255.252
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source Ethernet0/1
 tunnel destination 203.0.113.1
 tunnel vrf INTERNET
 tunnel protection ipsec profile IPSEC-PROF
!
interface Ethernet0/0
 description BR-LAN (BR-PC1)
 ip address 10.150.108.1 255.255.255.0
!
interface Ethernet0/1
 description -=isp;Ethernet0/1=-
 vrf forwarding INTERNET
 ip address 198.51.100.1 255.255.255.252
!
interface Ethernet0/2
 no ip address
 shutdown
!
interface Ethernet0/3
 no ip address
 shutdown
!
router ospf 1
 router-id 10.150.112.2
 passive-interface Ethernet0/0
 network 10.150.108.0 0.0.0.255 area 0
 network 10.150.112.0 0.0.0.3 area 0
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route vrf INTERNET 0.0.0.0 0.0.0.0 198.51.100.2
!
!
!
!
control-plane
!
!
!
!
!
!
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login local
 transport input ssh
!
!
end


# isp.txt


isp#sh run
Building configuration...

Current configuration : 1414 bytes
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname isp
!
boot-start-marker
boot-end-marker
!
aqm-register-fnf
!
!
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
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
no ip domain lookup
ip domain name skills.local
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
!
username itcore privilege 15 secret 5 $1$cUEk$XEXLvcAl6vlpGPVYG6.OP.
!
redundancy
!
!
ip ssh version 2
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
interface Loopback0
 description test resource "internet"
 ip address 192.0.2.100 255.255.255.255
!
interface Ethernet0/0
 description -=r-edge;Ethernet0/1=-
 ip address 203.0.113.2 255.255.255.252
!
interface Ethernet0/1
 description -=r-branch;Ethernet0/1=-
 ip address 198.51.100.2 255.255.255.252
!
interface Ethernet0/2
 no ip address
 shutdown
!
interface Ethernet0/3
 no ip address
 shutdown
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 10.150.104.0 255.255.255.0 203.0.113.1
ip route 10.150.105.0 255.255.255.0 203.0.113.1
ip route 10.150.106.0 255.255.255.0 203.0.113.1
ip route 10.150.110.0 255.255.255.0 203.0.113.1
ip route 10.150.111.0 255.255.255.252 203.0.113.1
!
!
!
!
control-plane
!
!
!
!
!
!
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login local
 transport input ssh
!
!
end


# fw-rules.txt

![[rules.png]]
Для LAN
pass, protocol: ICMP (echo-request), Source: OFFICE_NETS, Destination: TEST_HOST, BRANCH_NET 
pass, Protocol: TCP, Source Port: any, Destination Port: 80, 443, 22 (алиас PORTS_SVC), Source: OFFICE_NETS, Destination: BRANCH_NET.
правило Block для всего лишнего с включенным логированием

Для WAN
Pass, Protocol: ICMP (destination unreachable) для PMTUD.
Pass, Protocol: ICMP (echo-request), Source: BRANCH_NET, Destination: OFFICE_NETS.
Pass, Protocol: TCP, Source: BRANCH_NET, Destination Port: 80, 443, 22, Destination: OFFICE_NETS.
правило Block для всего лишнего с включенным логированием