## Какие LSDB хранит ABR и почему база не одинакова во всем многообластном домене?
ABR хранит отдельную LSDB для каждой area, к которой подключён, то есть у sw-core своя LSDB area 0 и своя LSDB area 10, они не сливаются в одну общую базу.

## Как r-branch узнает VLAN из area 0?
VLAN 10/20/99 объявлены на SVI sw-core в area 0. sw-core как ABR, при переходе в area 10 не пересылает исходные Router-LSA этих сетей, а генерирует Type-3 Summary LSA на каждую сеть (10.150.104, 10.150.105, 10.150.110) и передает их в area 10.

## Почему OSPF process ID может различаться у соседей, а Router ID должен быть уникальным?
Process ID, локальный идентификатор процесса внутри одного устройства. В сам протокол OSPF, в заголовок пакетов, номер процесса не попадает. Router ID, поле внутри OSPF-пакета, служит идентификатором устройства во всей области видимости LSDB

## Что происходит с внутренним default route, когда оба BGP default исчезли?
Если оба BGP default одновременно пропадут, у r-edge исчезает собственный 0.0.0.0/0 в RIB и перестаёт генерировать OSPF-default

## Почему мы не перераспределяем все BGP-маршруты в OSPF?
Потому что это переполнит память и CPU внутренних макшрутизаторов и коммутаторов

## Где в нашей схеме находится AS_PATH и где OSPF cost?
AS_PATH, атрибут BGP, существует на границе AS 65010 и наружу
OSPF cost, метрика, существует только внутри AS 65010

## Почему Local Preference управляет исходящим путем предприятия?
Local Preference> BGP-атрибут, который передаётся внутри одной AS, влияет на первый шаг алгоритма BGP best path.

## Почему prepend не гарантирует вход через ISP-A?
удлининяет путь, который видят чужие AS, делая маршрут через ISP-B менее привлекательным по одному из критериев локального best path алгоритма

## Чем BGP best path отличается от выбора маршрута между BGP и OSPF в общей RIB?
BGP best path, алгоритм _внутри самого протокола BGP_, сравнивает несколько BGP-путей. Выбор между BGP и OSPF для одного и того же префикса это отдельный, более простой механизм определения дистанции

## Почему две Established-сессии не доказывают доступность 192.0.2.100?
TCP-сессия на порту 179 поднята, стороны обменялись OPEN/KEEPALIVE, BGP таблицы синхронизированы. Cоседи между собой могут обмениваться маршрутной информацией, но это доказывает реальную сквозную доступность хоста.

## Что означает route-map deny в BGP и что она означает в PBR?
В BGP означает полное отбрасывание (фильтрацию) маршрута, а в PBR пропуск пакета мимо политик PBR для обычной маршрутизации.

## Почему IP SLA probe зафиксирована отдельным host route?
Чтобы гарантировать, что сама проверка идёт предсказуемым путём, независимо от того, что происходит в BGP таблице

## Что произойдет с существующей TCP-сессией после смены провайдера, если на edge используется NAT?
При переключении провайдера меняется NAT-транслируемый source-адрес исходящих пакетов. Существующая TCP-сессия разорвется, и передача данных по ней прекратится.

## В чем различие control plane и data plane в этой лаборатории?
Control plane, обмен служебной информацией о маршрутах.
Data plane, реальное прохождение пользовательского трафика.

## Какие проверки вы выполните перед изменением route-map на рабочем edge?
Сохранил текущие конфиги
Проверил существующие prefix-list/ACL
Проверил счетчики после применения
Заранее подготовил хосты для проверки 


### Ответьте письменно: почему route-map установлена на внутреннем интерфейсе, а не на интерфейсе ISP-B?
PBR обрабатывает пакет на входящем интерфейсе, до попадания в обычный процесс маршрутизации.
Если поставить policy на исходящий интерфейс к ISP-B? то пакет туда попадёт только после того как обычная маршрутизация уже выбрала для него путь, то есть решение о next-hop уже принято через BGP best path (ISP-A).


## Захват control plane
### OSPF
#### Hello;
Hello каждые ~10 сек с самого начала захвата      

#### Router ID обеих сторон;
Кадр 7: 10.255.0.2
Кадр 10: 10.255.0.3
### Area ID;
0.0.0.10

#### Hello/Dead Interval;
Hello Interval 10 секунд, Dead Interval 40 секунд.

#### protocol number 89;
OSPF протокол, IP protocol number 89, это видно на IP-уровне каждого кадра

#### DBD, LSR, LSU и LSAck во время синхронизации;
В кадрах 41–59 OSPF-соседство было установлено заново. Hello, DBD, LSR, LSU и LSAck. В кадре 47 r-edge отправляет LSR с запросом Router-LSA, в кадре 49 получает её в составе LSU. После обмена LSA и подтверждений LSAck соседство переходит в состояние Full. Вся процедура заняла около 6 секунд.

#### одну Router-LSA Type 1.
кадр 49, от r-branch

#### Объясните разницу между LSU и LSA по собственному захвату.
LSU (кадр 49), это фактическая передача содержимого LSA, целый пакет с полем Link State ID, Advertising Router, списком линков и метрик. 
LSA (кадр 54), это подтверждение получения, там тоже есть LSA-заголовок, но без содержимого линков, тела LSA внутри нет.


### BGP
#### TCP three-way handshake и destination port 179;
handshake находится в кадрах 14-16
Кадр 14 TCP SYN: 203.0.113.1:11058 - 203.0.113.2:179
Кадр 15 TCP SYN, ASK: 203.0.113.2:179 - 203.0.113.1:11058
Кадр 16 TCP ASK: 203.0.113.1:11058 - 203.0.113.2:179

#### OPEN с ASN;
кадр 17 - кадр 19

#### KEEPALIVE;
кадр 20: 203.0.113.2 - 203.0.113.1
кадр 22: 203.0.113.1 - 203.0.113.2
кадр 23: 203.0.113.1 - 203.0.113.2
кадр 25: 203.0.113.2 - 203.0.113.1
KEEPALIVE не содержит маршрутов. Он нужен для поддержание BGP-сессии и подтверждение того, что сосед остаётся доступным.

#### UPDATE с NLRI, AS_PATH и NEXT_HOP;
UPDATE кадр 24, 26
Кадр 24 содержит:
AS_PATH: 65010
NEXT_HOP: 203.0.113.1
NLRI: 10.150.104.0/21

Кадр 26 содержит:
AS_PATH: 65001 65000 
NEXT_HOP: 203.0.113.2
NLRI: 192.0.2.100/32, 192.0.2.200/32
Другой UPDATE содержит:
AS_PATH: 65001
NEXT_HOP: 203.0.113.2
NLRI: 0.0.0.0/0

#### withdrawal при отключении одного из путей, если он попал в capture.
В предоставленном захвате нет примера Withdrawal. При наличии такого UPDATE были бы указаны префиксы, которые необходимо удалить из BGP RIB.



## 6. IP SLA
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


