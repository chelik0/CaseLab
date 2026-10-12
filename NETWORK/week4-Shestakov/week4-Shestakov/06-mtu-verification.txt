Файл посвящен MTU, фрагментации и TCP MSS

## расчет накладных расходов GRE over IPsec для вашей конфигурации, расчет доступного размера IP-пакета внутри туннеля, результат до настройки ip mtu и ip tcp adjust-mss;
66 байт постоянной части, максимум внутреннего IP-пакета 1434. От MTU 1500 остаётся 1434. 1406 даёт ровно 1434, 1472 даёт 1500.
Проверка
![[MTU_ping.png]]


## значения ip mtu и ip tcp adjust-mss, которые вы выбрали; повторная проверка после настройки;
interface Tunnel0 
	ip mtu 1400 
	ip tcp adjust-mss 1360
Повторная проверка
![[MTU_ping2.png]]

## проверка ping с запретом фрагментации и подбором размера пакета
1372 IP-пакет 1400, он проходит. 1373 даёт 1401, не проходит.

Тест с передачей большого файла прошел успешно:
![[Передача файла1.png]]
так же на r-edge счётчики выросли

r-edge#show crypto ipsec sa | include pkts
    #pkts encaps: 2127, #pkts encrypt: 2127, #pkts digest: 2127
    #pkts decaps: 4826, #pkts decrypt: 4826, #pkts verify: 4826
    #pkts compressed: 0, #pkts decompressed: 0
    #pkts not compressed: 0, #pkts compr. failed: 0
    #pkts not decompressed: 0, #pkts decompress failed: 0
r-edge#show crypto ipsec sa | include pkts
    #pkts encaps: 3120, #pkts encrypt: 3120, #pkts digest: 3120
    #pkts decaps: 8740, #pkts decrypt: 8740, #pkts verify: 8740
    #pkts compressed: 0, #pkts decompressed: 0
    #pkts not compressed: 0, #pkts compr. failed: 0
    #pkts not decompressed: 0, #pkts decompress failed: 0


## результат опыта с временным запретом ICMP unreachable на МСЭ; краткое объяснение, почему передача крупных данных может зависать при блокировке необходимых ICMP-сообщений.
Блокировка на МСЭ ICMP unreachable
соединение установится (SYN/ACK), но данные не идут, крупные сегменты будут отбрасываются на границе 1434, ICMP-ошибка о превышении MTU не доходит до отправителя.