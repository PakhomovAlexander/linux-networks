 # Администрирование информационных систем

 ## Вводная леция 

 - Будет 4 лабы
 - Debian \ CentOS 

 ### Про предмет

 - сети и их устройство
 - сетевые протоколы
 - сервера

 ### История развития 

 - 1950-е Системы пакетной обработки 
 - многотерминальные системы разделения времени, построены на телефонных сетях (были отработаны: маршрутизация пакетов, коммутация пакетов и многоуровневое построение протоколов)
 - сеть X.25 (начало 70-х)
 - в 1969 создана сеть ARPANET, эта сеть явлеяется прообразом INTERNET
 - в конце 60-х был переход на цифрвой сигнал
 - в начале 70-х стали появляться большие интегральне схемы на полупроводниках и первые миникомпьютеры, появилась концепция распределения ресурсов на предприятии
 - в середине 80-х появились ПК и утвердились стандартные технолонии объединения в сеть( Ethernet, TokenRing, TonenBus, FDDI, ArcNet)

 ### Термины

1) Направленность связи
    - Simplex (однонаправленная)
    - Full-Duplex (связь идет в 2 стороны одновременно)
    - Half-duplex (в обе, но по очереди)

2) Топология сети
    - Полносвязная (все со всеми)
    - Ячеестая (полносвязная минус несколько ребер)
    - Кольцевая
    - Звезда
    - Дерево
    - Общая шина

3) Адресация узлов
    - Unicast (идет одному конкретному адресу)
    - Multicast (груповой адрес - конференции)
    - Broadcast (широковещательный адрес)
    - Anycast (адрес произвольной рассылки, идет как multycast, но получает сообщение только первый)
    - Geocast (МЧС сообщения)

### Уровни сетевых протоколов

**OSI - Open System Interconnection**, общая модель, которой пользуются все люди, которые пользуются сетями. Имеет 7 уровней: 

1) *Физический (Physical layer)*. Передача потоков битов по физическим каналам связи. Протокол физического уровня 
определяет физические параметр среды передачи. Например: среда, напряжение, уровень поля. 10Base-T Ethernet, IRDA, RS-232, DSL, 802.11 WiFi.
2) *Канальный (Data link layer)*. Уровень протокола, который работает в режиме комутации пакетов. Единица передачи информации - кадр. Обнаруживает и корректирует ошибки. 
Это сетевая карта. Тут находится mac адрес. Пример: Ethernet, FDDI, Frame Relay, IEEE 802.11 wlan, PPP, PPPoE. Все проблемы канального уровня вероятнее всего это проблемы сетевой карты. 
3) *Сетевой (Network layer)*. Это протоколы, которые служат для образования единой транспортной системы, объединяющие несколько сетей. На этом уровне происходит маршрутизация пакетов.
IP(TCP/IP), стэк протоколов - набор, который обеспечивает уровни взаимодействия компьютеров. IPX (Novell IPX/SPX), X.25, IPSeq(Cisco), RIP, OSPF.
4) *Транcпортный (Transport layer)*. Обеспечивает доставку пакетов с необходимой степенью надежности. TCP, UDP, SCTP, ATP, ICMP. 
5) *Сеансовый уровень (Session layer)*. Поддержка сторон и управление сеансами. Прикладной уровень. PPTP, L2TP, NetBios.
6) *Уровень представления (Presentation layer)*. Изменяет представление информации не меняя сути. Нужны для шифрования. SSL, AFP, ICA, LPP, XDR.
7) *Прикладной (Application layer)*. Набор протоколов, предоставляющих доступ к разделяемым ресурсам. Сетевые сервисы. HTTTP, FTP, SMTP, RDP, SSH.

TCP\IP - включает последние 3 уровня в прикладной уровань. 

Wireshark - анализатор пакетов.

### Протокол ARP (Adress resolution protocol) 

