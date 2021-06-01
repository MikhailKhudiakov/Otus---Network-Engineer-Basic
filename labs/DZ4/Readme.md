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
    FF02::2
    FF02::1:FF00:1
```
```
Интерфейсу G0/0/0 назаначены два мультикастовых адреса для групп многоадресной рассылки: 
FF02::1 группа многоадресной рассылки всех узлов IPv6 в локальной сети или канале;
FF02::2 группа многоадресной рассылки всех роутреров IPv6;
а также FF02::1:FF00:1 групповой адрес запрашиваемого узла для многоадресной рассылки на запрашиваемые узлы.
```

![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ2/IP%20PC-A.bmp)
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ2/IP%20PC-B.bmp)
#### Шаг 3.Выполните инициализацию и перезагрузку коммутаторов.
````Switch>enable
Switch#show flash
Directory of flash:/

    1  -rw-     4670455          <no date>  2960-lanbasek9-mz.150-2.SE4.bin

64016384 bytes total (59345929 bytes free)
Switch#erase startup-config
Erasing the nvram filesystem will remove all configuration files! Continue? [confirm]
[OK]
Erase of nvram: complete
%SYS-7-NV_BLOCK_INIT: Initialized the geometry of nvram
Switch#reload
````
