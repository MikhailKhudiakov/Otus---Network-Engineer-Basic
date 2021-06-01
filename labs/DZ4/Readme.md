### Домашнее задание №4 
### Лабораторная работа. Настройка IPv6 адресов на сетевых устройствах. 
#### Топология.
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ4/%D1%82%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F%20%D0%B4%D0%B74.bmp)
#### Таблица адресации
Устройство |интерфейс| IPv6-адрес|длина префикса|шлюз по умолчанию|
---|---|---|---|---
R1|G0/0/0|2001:db8:acad: a::1 |64|-
R1|G0/0/1|2001:db8:acad:1::1 |64|-
S1|VLAN1|2001:db8:acad:1::b|64|-
PC-A|NIC|2001:db8:acad:1::3|64|fe80::1
PC-B|NIC|2001:db8:acad: a::3|64|fe80::1
### Задачи
#### Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора
#### Часть 2. Ручная настройка IPv6-адресов
#### Часть 3. Проверка сквозного соединения
### Часть 1. Создание и настройка сети.
#### Шаг 1.Настройте маршрутизатор.
```
Router>enable
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname R1
R1(config)#service password-encryption
R1(config)#enable secret class
```
#### Шаг 2. Настройте коммутатор.
```
Switch(config)#no ip domain lookup
Switch(config)#hostname S1
S1(config)#service password-encryption
S1(config)# enable secret class
```
### Часть 2. Ручная настройка IPv6-адресов
#### Шаг 1. Назначьте IPv6-адреса интерфейсам Ethernet на R1.
a.	Назначьте глобальные индивидуальные IPv6-адреса, указанные в таблице адресации обоим интерфейсам Ethernet на R1.
```
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#int g0/0/1
R1(config-if)#ipv6 address 2001:db8:acad:1::1/64
R1(config-if)#description Link to S1
R1(config-if)#no shutdown

R1(config)#int g0/0/0
R1(config-if)#ipv6 address 2001:db8:acad:a::1/64
R1(config-if)#description Link to PC-B
R1(config-if)#no shutdown
```
b.	Введите команду show ipv6 interface brief, чтобы проверить, назначен ли каждому интерфейсу корректный индивидуальный IPv6-адрес.
```
R1#show ipv6 int brief
GigabitEthernet0/0/0       [up/up]
    FE80::260:2FFF:FEBE:1901
    2001:DB8:ACAD:A::1
GigabitEthernet0/0/1       [up/up]
    FE80::260:2FFF:FEBE:1902
    2001:DB8:ACAD:1::1
GigabitEthernet0/0/2       [administratively down/down]
    unassigned
Vlan1                      [administratively down/down]
    unassigned
```
c.	Чтобы обеспечить соответствие локальных адресов канала индивидуальному адресу, вручную введите локальные адреса канала на каждом интерфейсе Ethernet на R1.
```
R1(config)#int g0/0/0
R1(config-if)#ipv6 address fe80::1 link-local
R1(config-if)#exit
R1(config)#int g0/0/1
R1(config-if)#ipv6 address fe80::1 link-local
R1(config-if)#exit
```
d.	Используйте выбранную команду, чтобы убедиться, что локальный адрес связи изменен на fe80::1
```
R1#show ipv6 int brief
GigabitEthernet0/0/0       [up/up]
    FE80::1
    2001:DB8:ACAD:A::1
GigabitEthernet0/0/1       [up/up]
    FE80::1
    2001:DB8:ACAD:1::1
GigabitEthernet0/0/2       [administratively down/down]
    unassigned
Vlan1                      [administratively down/down]
    unassigned
```
Какие группы многоадресной рассылки назначены интерфейсу G0/0?
```
R1#show ipv6 interface
GigabitEthernet0/0/0 is up, line protocol is up
  IPv6 is enabled, link-local address is FE80::1
  No Virtual link-local address(es):
  Global unicast address(es):
    2001:DB8:ACAD:A::1, subnet is 2001:DB8:ACAD:A::/64
  Joined group address(es):
    FF02::1
    FF02::1:FF00:1
```
```
Интерфейсу G0/0/0 назаначены два мультикастовых адреса для групп многоадресной рассылки: 
FF02::1 группа многоадресной рассылки всех узлов IPv6 в локальной сети или канале,
а также FF02::1:FF00:1 групповой адрес запрашиваемого узла для многоадресной рассылки на запрашиваемые узлы.
```
#### Шаг 2. Активируйте IPv6-маршрутизацию на R1.
a.	В командной строке на PC-B введите команду ipconfig, чтобы получить данные IPv6-адреса, назначенного интерфейсу ПК.
```
C:\>ipconfig
FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Link-local IPv6 Address.........: FE80::2D0:BAFF:FE65:588B
   IPv6 Address....................: ::
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: ::
                                     0.0.0.0
```
Вопрос:Назначен ли индивидуальный IPv6-адрес сетевой интерфейсной карте (NIC) на PC-B?
```
Адрес не назначен.
```
b.	Активируйте IPv6-маршрутизацию на R1 с помощью команды IPv6 unicast-routing.
```
R1(config)#ipv6 unicast-routing

R1#show ipv6 int
GigabitEthernet0/0/0 is up, line protocol is up
  IPv6 is enabled, link-local address is FE80::1
  No Virtual link-local address(es):
  Global unicast address(es):
    2001:DB8:ACAD:A::1, subnet is 2001:DB8:ACAD:A::/64
  Joined group address(es):
    FF02::1
    FF02::2
    FF02::1:FF00:1
```    
c.	Теперь, когда R1 входит в группу многоадресной рассылки всех маршрутизаторов, еще раз введите команду ipconfig на PC-B. Проверьте данные IPv6-адреса.
```
C:\>ipconfig

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Link-local IPv6 Address.........: FE80::2D0:BAFF:FE65:588B
   IPv6 Address....................: 2001:DB8:ACAD:A:2D0:BAFF:FE65:588B
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: FE80::1
                                     0.0.0.0
```
Вопрос:
Почему PC-B получил глобальный префикс маршрутизации и идентификатор подсети, которые вы настроили на R1?
```
РС-B получил свой IPv6 адрес по методу SLAAC на основе RA сообщения от R1, при этом 
префикс (глобальный префикс+идентификатор подсети) используется из сообщения RA, идентификатор интерфейса генерируется в PC-B. 
```
#### Шаг 3. Назначьте IPv6-адреса интерфейсу управления (SVI) на S1.
a.	Назначьте адрес IPv6 для S1. Также назначьте этому интерфейсу локальный адрес канала.
```
S1#conf t
S1(config)#int vlan1
S1(config-if)#ipv6 address 2001:db8:acad:1::b/64
S1(config-if)#no shutdown
S1(config-if)#end
```
b.	Проверьте правильность назначения IPv6-адресов интерфейсу управления с помощью команды show ipv6 interface vlan1.
```
S1#show ipv6 int vlan 1
Vlan1 is up, line protocol is up
  IPv6 is enabled, link-local address is FE80::20A:F3FF:FE96:347
  No Virtual link-local address(es):
  Global unicast address(es):
    2001:DB8:ACAD:1::B, subnet is 2001:DB8:ACAD:1::/64
  Joined group address(es):
    FF02::1
    FF02::1:FF00:B
    FF02::1:FF96:347
```
#### Шаг 4. Назначьте компьютерам статические IPv6-адреса.
a.	Откройте окно Свойства Ethernet для каждого ПК и назначьте адресацию IPv6.
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ4/%D0%A72%20%D1%88%D0%B0%D0%B3%204%D0%B0-2.bmp)
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ4/%D0%A72%20%D1%88%D0%B0%D0%B3%204%D0%B0.bmp)

