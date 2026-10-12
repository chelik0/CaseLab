Файл подтверждает, что GRE over IPsec и маршрутизация через туннель работают.


r-edge#ping vrf INTERNET 198.51.100.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 198.51.100.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms
## show crypto isakmp s
r-edge#show crypto isakmp sa
IPv4 Crypto ISAKMP SA
dst             src             state          conn-id status
203.0.113.1     198.51.100.1    QM_IDLE           1001 ACTIVE
198.51.100.1    203.0.113.1     QM_IDLE           1002 ACTIVE

IPv6 Crypto ISAKMP SA

## состояние интерфейса Tunnel0 и его адресацию;
r-edge#show interfaces tunnel0
Tunnel0 is up, line protocol is up
  Hardware is Tunnel
  Description: GRE to r-branch
  Internet address is 10.150.112.1/30
  MTU 17874 bytes, BW 100 Kbit/sec, DLY 50000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation TUNNEL, loopback not set
  Keepalive not set
  Tunnel source 203.0.113.1 (Ethernet0/1), destination 198.51.100.1
   Tunnel Subblocks:
      src-track:
         Tunnel0 source tracking subblock associated with Ethernet0/1
          Set of tunnels with source Ethernet0/1, 1 member (includes iterators), on interface OK>
  Tunnel protocol/transport GRE/IP
    Key disabled, sequencing disabled
    Checksumming of packets disabled
  Tunnel TTL 255, Fast tunneling enabled
  Tunnel transport MTU 1434 bytes
  Tunnel transmit bandwidth 8000 (kbps)
  Tunnel receive bandwidth 8000 (kbps)
  Tunnel protection via IPSec (profile "IPSEC-PROF")
  Last input 00:00:02, output never, output hang never
  Last clearing of "show interface" counters 00:11:47
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 16
  Queueing strategy: fifo
  Output queue: 0/0 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     48 packets input, 5044 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     70 packets output, 7256 bytes, 0 underruns
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out

## show crypto ipsec sa

r-edge#ping 10.150.112.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.150.112.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 5/7/9 ms
## r-edge#show crypto ipsec sa

interface: Tunnel0
    Crypto map tag: Tunnel0-head-0, local addr 203.0.113.1

   protected vrf: (none)
   local  ident (addr/mask/prot/port): (203.0.113.1/255.255.255.255/47/0)
   remote ident (addr/mask/prot/port): (198.51.100.1/255.255.255.255/47/0)
   current_peer 198.51.100.1 port 500
     PERMIT, flags={origin_is_acl,}
    #pkts encaps: 65, #pkts encrypt: 65, #pkts digest: 65
    #pkts decaps: 59, #pkts decrypt: 59, #pkts verify: 59
    #pkts compressed: 0, #pkts decompressed: 0
    #pkts not compressed: 0, #pkts compr. failed: 0
    #pkts not decompressed: 0, #pkts decompress failed: 0
    #send errors 0, #recv errors 0

     local crypto endpt.: 203.0.113.1, remote crypto endpt.: 198.51.100.1
     plaintext mtu 1458, path mtu 1500, ip mtu 1500, ip mtu idb Ethernet0/1
     current outbound spi: 0x6FADA3CC(1873650636)
     PFS (Y/N): N, DH group: none

     inbound esp sas:
      spi: 0x86BFA96C(2260707692)
        transform: esp-256-aes esp-sha256-hmac ,
        in use settings ={Transport, }
        conn id: 1, flow_id: SW:1, sibling_flags 80000000, crypto map: Tunnel0-head-0
        sa timing: remaining key lifetime (k/sec): (4608000/3127)
        IV size: 16 bytes
        replay detection support: Y
        ecn bit support: Y status: off
        Status: ACTIVE(ACTIVE)
      spi: 0xEE5AB788(3998922632)
        transform: esp-256-aes esp-sha256-hmac ,
        in use settings ={Transport, }
        conn id: 3, flow_id: SW:3, sibling_flags 80004000, crypto map: Tunnel0-head-0
        sa timing: remaining key lifetime (k/sec): (4148780/3135)
        IV size: 16 bytes
        replay detection support: Y
        ecn bit support: Y status: off
        Status: ACTIVE(ACTIVE)

     inbound ah sas:

     inbound pcp sas:

     outbound esp sas:
      spi: 0x65A065C8(1705010632)
        transform: esp-256-aes esp-sha256-hmac ,
        in use settings ={Transport, }
        conn id: 2, flow_id: SW:2, sibling_flags 80000000, crypto map: Tunnel0-head-0
        sa timing: remaining key lifetime (k/sec): (4608000/3127)
        IV size: 16 bytes
        replay detection support: Y
        ecn bit support: Y status: off
        Status: ACTIVE(ACTIVE)
      spi: 0x6FADA3CC(1873650636)
        transform: esp-256-aes esp-sha256-hmac ,
        in use settings ={Transport, }
        conn id: 4, flow_id: SW:4, sibling_flags 80004000, crypto map: Tunnel0-head-0
        sa timing: remaining key lifetime (k/sec): (4148779/3135)
        IV size: 16 bytes
        replay detection support: Y
        ecn bit support: Y status: off
        Status: ACTIVE(ACTIVE)

     outbound ah sas:

     outbound pcp sas:


## show ip ospf neighbor с соседством через Tunnel0;
### r-edge#show ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
10.150.112.2      0   FULL/  -        00:00:34    10.150.112.2    Tunnel0
r-edge#show ip route ospf
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
O        10.150.108.0/24 [110/1010] via 10.150.112.2, 00:07:40, Tunnel0

### r-branch#show ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
10.150.112.1      0   FULL/  -        00:00:31    10.150.112.1    Tunnel0
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

      10.0.0.0/8 is variably subnetted, 8 subnets, 3 masks
O E2     10.150.104.0/24 [110/20] via 10.150.112.1, 00:15:38, Tunnel0
O E2     10.150.105.0/24 [110/20] via 10.150.112.1, 00:15:38, Tunnel0
O E2     10.150.106.0/24 [110/20] via 10.150.112.1, 00:15:38, Tunnel0
O E2     10.150.110.0/24 [110/20] via 10.150.112.1, 00:15:38, Tunnel0

### r-branch#show crypto ipsec sa

interface: Tunnel0
    Crypto map tag: Tunnel0-head-0, local addr 198.51.100.1

   protected vrf: (none)
   local  ident (addr/mask/prot/port): (198.51.100.1/255.255.255.255/47/0)
   remote ident (addr/mask/prot/port): (203.0.113.1/255.255.255.255/47/0)
   current_peer 203.0.113.1 port 500
     PERMIT, flags={origin_is_acl,}
    #pkts encaps: 140, #pkts encrypt: 140, #pkts digest: 140
    #pkts decaps: 150, #pkts decrypt: 150, #pkts verify: 150


## маршрут до сети филиала на стороне офиса и маршрут до офисных сетей на стороне филиала;
### sw-core#show ip route 10.150.108.0
Routing entry for 10.150.108.0/24
  Known via "static", dist ance 1, metric 0
  Routing Descriptor Blocks:
  * 10.150.111.2
      Route metric is 0, traffic share count is 1

## успешный ping от PC1 до BR-PC1;
obsidian://open?vault=Obsidian%20Vault&file=case%20lab%2FNETWORK%2Fweek4-Shestakov%2FMTU_ping.png

Без правил FW туннель установился, но открытый трафик офиса fw отбрасывает.