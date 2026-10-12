## OSPF
### объясните, какие LSA позволяют r-branch узнать VLAN из area 0 и почему ему не нужна копия LSDB area 0:
r-branch не хранит копию LSDB area 0, пограничный маршрутизатор преобразует информацию из area 0 в Summary LSA Type 3 и рассылает их в area 10.
### После повышения cost
r-branch#show ip route ospf
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
O IA     10.150.104.0/24 [110/1010] via 10.150.111.5, 01:54:05, Ethernet0/0
O IA     10.150.105.0/24 [110/1010] via 10.150.111.5, 01:54:05, Ethernet0/0
O IA     10.150.110.0/24 [110/1010] via 10.150.111.5, 01:54:05, Ethernet0/0
O IA     10.150.111.0/30 [110/2000] via 10.150.111.5, 01:54:21, Ethernet0/0
