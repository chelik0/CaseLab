## 9.1. Агрегация и L2 в офисе A
### 9.1.1 A-SW-CORE#show etherchannel summary
Flags:  D - down        P - bundled in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      N - not in use, no aggregation
        f - failed to allocate aggregator

        M - not in use, minimum links not met
        m - not in use, port not aggregated due to minimum links not met
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port

        A - formed by Auto LAG


Number of channel-groups in use: 2
Number of aggregators:           2

Group  Port-channel  Protocol    Ports
------+-------------+-----------+-----------------------------------------------
1      Po1(SU)         LACP      Et0/0(P)    Et0/1(P)
2      Po2(SU)         LACP      Et0/2(P)    Et0/3(P)

### 9.1.2 A-SW-CORE#show interfaces trunk

Port        Mode             Encapsulation  Status        Native vlan
Po1         on               802.1q         trunking      999
Po2         on               802.1q         trunking      999

Port        Vlans allowed on trunk
Po1         10,20,99,999
Po2         10,20,99,999

Port        Vlans allowed and active in management domain
Po1         10,20,99,999
Po2         10,20,99,999

Port        Vlans in spanning tree forwarding state and not pruned
Po1         10,20,99,999
Po2         10,20,99,999
A-SW-CORE#
A-SW-CORE#
### 9.1.3 A-SW-CORE#show spanning-tree vlan 10

VLAN0010
  Spanning tree enabled protocol ieee
  Root ID    Priority    32778
             Address     aabb.cc00.0100
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32778  (priority 32768 sys-id-ext 10)
             Address     aabb.cc00.0100
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Po1                 Desg FWD 56        128.65   P2p
Po2                 Desg FWD 56        128.66   P2p
## 9.2. Маршрутизация внутри сети

### 9.2.1 show ip interface brief и ping 10.150.111.254
#### A-SW-CORE#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
Ethernet0/0            unassigned      YES unset  up                    up
Ethernet0/1            unassigned      YES unset  up                    up
Ethernet0/2            unassigned      YES unset  up                    up
Ethernet0/3            unassigned      YES unset  up                    up
Ethernet1/0            10.150.111.253  YES NVRAM  up                    up
Ethernet1/1            unassigned      YES unset  up                    up
Ethernet1/2            unassigned      YES unset  up                    up
Ethernet1/3            unassigned      YES unset  up                    up
Port-channel1          unassigned      YES unset  up                    up
Port-channel2          unassigned      YES unset  up                    up
Vlan10                 10.150.104.1    YES NVRAM  up                    up
Vlan20                 10.150.105.1    YES NVRAM  up                    up
Vlan99                 10.150.110.1    YES NVRAM  up                    up
#### A-SW-CORE#ping 10.150.111.254
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.150.111.254, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms

### 9.2.2 ping SRV1-A, ping PC1-B, ping PC1-C — подряд, одним блоком
![[Pasted image 20260912005907.png]]

### 9.2.3 ping PC1-C
![[Pasted image 20260912010014.png]]
### 9.2.4 ping 192.168.5.98

A-1-BR#ping 192.168.5.98
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.5.98, timeout is 2 seconds:
.....
Success rate is 0 percent (0/5)
## 9.3. Выход в интернет
### 9.3.1 ping 77.88.8.8
![[Pasted image 20260912023319.png]]![[Pasted image 20260912023355.png]]![[Pasted image 20260912023436.png]]
### 9.3.2 traceroute 77.88.8.8
![[Pasted image 20260912024202.png]]
### 9.3.3 show ip route
INET#sh ip route
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

      77.0.0.0/32 is subnetted, 1 subnets
C        77.88.8.8 is directly connected, Loopback0
      203.0.113.0/24 is variably subnetted, 2 subnets, 2 masks
C        203.0.113.0/24 is directly connected, Ethernet0/0
L        203.0.113.1/32 is directly connected, Ethernet0/0

## 9.4. Публикация сервера
### 9.4.1 ping 203.0.113.20, затем telnet 203.0.113.20 80
INET#ping 203.0.113.20
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 203.0.113.20, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms
INET#telnet 203.0.113.20 80
Trying 203.0.113.20, 80 ... Open
### 9.4.2 show ip nat translations
A-1-BR#show ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
icmp 203.0.113.20:2    10.150.105.10:2    203.0.113.1:2      203.0.113.1:2
tcp 203.0.113.20:80    10.150.105.10:80   203.0.113.1:20217  203.0.113.1:20217
--- 203.0.113.20       10.150.105.10      ---                ---

## 9.5. Проверки «от обратного»
### 9.5.1 ping 10.150.105.10 от INET
INET#ping 10.150.105.10
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.150.105.10, timeout is 2 seconds:
.....
Success rate is 0 percent (0/5)
### 9.5.2 ping 203.0.113.1 от PC1-C
![[Pasted image 20260912025507.png]]не проходит потому что нет дефолтного и нет прямого маршрута к сети 203.0.113.0/24, на C-1-BR всего три статических маршрута.
### 9.5.3 ping 77.88.8.8 от PC1-C
![[Pasted image 20260912025452.png]]
через C-1-BR есть маршрут к 77.88.8.8. Исправить можно добавив ещё один маршрут "ip route 203.0.113.0 255.255.255.0 172.17.5.25"
## 9.6. DHCP
### 9.6.1 show ip PC1-A
![[Pasted image 20260912030833.png]]
### 9.6.2 show ip dhcp binding A-SW-CORE
A-SW-CORE#show ip dhcp binding
Bindings from all pools not associated with VRF:
IP address      Client-ID/              Lease expiration        Type       State      Interface
                Hardware address/
                User name
10.150.104.21    0100.5079.6668.08       Sep 12 2026 09:42 PM    Automatic  Active     Vlan10

### 9.6.3 ping SRV1-A от PC1-A
![[Pasted image 20260912031028.png]]