b.	Убедитесь, что оба компьютера имеют правильную информацию адреса IPv6. Каждый компьютер должен иметь два глобальных адреса IPv6: один статический и один SLACC
```
C:\>ipconfig

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Link-local IPv6 Address.........: FE80::2D0:BAFF:FE65:588B
   IPv6 Address....................: 2001:DB8:ACAD:A::3
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: FE80::1
                                     0.0.0.0
C:\>ipconfig

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Link-local IPv6 Address.........: FE80::201:96FF:FEE2:71C6
   IPv6 Address....................: 2001:DB8:ACAD:1::3
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: FE80::1
                                     0.0.0.0
```
### Часть 3. Проверка сквозного подключения
С PC-A отправьте эхо-запрос на FE80::1. Это локальный адрес канала, назначенный G0/1 на R1.
```
C:\>ping FE80::1

Pinging FE80::1 with 32 bytes of data:

Reply from FE80::1: bytes=32 time<1ms TTL=255
Reply from FE80::1: bytes=32 time<1ms TTL=255
Reply from FE80::1: bytes=32 time<1ms TTL=255
Reply from FE80::1: bytes=32 time<1ms TTL=255

Ping statistics for FE80::1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
Отправьте эхо-запрос на интерфейс управления S1 с PC-A.
```
C:\>ping 2001:db8:acad:1::b