Позволяет получить макадрес через ip. 
1) Отсылается широковещательный (локальная сеть) ARP запрос со следующим запросом: "У кого адрес 192.168.0.2?, ответьте на MAC: 01:...". 
2) Компьютеры в локальной сети видят друг друга в канальной сети, и комп, который имеет нужный ip отправляет ответ. 
3) Посылающий принимает ответ и сохраняет в arp cache (arp -a).

### Протокол IP(IPv4)

Это адресация компьютеров. Пример адреса (192.168.39.11 - ХХ.ХХ.ХХ.ХХ)

Маска подсети - битовая маска, определяющая какая часть адреса относится к адресу сети, а какая к адресу хоста. Она записывается количеством битов (/24) или 255.255.255.0. 

Пример: 192.168.0.0/24 - ip сети. Его битовое представление: 11000000.101010000.00000000.00000000, 
маска выглядит так: 1111111111.1111111111.1111111111.0000000000 (255.255.255.0). Сделав последние 0 -> 1, получаем широковещательный алрес сети. 

Другой пример: допустим у нас есть сеть 192.168.1.128/25. 
```
(1100000000.1010101000.0000001.10000000) (192.168.1.128)
(1100000000.1010101000.0000001.10000000) (255.255.255.128)
```

Для испольщования в частных локальных етях есть 3 диапазона:

1) 10.0.0.0.0/8
2) 172.16.0.0/12
3) 192.168.0.0/16

127.0.0.0/8 - комуникации внутри хоста.

169.254.0.0/16 link-lical (zeroconf) втоконфигурации ip адреса без DHCP. 

192.88.99.0/24 - туннелирования IPv6 внутри IPv4 

224.0.0.0/4 - multycast messages 

``` bash
ifconfig - устаревшая утилита 
ip (iproute2)

ip address add 192.168.1.1 dev eth0
ip address del 192.168.1.1 dev eth0
ip address show = ip as
ip address flush = ip af
```
 
ip - временная настройка, работае до перезагрузки

### Debian 

Есть файл /etc/network/interfaces 

У нас есть сетевые картыЮ например, eth0, eth1 

``` bash
auto eth0
iface etg0 inet dhcp
auto eth1
iface eth1 inet static
    address 192.168.1.1
    netmask 255.255.255.0
    gateway 192.168.1.100
```

### CentOS
``` bash
/etc/sysconfig/network-scripts/ifcfg-eth0

DEVICE = eth0
ONBOOT = yes 
BOOTPROTO = dhcp
HWADDR = 12:34:56:78:90:AB
```

``` bash
/etc/sysconfig/network-scripts/ifcfg-eth1

DEVICE = eth0
ONBOOT = yes 
BOOTPROTO = dhcp
HWADDR = 12:34:56:78:90:AB
NETMASK = 192.168.1.1 
GATEWAY = 255.255.255.0
HWADDR = ...
```

## IPv6

2001:0db8:113a:09d7:1f34:8a2a:07a0:7566

http://[2001::7566]/

http://[2001::7566]:8080/

1) Он поддерживает пакеты до 4-х гигабайт
2) метки потоков 
3) классы трафиков

## Пакет 

Единица передачи информации. 

ip-packet содержит следующие поля:

1) Версия протокола
2) Размер заголовка
3) ToS - Type of service 
4) Размер пакета 
5) Идентификатор пакета 
6) Флаги 
7) TTL - time to live - количество возможностей маршрутизации
8) Протокол внутр
9) checksum 
10) source address and destination address

## Routing

Маршрутизация - процесс определения маршрута следования информации в сетях связи

1) Маршрутизация в компьютерных сетях задается таблицами, которые представляют из себя список адресов сетей (узлов), маршрутизатор для них и метрик маршрутов. 
2) Если хост локально доступен, используется arp
3) Если хост локально недоступен, тогда шлем маршрутизатор.
4) Маршрут выбираетя с наименьшей метрикой
5) Маршруты могут быть статическими или динамическими
6) Обычно пауеты проходят через несколько маршрутизаторов, прежде чем достигают цели, и на каждом из них есть свои правила и таблицы

