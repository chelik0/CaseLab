## 6. ## IP SLA
### r-edge#show ip sla configuration 20
IP SLAs Infrastructure Engine-III
Entry number: 20
Owner:
Tag:
Operation timeout (milliseconds): 5000
Type of operation to perform: icmp-echo
Target address/Source interface: 192.0.2.200/Ethernet0/0
Type Of Service parameter: 0x0
Request size (ARR data portion): 28
Verify data: No
Vrf Name:
Schedule:
   Operation frequency (seconds): 5  (not considered if randomly scheduled)
   Next Scheduled Start Time: Start Time already passed
   Group Scheduled : FALSE
   Randomly Scheduled : FALSE
   Life (seconds): Forever
   Entry Ageout (seconds): never
   Recurring (Starting Everyday): FALSE
   Status of entry (SNMP RowStatus): Active
Threshold (milliseconds): 5000
Distribution Statistics:
   Number of statistic hours kept: 2
   Number of statistic distribution buckets kept: 1
   Statistic distribution interval (milliseconds): 20
Enhanced History:
History Statistics:
   Number of history Lives kept: 0
   Number of history Buckets kept: 15
   History Filter Type: None


### r-edge#show ip sla statistics 20
IPSLAs Latest Operation Statistics

IPSLA operation id: 20
        Latest RTT: 1 milliseconds
Latest operation start time: 21:59:32 UTC Thu Sep 17 2026
Latest operation return code: OK
Number of successes: 44
Number of failures: 13
Operation time to live: Forever


### r-edge#show track 20
Track 20
  IP SLA 20 state
  State is Up
    4 changes, last change 00:00:32
  Delay up 5 secs, down 10 secs
  Latest operation return code: OK
  Latest RTT (millisecs) 1
  Tracked by:
    Route Map 0
    EEM applet TRACK20-UP
    EEM applet TRACK20-DOWN
### r-edge#show route-map SERVERS-VIA-ISP-B
route-map SERVERS-VIA-ISP-B, permit, sequence 10
  Match clauses:
    ip address (access-lists): SERVERS-TO-TESTSVC
  Set clauses:
    ip next-hop verify-availability 198.51.100.2 10 track 20  [up]
  Policy routing matches: 174 packets, 12876 bytes
route-map SERVERS-VIA-ISP-B, deny, sequence 20
  Match clauses:
  Set clauses:
  Policy routing matches: 0 packets, 0 bytes
### r-edge#show ip policy
Interface      Route map
Ethernet0/0    SERVERS-VIA-ISP-B


![[Ping_SRV1-A_IP SLA.png]]


## 7. EEM
### r-edge#show event manager policy registered
No.  Class     Type    Event Type          Trap  Time Registered           Name
1    applet    user    track               Off   Thu Sep 17 20:39:40 2026  TRACK20-DOWN
 track 20 state down
 maxrun 20.000
 action 1.0 syslog priority critical msg "ISP-B path unreachable (track 20 down)"

2    applet    user    track               Off   Thu Sep 17 20:39:48 2026  TRACK20-UP
 track 20 state up
 maxrun 20.000
 action 1.0 syslog priority notifications msg "ISP-B path restored (track 20 up)"

### r-edge#show event manager history events
No.  Job Id Proc Status   Time of Event            Event Type        Name
1    1      Actv success  Thu Sep17 21:54:57 2026  track             applet: TRACK20-UP
2    2      Actv success  Thu Sep17 21:58:03 2026  track             applet: TRACK20-DOWN
3    3      Actv success  Thu Sep17 21:59:03 2026  track             applet: TRACK20-UP
### r-edge#show logging | include ISP-B
*Sep 17 21:54:57.866: %HA_EM-5-LOG: TRACK20-UP: ISP-B path restored (track 20 up)
*Sep 17 21:58:03.346: %HA_EM-2-LOG: TRACK20-DOWN: ISP-B path unreachable (track 20 down)
*Sep 17 21:59:03.466: %HA_EM-5-LOG: TRACK20-UP: ISP-B path restored (track 20 up)
r-edge#