Pinging 2001:db8:acad:1::b with 32 bytes of data:

Reply from 2001:DB8:ACAD:1::B: bytes=32 time<1ms TTL=255
Reply from 2001:DB8:ACAD:1::B: bytes=32 time<1ms TTL=255
Reply from 2001:DB8:ACAD:1::B: bytes=32 time<1ms TTL=255
Reply from 2001:DB8:ACAD:1::B: bytes=32 time<1ms TTL=255

Ping statistics for 2001:DB8:ACAD:1::B:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
Введите команду tracert на PC-A, чтобы проверить наличие сквозного подключения к PC-B.
```
C:\>tracert 2001:db8:acad:a::3

Tracing route to 2001:db8:acad:a::3 over a maximum of 30 hops: 

  1   0 ms      0 ms      0 ms      2001:DB8:ACAD:1::1
  2   0 ms      1 ms      1 ms      2001:DB8:ACAD:A::3

Trace complete.
```
С PC-B отправьте эхо-запрос на PC-A.
```
C:\>ping 2001:DB8:ACAD:1::1

Pinging 2001:DB8:ACAD:1::1 with 32 bytes of data:

Reply from 2001:DB8:ACAD:1::1: bytes=32 time<1ms TTL=255
Reply from 2001:DB8:ACAD:1::1: bytes=32 time<1ms TTL=255
Reply from 2001:DB8:ACAD:1::1: bytes=32 time<1ms TTL=255
Reply from 2001:DB8:ACAD:1::1: bytes=32 time<1ms TTL=255

Ping statistics for 2001:DB8:ACAD:1::1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
С PC-B отправьте эхо-запрос на локальный адрес канала G0/0 на R1.
```
C:\>ping fe80::1

Pinging fe80::1 with 32 bytes of data:

Reply from FE80::1: bytes=32 time<1ms TTL=255
Reply from FE80::1: bytes=32 time<1ms TTL=255
Reply from FE80::1: bytes=32 time<1ms TTL=255
Reply from FE80::1: bytes=32 time<1ms TTL=255

Ping statistics for FE80::1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
#### Вопросы для повторения
1.	Почему обоим интерфейсам Ethernet на R1 можно назначить один и тот же локальный адрес канала — FE80::1?
```
Область использования локального адреса ограничена одним каналом, глобальная маршрутизация по этому адресу не может быть выполнена, поэтому одинаковый локальный адрес может быть присвоен всем интерфейсам маршрутизатора.
```
2.	Какой идентификатор подсети в индивидуальном IPv6-адресе 2001:db8:acad::aaaa:1234/64?
```
2001:0db8:acad:0000:0000:0000:aaaa:1234	        
Префикс глобальной маршрутизации:2001:0db8:acad
Идентификатор подсети: 0000
Идентификатор интерфейса:0000:0000:aaaa:1234
```