win - riute print

linux

```0
ip route list  == ip rl
ip route add 
ip route del 
ip route flush
```

### Пример сети

A(192.168.0.1 Eth0) - (192.168.0.2 Eth1) B (192.168.1.2 Eth0) - (192.168.1.1 Eth0)C (192.168.2.1 Eth1) - (192.168.2.2 Eth0) D

Что бы пакеты спокойно ходили от A до D нужно: 

- A ip r a default via 192.168.0.2 
- B ip r a 192.168.2.0 via 192.168.1.1 dev eth0 
- C ip r a 192.168.0.0 via 192.168.1.2 dev eth0
- D ip r a default via 192.168.2.1


### MTU (Maximum transfer unit) 

Максимальнай размер пакета, который может быть передан посредством протокола

| Protocol | bytes  |
|:---------|:------:|
| ipv4     | > 576  |
| ipv6     | > 1280 | 
| Ethernet | 1500   |
| Wifi     |  7981  |

Фрагментация пакета: Если размера не хватает для данных (на сетевом уровне) - пакет разбивается на несколько ip пакетов, каждый из них получает одинаковый идентиыикатор. -!Все, ктоме последнего, получают флаг MF(More Fragments). Хост получателя производит сборку.

### Path MTU discovery

Метод для определения пропускного MTU цепочки маршрутизаторов.

#### Как это делается? 

1) Посылается большой DF (Don't fragment) пакет 
2) Если не проходит, то в ответ на этот пакет придет сообщение ICMP: Fragmentation needed, MTU
3) Снижаем до указанного, goto п.1

## ICMP (Internet Control Messages Protocol)

Нужен для служебных нужд, инкапсулируется в IP. 

| 0-7 | 8-15 | 16-31 | 32 -63               |   ?          | 8                            |
|-----|------|----------|-------------------|--------------|----------------------------|
| тип | код  | Checksum | Остаток заголовка | Заголовок IP | первые 8 байт IP сообщения |


Тип                 |   Код  |  Пояснение                         |
--------------------|--------|------------------------------------|
8                   |    0   |   Echo requst                      |
0                   |    0   |   Echo reply                       |           
30                  |    0   |   Tranceroute                      |
12 - bad ip header  | 0      | Pointer indicates error            |
                    | 1      | Missing required option            |
                    | 2      | Bad length                         |
3                   | 0      | Destination network unreachable    |
                    | 1      | Destination host inreachable       |
                    | 3      | Destination port unreachable       |
                    | 4      | Fragmentation needed               |
11 Time Exceeded    | 0      | TTL expired                        |
                    | 1      | Fragment reassembly time expered   |

### IPSec 

Набор протоколов для обеспечения защиты передаваемых данных по IP

### IGMP

Протокол для групповой передачи данных, похволяет эффективно использовать ресурсы для потоковых данных и онлайн игр.

### RARP (Revers ARP)

Нужен для присвоения IP адреса при загрузке машины

### RIP

Для маршрутизации, служит для обновления информации о маршрутах. Принимается в малых сетях.

### OSPF, IGRP (Cisco)

Протоколы для маршрутизации 

## UDP (user datagamm protocol)

Протокол транспортного уровня. Самый не надёжный протокол, можно отсылать пакеты без предварительного согласования. е 

1) Не гарантирует передачу данных, то есть часть пакетов могут не придти
2) Не гарантирует порядок пакетов
3) Лёгкий и быстрый

### Структура заголовка UDP 

| 0-1        | 2-3               | 4-5    | 6-7      |
|------------|-------------------|--------|----------|
|source port | destination port  | length | checksum |

port = (0, 65535). 0-1023 - служебный порт (http 80, ftp 21), 1024-49151 - прочие службы, 49152+ используются как угодно. 

### Схема использования сокетов в сетевом взаимодействии

