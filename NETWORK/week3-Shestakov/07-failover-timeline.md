# Отказ линка r-edge — isp-a
## точное действие и время;

r-edge(config)#interface e0/2 
r-edge(config-if)#shutdown

Команда shutdown 11:43:01.983
BGP neighbor 203.0.113.2 reset (interface flap) 11:43:01.984
Interface administratively down 11:43:03.976 
Line protocol down 11:43:04.988 
Команда no shutdown 11:47:14
Interface up 11:47:18.672
Line protocol up 11:47:19.694 
BGP neighbor 203.0.113.2 Established

## что произошло с interface/OSPF/BGP/IP SLA/track;
Interface: admin down - line protocol down.
OSPF: линк r-edge -- isp-a не участвует в OSPF (это eBGP-линк наружу).
BGP: сессия с 203.0.113.2 (isp-a) сброшена после shutdown, состояние стало в Idle/Active. После восстановления линка сессия снова поднялась до Established за ~18 сек.
IP SLA и track ещё не были настроены

## какой маршрут был до и после;
до
best: 203.0.113.2 (65001 65000), localpref 200 (снова стал таким после восстановления линка)
alt: 198.51.100.2 (65002 65000), localpref 100
после
best: 198.51.100.2 (65002 65000), localpref 100

## сколько ответов ping потеряно;
Первый отрицательный пакет при ping seq=9, последний перед положительным пингом seq=171. 
seq=9 (начало потерь): 1789489190.832815 
seq=172 (первый успех): 1789489356.740640 
простой трафика = 356.740640 − 190.832815 = 165,908 секунд (≈ 2 мин 46 сек)
![[Pasted image 20260917150537.png]]

## какой traceroute получился после сходимости;
![[Pasted image 20260917142045.png]]

какое сообщение записал EEM

### r-edge#show ip bgp 192.0.2.100
BGP routing table entry for 192.0.2.100/32, version 16
Paths: (1 available, best #1, table default)
  Not advertised to any peer
  Refresh Epoch 2
  65002 65000
    198.51.100.2 from 198.51.100.2 (10.255.0.12)
      Origin IGP, localpref 100, valid, external, best
      rx pathid: 0, tx pathid: 0x0
### r-edge#show ip route 0.0.0.0
Routing entry for 0.0.0.0/0, supernet
  Known via "bgp 65010", distance 20, metric 0, candidate default path
  Tag 65002, type external
  Last update from 198.51.100.2 00:01:19 ago
  Routing Descriptor Blocks:
  * 198.51.100.2, from 198.51.100.2, 00:01:19 ago
      Route metric is 0, traffic share count is 1
      AS Hops 1
      Route tag 65002
      MPLS label: none


# Отказ линка isp-b — inet
## точное действие и время
isp-b(config)#interface e0/1 
isp-b(config-if)#shutdown

Track 20 State: Up → Down 21:58:03.345
EEM applet TRACK20-DOWN сработал 21:58:03.346
## что произошло с interface/OSPF/BGP/IP SLA/track;
Interface: физически затронут линк isp-b -- inet - first hop от r-edge (198.51.100.2) остался доступен, но путь до 192.0.2.200 пропал.
BGP: сессия r-edge - isp-b не рвалась, Established сохранялся весь тест, подтверждение, что "две Established-сессии не доказывают доступность конкретного хоста".
IP SLA 20: переходила из OK в Timeout - Latest operation return code: Timeout.
Track 20: переключился Up - Down через 10 сек задержки после того как SLA не сработало.
PBR: как только track ушёл в Down, set ip next-hop verify-availability 198.51.100.2 10 track 20 перестал быть активным, трафик от SRV1 перестал направляться на ISP-B - сработал fallback на обычный BGP best path.
## какой маршрут был до и после;
До: трафик SRV1 - 192.0.2.100 шёл через PBR на next-hop 198.51.100.2 (ISP-B), в обход обычного BGP best path.
После отказа: PBR перестал переопределять next-hop, трафик SRV1 пошёл по обычному BGP best path - через 203.0.113.2 (ISP-A).

## сколько ответов ping потеряно;
Переключение зафиксировано через traceroute, а не через непрерывный ping.

## какой traceroute получился после сходимости;
 После отказа маршрута isp-b трафик srv1-a пошел через isp-s.
![[Ping_SRV1-A_IP SLA.png]]

## какое сообщение записал EEM
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
