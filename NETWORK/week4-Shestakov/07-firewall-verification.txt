Файл подтверждает, что правила межсетевого экрана минимальны и каждое из них необходимо.
## Список правил
На FW созданы алиасы для удобства: с сетями офисов OFFICE_NETS, с сетями филиала BRANCH_NET, тестовый интернет TEST_HOST, список порты PORTS_SVC.

### Для LAN
pass, protocol: ICMP (echo-request), Source: OFFICE_NETS, Destination: TEST_HOST, BRANCH_NET 
pass, Protocol: TCP, Source Port: any, Destination Port: 80, 443, 22 (алиас PORTS_SVC), Source: OFFICE_NETS, Destination: BRANCH_NET.
правило Block для всего лишнего с включенным логированием

### Для WAN
Pass, Protocol: ICMP (destination unreachable) для PMTUD.
Pass, Protocol: ICMP (echo-request), Source: BRANCH_NET, Destination: OFFICE_NETS.
Pass, Protocol: TCP, Source: BRANCH_NET, Destination Port: 80, 443, 22, Destination: OFFICE_NETS.
правило Block для всего лишнего с включенным логированием

UDP 500, ESP нет ни через LAN, ни через WAN fw, потому что эти пакеты вообще не заходят на fw.

GRE инкапсулирован внутри ESP на r-edge ещё до выхода в isp, а fw вообще не видит трафик r-edge - isp. Отдельное правило для protocol 47 на fw не нужно.
![[Пример из журнала.png]]