Рисунок 

## TCP (Transmission Control Protocol)

Протокол транспортного уровня с гарантированной доставкой и уведомлением отправителя


| биты               | поле              | пояснение    
|--------------------|-------------------|--------|----------|--------------|----------------------------|
|16       | source port            | |
|16       | dest port              | |
|32       | sequence number        | |
|32       | ack number             | |
|4        | data offset            |  (5 -15)*32|
|3        | reserved               | |
|9        | flugs                  | NS, CWR, ECE, URG - указатель важности, ACK-указывает на то, что ACK number значимый, PSH - передать данные принимающему приложению(закончить накопление) RST - reset, SYN - synchronized number, FIN - finish|
|16       | window size            | |
|16       | checksum               | |
|16       | urgernt pointer        | |
|0-320bit | options (32 выровнены) | |
|         | pudding                | |

SCTP, DCCP, RUDP, RTP, GRE

DHCP - автоматическая настройва ip адреса и прочих параметров сети. Для работы он использует протокол UDP. Клиент испольщует порт 68, сервер 67.

1) Динамический, выдается на время, из указанного на сервере диапазона.
2) Автоматический адрес - то же самое что диманический, но выдается навсегда.
3) Статический. В конфигах сервера жество привязывается mac адрес к ip.

Через DHCP может выдаваться шлюз по умолчанию, маска подсети, адреса DNS серверов, имя домена DNS, маршруты и тд.

/etc/dhcp/dhcpconf 
	default-lease-time
	max-lease-time
	ping-check
	log-facility

    subnet 192.168.0.0 netmask 255.255.255.o {
        rage 192.168.0.100 192.168.0.254
        broadcast0address 192.168.0.255
        domain-name example.org
        domain-name-servers 192.168.0.5, 8.8.8.8 
        option routers 192.168.0.1
    }

    host somehost1 {
        option host-name "somehost1-example.org"
        hardware-erhernet 12:34:56:78:90:AB
        fixed-address 192.168.0.20
    }

## IPTABLES

В Linux есть netfilter (firewall), доступен с ядра 2.4. 

iptables утилита, кторая работает с firewall 

### Принцип работы

1) Пакеты пропускаются через цепочки
2) Цепочки - упорядоченный набор правил
3) Правило содержит действия, критерий, переход в другую цепочку, счетчик пакетов и т.д
4) Цепочки объединяются в таблицы 

### Цепочки

1) PREROUTING - начальная обработка входящих пакетов перед принятием решений о маршртизации
2) INPUT - входящие пакеты, адресованные локальному процессу
3) FORWARD - входящие пакеты адресованные другим хостам 
4) UOTPUT - пакеты, сгенерированные локальными процессами
5) POSTROUTING - окончательная обработка исходящих пакетов

### Пакет может находиться в одном из 5 состояний 

1) NEW - открытие нового сеанса
2) ESTABLESHED - асть открытого сеанса
3) RELATED - новый сеанс связанный с уже открытым
4) UNTRACKED - необрабатывается
5) INVALID - все остальные

conntrack - система определения состояния пакета

### Tables

#### Row 

Просматривается до передачи conntrack. Используется для маркировки пакетов, которые не надо отдавать conntrack, для блакировки нежелательных пакетов, что бы не тратить на них время conntrack.

Действия, которые можно указывать в этой таблице: 
- notrack

### mandle 

Правила модификации пакетов (обычно заголовки)

Действия, которые можно указывать в этой таблице: 
- TTL
- TOS (type of service)
- MARK

### nat

Таблица просматривает пакеты, создающие новые подключения, согластно conntrack. Используется для адресации? 

Действия, которые можно указывать в этой таблице: 
- SNAT
- DNAT
- MASQUERATE (dynamic SNAT)
- REDIRECT 

filter: accept, drop, reject - действия с пакетами.

*тут должна быть фотка сложной схемы*

### Примеры использования iptables

