# VRF INTERNET
## r-edge
### r-edge#show vrf
  Name                             Default RD            Protocols   Interfaces
  INTERNET                         not set             ipv4        Et0/1
### r-edge#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is not set

      10.0.0.0/8 is variably subnetted, 7 subnets, 3 masks
S        10.150.104.0/24 [1/0] via 10.150.111.5
S        10.150.105.0/24 [1/0] via 10.150.111.5
S        10.150.106.0/24 [1/0] via 10.150.111.5
S        10.150.110.0/24 [1/0] via 10.150.111.5
S        10.150.111.0/30 [1/0] via 10.150.111.5
C        10.150.111.4/30 is directly connected, Ethernet0/0
L        10.150.111.6/32 is directly connected, Ethernet0/0
      192.0.2.0/32 is subnetted, 1 subnets
S        192.0.2.100 [1/0] via 203.0.113.2, Ethernet0/1
### r-edge#show ip route vrf INTERNET

Routing Table: INTERNET
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
      10.0.0.0/24 is subnetted, 4 subnets
S        10.150.104.0 [1/0] via 10.150.111.5
S        10.150.105.0 [1/0] via 10.150.111.5
S        10.150.106.0 [1/0] via 10.150.111.5
S        10.150.110.0 [1/0] via 10.150.111.5
      203.0.113.0/24 is variably subnetted, 2 subnets, 2 masks
C        203.0.113.0/30 is directly connected, Ethernet0/1
L        203.0.113.1/32 is directly connected, Ethernet0/1
### r-edge#ping vrf INTERNET 203.0.113.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 203.0.113.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
r-edge#ct
% Incomplete command.

### r-edge#ping 203.0.113.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 203.0.113.2, timeout is 2 seconds:
.....
Success rate is 0 percent (0/5)


## r-branch
### r-branch>show vrf
  Name                             Default RD            Protocols   Interfaces
  INTERNET                         not set             ipv4        Et0/1
### r-branch>show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is not set

      10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C        10.150.108.0/24 is directly connected, Ethernet0/0
L        10.150.108.1/32 is directly connected, Ethernet0/0
### r-branch>show ip route vrf INTERNET

Routing Table: INTERNET
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 198.51.100.2 to network 0.0.0.0

S*    0.0.0.0/0 [1/0] via 198.51.100.2
      198.51.100.0/24 is variably subnetted, 2 subnets, 2 masks
C        198.51.100.0/30 is directly connected, Ethernet0/1
L        198.51.100.1/32 is directly connected, Ethernet0/1
### r-branch>ping vrf INTERNET 198.51.100.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 198.51.100.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
### r-branch>ping 198.51.100.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 198.51.100.2, timeout is 2 seconds:
.....
Success rate is 0 percent (0/5)

## краткое объяснение
VRF это отдельная таблица маршрутизации. Транспортная сеть изолирована от внутренних сетей, и адреса провайдера не попадают в глобальную таблицу. Обычный ping ищет маршрут только в глобальной таблице


# VRF MGMT
### sw-core#show vrf
  Name                             Default RD            Protocols   Interfaces
  MGMT                             not set            ipv4        Vl99
### sw-core#show running-config interface Vlan99
Building configuration...

Current configuration : 110 bytes
!
interface Vlan99
 description gateway MGMT
 vrf forwarding MGMT
 ip address 10.150.110.1 255.255.255.0
end
### sw-core#show ip route vrf MGMT

Routing Table: MGMT
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR

Gateway of last resort is not set

      10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C        10.150.110.0/24 is directly connected, Vlan99
L        10.150.110.1/32 is directly connected, Vlan99
### sw-core#ping vrf MGMT 10.150.110.11
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.150.110.11, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
### sw-core#show ip arp vrf MGMT
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.150.110.1            -   aabb.cc80.0300  ARPA   Vlan99
Internet  10.150.110.11           5   aabb.cc80.0100  ARPA   Vlan99
### sw-core#ping vrf MGMT 10.150.104.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.150.104.1, timeout is 2 seconds:
.....
Success rate is 0 percent (0/5)
sw-core#show ip route vrf MGMT 10.150.104.0

Routing Table: MGMT
% Subnet not in table
### sw-core#ping vrf MGMT 10.150.105.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.150.105.1, timeout is 2 seconds:
.....
Success rate is 0 percent (0/5)
### sw-core#ping vrf MGMT 10.150.106.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.150.106.1, timeout is 2 seconds:
.....
Success rate is 0 percent (0/5)
sw-core#show ip route vrf MGMT 10.150.106.0

Routing Table: MGMT
% Subnet not in table
