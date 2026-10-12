# sw-core.txt
sw-core#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR

Gateway of last resort is 10.150.111.2 to network 0.0.0.0

S*    0.0.0.0/0 [1/0] via 10.150.111.2
      10.0.0.0/8 is variably subnetted, 10 subnets, 3 masks
C        10.150.104.0/24 is directly connected, Vlan10
L        10.150.104.1/32 is directly connected, Vlan10
C        10.150.105.0/24 is directly connected, Vlan20
L        10.150.105.1/32 is directly connected, Vlan20
C        10.150.106.0/24 is directly connected, Vlan30
L        10.150.106.1/32 is directly connected, Vlan30
C        10.150.110.0/24 is directly connected, Vlan99
L        10.150.110.1/32 is directly connected, Vlan99
C        10.150.111.0/30 is directly connected, Ethernet0/2
L        10.150.111.1/32 is directly connected, Ethernet0/2



# r-edge.txt
r-edge# show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 203.0.113.2 to network 0.0.0.0

S*    0.0.0.0/0 [1/0] via 203.0.113.2
      10.0.0.0/8 is variably subnetted, 6 subnets, 3 masks
S        10.150.104.0/24 [1/0] via 10.150.111.1
S        10.150.105.0/24 [1/0] via 10.150.111.1
S        10.150.106.0/24 [1/0] via 10.150.111.1
S        10.150.110.0/24 [1/0] via 10.150.111.1
C        10.150.111.0/30 is directly connected, Ethernet0/0
L        10.150.111.2/32 is directly connected, Ethernet0/0
      203.0.113.0/24 is variably subnetted, 2 subnets, 2 masks
C        203.0.113.0/30 is directly connected, Ethernet0/1
L        203.0.113.1/32 is directly connected, Ethernet0/1


# r-branch.txt

r-branch#sh run
Building configuration...

Current configuration : 1147 bytes
!
! Last configuration change at 22:19:44 UTC Sun Sep 27 2026
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
 description BR-LAN (BR-PC1)
 ip address 10.150.108.1 255.255.255.0
!
interface Ethernet0/1
 description -=isp;Ethernet0/1=-
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
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 198.51.100.2
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

Current configuration : 1459 bytes
!
! Last configuration change at 22:19:59 UTC Sun Sep 27 2026
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

# fw.txt

![[case lab/NETWORK/week4-Shestakov/01-topology.png]]