```bash 
iptables [-t table] -A chain rule 
iptables [-t table] -D chain rule 
iptables [-t table] -C chain  
iptables [-t table] -F chain 
iptables [-t table] -P policy  
iptables [!] -p tcp|udp|udplite|icmp|ctp|..|all
iptables [!] -s address [\mask] [,...]
iptables [!] -d
iptables (-i|o) name
iptables -j TARGET 
-m module
-g chain
```

1) Запрет всех входящих, но исходящие работают
```bash 
iptables -p INPUT -j DROP
iptables -A INPUT -m state ESTABLISHED, RELATED -j ACCEPT
```
2) Настройка маршрутизатора
```bash 
iptables -p FORWARD -j DROP
iptables  -A INPUT -m state ESTABLISHED, RELATED -j ACCEPT
iptables -A FORWARD -i eth0 -p icmp -j ACCEPT
iptables -A FORWARD -i eth0 -p tcp --dport 80 -j ACCEPT
```
3) SNAT
```bash 
iptables -A FORWARD -s 192.168.0.0/24 -o eth1 -j ACCEPT
iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -o eth1 -j MASQUERADE
```

4) DNAT
```bash 
iptables -t nat -A PREROUTING -i eth1 -p tcp --dport 5555 -j DNAT --to-destination 192.168.0.5:8080
```

Что бы у нас работал FORWARDING нужно сделать шаманство echo 1>/proc/sys/net/ipv4/ip_forfard

## DNS 

Иерархическая распределённая система иеминования компьютеров, сервисов и других ресурсов, подключенных к сети интернет.

1) Распредедённость администрирования (ответственность на разных людях)
2) Распределённость хранения информации (каждый хранит свою хону и корневые сервера)
3) Кеширование информации (узел может хранить данные не из своей сети для уменьшения нагрузки на сеть)
4) Иерархическая структура (делегирование подзон нижележащим узлам)
5) Резервирование (за хранение и обслуживание файловых зон отвечает несколько серверов)

### Основные понятия

1) Протстанство имён доменов (domain namespase) - это дерево доменных имен, верхнее из которых "." (дерево содержит 0 или более ресурсных записей)
2) Domain - узел имен 
3) Subdomain
4) Zone - ветвь общего дерева имен, может иметь общие зоны. Размещается как единое целое на сервере имён, используется для делегирования ответственности за часть пространства имён разным организациям.
5) Resource record - удиница хранения и передачи информации в dns
6) Делегирование - передача ответственнности за чать дерева, осуществляемая с помощью выделения части дерева в отдельную зону. 
7) Авторитетность сервера (Authritative server ) - google.com

### Синтаксис имён доменов

1) Имя домена состоит из нескольких меток, разделённых точками.
2) Порядок в иерархии справа налево
3) Одна метка содержит до 63 символов
4) Общая длинна не превышает 254 символа
5) До 127 уровней вложенности

IDN - Internationalized domain names, домены с нелатинскими именами, например м кирилическими. Кирилица преобразуется в специальную кодировку Punicode

Ресурсная запись состоит из нескольких полей. 

1) Name
2) TTL (time to live) - допустимое время зранение в кеше сервера
3) Класс сетей IN 
4) RDLEN - длинна поля данных 
5) RDATA - поле данных 
6) Тип записи
    - SOA - начальная запись зоны, включает в себя (на каком сервере находится эталнная информация о домене, контакты лица, ответственного за зону и тайминги кеширования инормации в этой зоне)
    - A - связыывает имя с ipv4 адресом 
    - AAA - связывает имя с ipv6 адресом
    - CNAME - connonical name
    - PTR запись обратного разрешения по p доменного имени
    - NS - nameserver я домена
    - MX СФТП сервер, отвечающий за почту для доменов
    - SRV - серверы для определённых сервисов в домене, например active directory (AD)


UDP, port, s3

TCP, s3 (длинный ответ, транфер зон) 

