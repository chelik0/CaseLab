### r-edge
r-edge#show ip bgp summary
BGP router identifier 10.255.0.2, local AS number 65010
BGP table version is 35, main routing table version 35
4 network entries using 560 bytes of memory
7 path entries using 560 bytes of memory
5/3 BGP path/bestpath attribute entries using 720 bytes of memory
4 BGP AS-PATH entries using 96 bytes of memory
0 BGP route-map cache entries using 0 bytes of memory
0 BGP filter-list cache entries using 0 bytes of memory
BGP using 1936 total bytes of memory
BGP activity 19/15 prefixes, 37/30 paths, scan interval 60 secs

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
198.51.100.2    4        65002      35      34       35    0    0 00:27:06        3
203.0.113.2     4        65001      35      34       35    0    0 00:27:04        3
r-edge#show ip bgp
BGP table version is 35, local router ID is 10.255.0.2
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *   0.0.0.0          203.0.113.2              0             0 65001 i
 *>                   198.51.100.2             0             0 65002 i
 *>  10.150.104.0/21  0.0.0.0                  0         32768 i
 *   192.0.2.100/32   198.51.100.2                           0 65002 65000 i
 *>                   203.0.113.2                            0 65001 65000 i
 *   192.0.2.200/32   198.51.100.2                           0 65002 65000 i
 *>                   203.0.113.2                            0 65001 65000 i
