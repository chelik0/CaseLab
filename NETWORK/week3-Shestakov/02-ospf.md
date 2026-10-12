show ip ospf neighbor
show ip ospf interface brief
show ip ospf database
 show ip route ospf
 
## sw-core
### sw-core#show ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
10.255.0.2        0   FULL/  -        00:00:36    10.150.111.2    Ethernet0/2
10.255.0.3        0   FULL/  -        00:00:37    10.150.111.6    Ethernet0/3
### sw-core#show ip ospf interface brief
Interface    PID   Area            IP Address/Mask    Cost  State Nbrs F/C
Vl99         10    0               10.150.110.1/24    10    DR    0/0
Vl20         10    0               10.150.105.1/24    10    DR    0/0
Vl10         10    0               10.150.104.1/24    10    DR    0/0
Et0/2        10    0               10.150.111.1/30    1000  P2P   1/1
Et0/3        10    10              10.150.111.5/30    1000  P2P   1/1
### sw-core#show ip ospf database

            OSPF Router with ID (10.255.0.1) (Process ID 10)

                Router Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum Link count
10.255.0.1      10.255.0.1      430         0x80000005 0x0098FE 5
10.255.0.2      10.255.0.2      487         0x80000003 0x005DAA 2

                Summary Net Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum
10.150.108.0    10.255.0.1      488         0x80000001 0x0056F6
10.150.108.0    10.255.0.2      483         0x80000001 0x0050FB
10.150.111.4    10.255.0.1      488         0x80000001 0x00C76D
10.150.111.4    10.255.0.2      483         0x80000001 0x00F453
10.150.111.8    10.255.0.1      488         0x80000001 0x00D272
10.150.111.8    10.255.0.2      483         0x80000001 0x009996

                Router Link States (Area 10)

Link ID         ADV Router      Age         Seq#       Checksum Link count
10.255.0.1      10.255.0.1      490         0x80000003 0x0004FC 2

### sw-core#show ip route ospf
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

      10.0.0.0/8 is variably subnetted, 12 subnets, 3 masks
O        10.150.108.0/24 [110/2000] via 10.150.111.6, 00:08:13, Ethernet0/3
O        10.150.111.8/30 [110/2000] via 10.150.111.6, 00:08:13, Ethernet0/3

## r-edge
### r-edge#show ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
10.255.0.1        0   FULL/  -        00:00:32    10.150.111.1    Ethernet0/0
10.255.0.3        0   FULL/  -        00:00:38    10.150.111.10   Ethernet0/1
### r-edge#show ip ospf interface brief
Interface    PID   Area            IP Address/Mask    Cost  State Nbrs F/C
Et0/0        10    0               10.150.111.2/30    1000  P2P   1/1
Et0/1        10    10              10.150.111.9/30    1000  P2P   1/1
### r-edge#show ip ospf database

            OSPF Router with ID (10.255.0.2) (Process ID 10)

                Router Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum Link count
10.255.0.1      10.255.0.1      1020        0x80000007 0x009401 5
10.255.0.2      10.255.0.2      975         0x80000005 0x0059AC 2

                Summary Net Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum
10.150.108.0    10.255.0.1      1020        0x80000003 0x0052F8
10.150.108.0    10.255.0.2      975         0x80000003 0x004CFD
10.150.111.4    10.255.0.1      1020        0x80000003 0x00C36F
10.150.111.4    10.255.0.2      975         0x80000003 0x00F055
10.150.111.8    10.255.0.1      1020        0x80000003 0x00CE74
10.150.111.8    10.255.0.2      975         0x80000003 0x009598

                Router Link States (Area 10)

Link ID         ADV Router      Age         Seq#       Checksum Link count
10.255.0.1      10.255.0.1      1021        0x80000005 0x00FFFE 2
10.255.0.2      10.255.0.2      975         0x80000005 0x008074 2
10.255.0.3      10.255.0.3      988         0x80000005 0x00784A 5

                Summary Net Link States (Area 10)