#### Процесс разрешения имени

1) Любой DNS сервер содержит информацию о корневых серверах.
2) Ответы кешируются.
3) Бывают 2 варианта запросов на разрешение имен - рекурсивный и не рекурсивный. В случае с рекурсивным запросом криент получает ответ у сервера, у которого запросил. Сервер сам сделает нужный запрс. В случае нерекурсивного клиент будет перенаправлен к следующему серверу.
4) Для обратного разрешения адресов используют
 in-addr.arpa, ip6.arpa. 192.168.1.2->2.1.168.192.in-adde.arpa

 ## 13 способов скопировать файл по сети в linux

 1) Смонтировать по сети. Могут быть 2 протокола (NFS, CFS(SmbFS))
 2) ftp
 3) sftp
 4) scp (scp a.txt root@192.168.0.1:/root)
 5) mc + shelllink (fish)
 6) Somba (mc, smbclient)
 7) Rsync  
 8) Google, etc
 9) Email, facebook
 10) netcat (nc -l -p 8080 < tar xzf -) tar cz file.txt | nc 192.168.0.1 8080
 11) Apache + wget\curl
 12) github
 13) torrent
 14) DC++

 ## NTP (Network time protocol)

0) на level0 расположены точные часы (атомный эталон времени, gps, глонасс).
1) Рядом с ними стоят машины, соединенные по протоколу rc-232 (com port) - level1.
2) level2... до 256.

ntpd

## FTP (file transpport protocol)
 
 Работает поверх TCP

 ftp://[user[:pass]@]host[:port]/path

 Испольщуется для 

 1) скачивания дистрибутивов
 2) заливка сайтов на хостинги
 3) широко используется людьми связанными с ИТ

 его особенности:

 1) основан на сессиях
 2) встроенная аутентификация
 3) поддерживает текстовый и двоичный формат передачи
 4) не поддерживает типы передаваемых данных 
 5) поддерживает операции над файловыми системами
 6) использует двойное подключение

 Управление символами 

 1) ASCII На клиента данные кодируются в 80-битный ASCI и потом в символьное представление принимающей стороны
 2) Binary - рекомендуется к испольщованию
 3) EBCDIC - передача с перекодировкой в "вот это вот", IB шняга
 4) Local Mode текстовый режим без перекодировки

1) FTPS
2) SFTP - надстройка над ssh 
3) FTP over ssh

TFTP - trivial ftp не имеет аутентификации и основан на udp, его используют для

1) загрузки бездиковых станций
2) получение логов с мини-атс и сетевых устройств.
3) заливка прошивок

## HTTTP

сообщение http состроит из 

1) СТартовая строка 
2) заголовок (Header)
3) Тело (Body)

1) ```
HTTP 0.9
GET <URL>
HTTP 1.0.1.1
<METHOD> URL HTTP/ <>

GET . HTTP/1.1
GET /login HTTP/1.1
```

- * GET - иденпотентен * - повторный запрос выведет одно и то же!!!
- HEAD - аналогичен GET о без тела сообщения. используется для получения метаданных, валидации и что бы узнать не изменился ли ресурс
- POST - передача данных пользователя ресурса, не иденпотентен. Применяется для отправки файлов и реализуется сервером.
- PUT, PATCH, DELETE, TRACE, LINK, UNLINK, CONNECT, OPTIONS - их использование редко

Веб сервер - сервер, который поддерживает GET и HEAD

Коды:

- 1хх - information
- 2xx - success
- 3xx - redirect
- 4xx - client error
- 5xx - server error

- 200 - ok
- 400 - bad reauest
- 403 - forbidden
- 404 - not found
- 500 - internal server error
- 501 - not implemented
- 418 - I am a teapot

headers: 

- ```
GET /   HTTP
Host: google.com
Accept: text/plain
Referer: google.ru
User-Agent: Mozila/5.0(x11, Linux x86)


Content-Type: image/png
Cookie: sid=....

```