r-edge#show ip bgp 10.150.104.0
BGP routing table entry for 10.150.104.0/21, version 23
Paths: (1 available, best #1, table default)
  Advertised to update-groups:
     12         13
  Refresh Epoch 1
  Local
    0.0.0.0 from 0.0.0.0 (10.255.0.2)
      Origin IGP, metric 0, localpref 100, weight 32768, valid, sourced, local, best
      rx pathid: 0, tx pathid: 0x0
r-edge#show ip bgp 192.0.2.100
BGP routing table entry for 192.0.2.100/32, version 34
Paths: (2 available, best #2, table default)
  Not advertised to any peer
  Refresh Epoch 1
  65002 65000
    198.51.100.2 from 198.51.100.2 (10.255.0.12)
      Origin IGP, localpref 100, valid, external
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  65001 65000
    203.0.113.2 from 203.0.113.2 (10.255.0.11)
      Origin IGP, localpref 100, valid, external, best
      rx pathid: 0, tx pathid: 0x0
r-edge#show ip route 0.0.0.0
Routing entry for 0.0.0.0/0, supernet
  Known via "bgp 65010", distance 20, metric 0, candidate default path
  Tag 65002, type external
  Last update from 198.51.100.2 00:27:06 ago
  Routing Descriptor Blocks:
  * 198.51.100.2, from 198.51.100.2, 00:27:06 ago
      Route metric is 0, traffic share count is 1
      AS Hops 1
      Route tag 65002
      MPLS label: none
r-edge#show ip route 192.0.2.100
Routing entry for 192.0.2.100/32
  Known via "bgp 65010", distance 20, metric 0
  Tag 65001, type external
  Last update from 203.0.113.2 00:27:06 ago
  Routing Descriptor Blocks:
  * 203.0.113.2, from 203.0.113.2, 00:27:06 ago
      Route metric is 0, traffic share count is 1
      AS Hops 2
      Route tag 65001
      MPLS label: none


### isp-a
isp-a#show ip bgp summary
BGP router identifier 10.255.0.11, local AS number 65001
BGP table version is 29, main routing table version 29
4 network entries using 560 bytes of memory
4 path entries using 320 bytes of memory
3/3 BGP path/bestpath attribute entries using 432 bytes of memory
2 BGP AS-PATH entries using 48 bytes of memory
0 BGP route-map cache entries using 0 bytes of memory
0 BGP filter-list cache entries using 0 bytes of memory
BGP using 1360 total bytes of memory
BGP activity 16/12 prefixes, 16/12 paths, scan interval 60 secs

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
192.0.2.2       4        65000      34      33       29    0    0 00:26:32        2
203.0.113.1     4        65010      33      34       29    0    0 00:26:29        1
isp-a#show ip bgp
BGP table version is 29, local router ID is 10.255.0.11
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  0.0.0.0          0.0.0.0                  0         32768 i
 *>  10.150.104.0/21  203.0.113.1              0             0 65010 i
 *>  192.0.2.100/32   192.0.2.2                0             0 65000 i
 *>  192.0.2.200/32   192.0.2.2                0             0 65000 i
isp-a#show ip bgp 10.150.104.0
BGP routing table entry for 10.150.104.0/21, version 29
Paths: (1 available, best #1, table default)
  Advertised to update-groups:
     12
  Refresh Epoch 1
  65010
    203.0.113.1 from 203.0.113.1 (10.255.0.2)
      Origin IGP, metric 0, localpref 100, valid, external, best
      rx pathid: 0, tx pathid: 0x0
isp-a#show ip bgp 192.0.2.100
BGP routing table entry for 192.0.2.100/32, version 27
Paths: (1 available, best #1, table default)
  Advertised to update-groups:
     13
  Refresh Epoch 1
  65000
    192.0.2.2 from 192.0.2.2 (10.255.0.100)
      Origin IGP, metric 0, localpref 100, valid, external, best
      rx pathid: 0, tx pathid: 0x0
isp-a#show ip route 0.0.0.0
Routing entry for 0.0.0.0/0, supernet
  Known via "static", distance 1, metric 0 (connected), candidate default path
  Advertised by bgp 65001
  Routing Descriptor Blocks:
  * directly connected, via Null0
      Route metric is 0, traffic share count is 1
isp-a#show ip route 192.0.2.100
Routing entry for 192.0.2.100/32
  Known via "bgp 65001", distance 20, metric 0
  Tag 65000, type external
  Last update from 192.0.2.2 00:26:33 ago
  Routing Descriptor Blocks:
  * 192.0.2.2, from 192.0.2.2, 00:26:33 ago
      Route metric is 0, traffic share count is 1
      AS Hops 1
      Route tag 65000
      MPLS label: none


### isp-b
isp-b#show ip bgp summary
BGP router identifier 10.255.0.12, local AS number 65002
BGP table version is 29, main routing table version 29
4 network entries using 560 bytes of memory
4 path entries using 320 bytes of memory
3/3 BGP path/bestpath attribute entries using 432 bytes of memory
2 BGP AS-PATH entries using 48 bytes of memory
0 BGP route-map cache entries using 0 bytes of memory
0 BGP filter-list cache entries using 0 bytes of memory
BGP using 1360 total bytes of memory
BGP activity 16/12 prefixes, 16/12 paths, scan interval 60 secs

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
192.0.2.6       4        65000      35      34       29    0    0 00:25:52        2
198.51.100.1    4        65010      33      34       29    0    0 00:25:54        1
isp-b#show ip bgp
BGP table version is 29, local router ID is 10.255.0.12
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  0.0.0.0          0.0.0.0                  0         32768 i
 *>  10.150.104.0/21  198.51.100.1             0             0 65010 i
 *>  192.0.2.100/32   192.0.2.6                0             0 65000 i
 *>  192.0.2.200/32   192.0.2.6                0             0 65000 i
isp-b#show ip bgp 10.150.104.0
BGP routing table entry for 10.150.104.0/21, version 27
Paths: (1 available, best #1, table default)
  Advertised to update-groups:
     13
  Refresh Epoch 1
  65010
    198.51.100.1 from 198.51.100.1 (10.255.0.2)
      Origin IGP, metric 0, localpref 100, valid, external, best
      rx pathid: 0, tx pathid: 0x0
isp-b#show ip bgp 192.0.2.100
BGP routing table entry for 192.0.2.100/32, version 28
Paths: (1 available, best #1, table default)
  Advertised to update-groups:
     12
  Refresh Epoch 2
  65000
    192.0.2.6 from 192.0.2.6 (10.255.0.100)
      Origin IGP, metric 0, localpref 100, valid, external, best
      rx pathid: 0, tx pathid: 0x0
isp-b#show ip route 0.0.0.0
Routing entry for 0.0.0.0/0, supernet
  Known via "static", distance 1, metric 0 (connected), candidate default path
  Advertised by bgp 65002
  Routing Descriptor Blocks:
  * directly connected, via Null0
      Route metric is 0, traffic share count is 1
isp-b#show ip route 192.0.2.100
Routing entry for 192.0.2.100/32
  Known via "bgp 65002", distance 20, metric 0
  Tag 65000, type external
  Last update from 192.0.2.6 00:25:55 ago
  Routing Descriptor Blocks:
  * 192.0.2.6, from 192.0.2.6, 00:25:55 ago
      Route metric is 0, traffic share count is 1
      AS Hops 1
      Route tag 65000
      MPLS label: none

### inet
inet#show ip bgp summary
BGP router identifier 10.255.0.100, local AS number 65000
BGP table version is 8, main routing table version 8
3 network entries using 420 bytes of memory
4 path entries using 320 bytes of memory
3/2 BGP path/bestpath attribute entries using 432 bytes of memory
2 BGP AS-PATH entries using 48 bytes of memory
0 BGP route-map cache entries using 0 bytes of memory
0 BGP filter-list cache entries using 0 bytes of memory
BGP using 1220 total bytes of memory
BGP activity 5/2 prefixes, 8/4 paths, scan interval 60 secs

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
192.0.2.1       4        65001      30      30        8    0    0 00:23:20        1
192.0.2.5       4        65002      31      32        8    0    0 00:23:16        1
inet#show ip bgp
BGP table version is 8, local router ID is 10.255.0.100
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *   10.150.104.0/21  192.0.2.1                              0 65001 65010 i
 *>                   192.0.2.5                              0 65002 65010 i
 *>  192.0.2.100/32   0.0.0.0                  0         32768 i
 *>  192.0.2.200/32   0.0.0.0                  0         32768 i
inet#show ip bgp 10.150.104.0
BGP routing table entry for 10.150.104.0/21, version 8
Paths: (2 available, best #2, table default)
  Not advertised to any peer
  Refresh Epoch 1
  65001 65010
    192.0.2.1 from 192.0.2.1 (10.255.0.11)
      Origin IGP, localpref 100, valid, external
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  65002 65010
    192.0.2.5 from 192.0.2.5 (10.255.0.12)
      Origin IGP, localpref 100, valid, external, best
      rx pathid: 0, tx pathid: 0x0
inet#show ip bgp 192.0.2.100
BGP routing table entry for 192.0.2.100/32, version 2
Paths: (1 available, best #1, table default)
  Advertised to update-groups:
     6
  Refresh Epoch 1
  Local
    0.0.0.0 from 0.0.0.0 (10.255.0.100)
      Origin IGP, metric 0, localpref 100, weight 32768, valid, sourced, local, best
      rx pathid: 0, tx pathid: 0x0
inet#show ip route 0.0.0.0
% Network not in table
inet#show ip route 192.0.2.100
Routing entry for 192.0.2.100/32
  Known via "connected", distance 0, metric 0 (connected, via interface)
  Advertised by bgp 65000
  Routing Descriptor Blocks:
  * directly connected, via Loopback10
      Route metric is 0, traffic share count is 1
