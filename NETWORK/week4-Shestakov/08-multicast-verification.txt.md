# sw-core
## sw-core#show ip igmp groups
IGMP Connected Group Membership
Group Address    Interface                Uptime    Expires   Last Reporter   Group Accounted
239.20.20.20     Vlan10                   00:28:11  00:02:20  10.150.104.11
224.0.1.40       Loopback0                01:54:17  00:02:46  10.255.0.1
sw-core#
*Sep 29 14:55:59.376: %SYS-5-CONFIG_I: Configured from console by console

## sw-core#show ip mroute
IP Multicast Routing Table
Flags: D - Dense, S - Sparse, B - Bidir Group, s - SSM Group, C - Connected,
       L - Local, P - Pruned, R - RP-bit set, F - Register flag,
       T - SPT-bit set, J - Join SPT, M - MSDP created entry, E - Extranet,
       X - Proxy Join Timer Running, A - Candidate for MSDP Advertisement,
       U - URD, I - Received Source Specific Host Report,
       Z - Multicast Tunnel, z - MDT-data group sender,
       Y - Joined MDT-data group, y - Sending to MDT-data group,
       G - Received BGP C-Mroute, g - Sent BGP C-Mroute,
       N - Received BGP Shared-Tree Prune, n - BGP C-Mroute suppressed,
       Q - Received BGP S-A Route, q - Sent BGP S-A Route,
       V - RD & Vector, v - Vector, p - PIM Joins on route,
       x - VxLAN group
Outgoing interface flags: H - Hardware switched, A - Assert winner, p - PIM Join
 Timers: Uptime/Expires
 Interface state: Interface, Next-Hop or VCD, State/Mode

(*, 239.20.20.20), 00:13:12/stopped, RP 10.255.0.1, flags: SJCF
  Incoming interface: Null, RPF nbr 0.0.0.0
  Outgoing interface list:
    Vlan10, Forward/Sparse, 00:13:12/00:02:45

(10.150.105.10, 239.20.20.20), 00:04:43/00:02:13, flags: FT
  Incoming interface: Vlan20, RPF nbr 0.0.0.0
  Outgoing interface list:
    Vlan10, Forward/Sparse, 00:04:43/00:02:45


## sw-core# show ip rpf 10.150.105.10
RPF information for ? (10.150.105.10)
  RPF interface: Vlan20
  RPF neighbor: ? (10.150.105.10) - directly connected
  RPF route/mask: 10.150.105.0/24
  RPF type: multicast (connected)
  Doing distance-preferred lookups across tables
  RPF topology: ipv4 multicast base


Control Plane отрабатывает: хост присылает IGMP Report на включение в группу, sw-core его слышит и шлет IGMP Query. Сервер SRV1 шлет мультикаст-поток в сторону sw-core. Но Data Plane (UDP-данные) не пересылается из ядра в сторону получателя.


## краткое объяснение различия
(*, G) Общее дерево, * означает любой источник, а G конкретная мультикаст-группа. (S, G), Дерево источника S конкретный IP-адрес источника (SRV1), G группа. Cтроится напрямую от сервера к получателям через RPF-интерфейс.

## IGMP Snooping

До отключения мультикаст-данные от сервера доходят до sw-core, уходят в коммутатор доступа sw-1-as. На sw-1-as включен snooping, поэтому по L2-таблице коммутации и пересылает UDP-пакеты только в порт получателя PC1.

После отключения Snooping мультикаст-трафик перестал обрабатываться на уровне L2. И сиал расцениваться как широковещательный трафик, коммутатор начинает флудить трафиком во все порты с vlan 10.

После повторного включения коммутатор снова активирует логику перехвата IGMP-пакетов.

## Список исходящих интерфейсов
До отключения получателя PC1, он шлет репорты, Vlan10 находится в состоянии Forward
(10.150.105.10, 239.20.20.20), 00:04:43/00:02:13, flags: FT
  Incoming interface: Vlan20, RPF nbr 0.0.0.0
  Outgoing interface list:
    Vlan10, Forward/Sparse, 00:04:43/00:02:45
    
После отключения получателя PC1. Маршрутизатор фиксирует, что в Vlan10 больше не осталось заинтересованных слушателей данной группы. Интерфейс Vlan10 исключается из списка OIL. запись переходит в состояние Pruned с флагом P.