Link ID         ADV Router      Age         Seq#       Checksum
10.150.104.0    10.255.0.1      1021        0x80000003 0x007BA1
10.150.104.0    10.255.0.2      975         0x80000003 0x00A986
10.150.105.0    10.255.0.1      1021        0x80000003 0x0070AB
10.150.105.0    10.255.0.2      975         0x80000003 0x009E90
10.150.110.0    10.255.0.1      1021        0x80000003 0x0039DD
10.150.110.0    10.255.0.2      975         0x80000003 0x0067C2
10.150.111.0    10.255.0.1      1021        0x80000003 0x00EB4B
10.150.111.0    10.255.0.2      975         0x80000003 0x00E550
### r-edge#show ip route ospf
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

      10.0.0.0/8 is variably subnetted, 9 subnets, 3 masks
O        10.150.104.0/24 [110/1010] via 10.150.111.1, 01:23:27, Ethernet0/0
O        10.150.105.0/24 [110/1010] via 10.150.111.1, 01:23:27, Ethernet0/0
O        10.150.108.0/24 [110/2000] via 10.150.111.10, 01:23:44, Ethernet0/1
O        10.150.110.0/24 [110/1010] via 10.150.111.1, 01:23:27, Ethernet0/0
O        10.150.111.4/30 [110/2000] via 10.150.111.10, 01:23:44, Ethernet0/1

## r-branch
### r-branch#show ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
10.255.0.2        0   FULL/  -        00:00:39    10.150.111.9    Ethernet0/1
10.255.0.1        0   FULL/  -        00:00:37    10.150.111.5    Ethernet0/0
### r-branch#show ip ospf interface brief
Interface    PID   Area            IP Address/Mask    Cost  State Nbrs F/C
Et0/2        10    10              10.150.108.1/24    1000  DR    0/0
Et0/1        10    10              10.150.111.10/30   1000  P2P   1/1
Et0/0        10    10              10.150.111.6/30    1000  P2P   1/1
### r-branch#show ip ospf database

            OSPF Router with ID (10.255.0.3) (Process ID 10)

                Router Link States (Area 10)

Link ID         ADV Router      Age         Seq#       Checksum Link count
10.255.0.1      10.255.0.1      1070        0x80000005 0x00FFFE 2
10.255.0.2      10.255.0.2      1027        0x80000005 0x008074 2
10.255.0.3      10.255.0.3      1037        0x80000005 0x00784A 5

                Summary Net Link States (Area 10)

Link ID         ADV Router      Age         Seq#       Checksum
10.150.104.0    10.255.0.1      1070        0x80000003 0x007BA1
10.150.104.0    10.255.0.2      1027        0x80000003 0x00A986
10.150.105.0    10.255.0.1      1070        0x80000003 0x0070AB
10.150.105.0    10.255.0.2      1027        0x80000003 0x009E90
10.150.110.0    10.255.0.1      1070        0x80000003 0x0039DD
10.150.110.0    10.255.0.2      1027        0x80000003 0x0067C2
10.150.111.0    10.255.0.1      1070        0x80000003 0x00EB4B
10.150.111.0    10.255.0.2      1027        0x80000003 0x00E550
### r-branch# show ip route ospf
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

      10.0.0.0/8 is variably subnetted, 10 subnets, 3 masks
O IA     10.150.104.0/24 [110/1010] via 10.150.111.5, 01:24:18, Ethernet0/0
O IA     10.150.105.0/24 [110/1010] via 10.150.111.5, 01:24:18, Ethernet0/0
O IA     10.150.110.0/24 [110/1010] via 10.150.111.5, 01:24:18, Ethernet0/0
O IA     10.150.111.0/30 [110/2000] via 10.150.111.9, 01:24:34, Ethernet0/1
                         [110/2000] via 10.150.111.5, 01:24:34, Ethernet0/0