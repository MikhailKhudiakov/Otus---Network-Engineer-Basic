
### Домашнее задание №2 
### Лабораторная работа. Просмотр таблицы МАС-адресов коммутатора. 
#### Топология.
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ2/%D1%82%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F%20%D0%B4%D0%B72.bmp)
#### Таблица адресации
Устройство |интерфейс| IP-адрес|маска подсети|
---|---|---|---
S1|VLAN1|192.168.1.11 |255.255.255.0
S2|VLAN1|192.168.1.12|255.255.255.0
PC-A|NIC|192.168.1.1|255.255.255.0
PC-B|NIC|192.168.1.2|255.255.255.0
### Цели
#### Часть 1. Создание и настройка сети.
#### Часть 2. Изучение таблицы МАС-адресов коммутатора.
### Часть 1. Создание и настройка сети.
#### Шаг 1. Подключите сеть в соответствии с топологией.
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ2/%D1%82%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F%202.bmp)
#### Шаг 2. Настройте узлы ПК.
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
#### Шаг 4.Настройте базовые параметры каждого коммутатора.
  а.Настройте имена уcтройств в соответствии с топологией.
  
  b.Настройте IP-адреса, как указано в таблице адресации.
  ````S1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#int vlan1
S1(config-if)#ip address 192.168.1.11 255.255.255.0
S1(config-if)#no shutdown

S1(config-if)#
%LINK-5-CHANGED: Interface Vlan1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
````
````S2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S2(config)#int vlan1
S2(config-if)#ip address 192.168.1.12 255.255.255.0
S2(config-if)#no shutdown

S2(config-if)#
%LINK-5-CHANGED: Interface Vlan1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
````

  с.Назначьте **cisco** в качетсве паролей консоли и VTY.
  
  d.Назначьте **class** в качестве пароля доступа к привелегированному режиму EXEC.
  
  ````Switch>enable
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#no ip domain-lookup
Switch(config)#hostname S2
S2(config)#service password-encryption
S2(config)#enable secret class
S2(config)#line console 0
S2(config-line)#password cisco
S2(config-line)#login
S2(config-line)#end
S2#
%SYS-5-CONFIG_I: Configured from console by console
````
### Часть 2. Изучение таблицы МАС-адресов коммутатора
#### Шаг 1. Запишите МАС-адреса сетевых устройств.

a. MAC-адреса PC
 
   ````
    C:\>ipconfig /all
    FastEthernet0 Connection:(default port)
    Connection-specific DNS Suffix..: 
    Physical Address................: 0010.1194.6594
    Link-local IPv6 Address.........: FE80::210:11FF:FE94:6594
    IPv6 Address....................: ::
    IPv4 Address....................: 192.168.1.1
    Subnet Mask.....................: 255.255.255.0
    Default Gateway.................: ::
                                     0.0.0.0
                     
   ````
   
   ````
    C:\>ipconfig /all
    FastEthernet0 Connection:(default port)
    Connection-specific DNS Suffix..: 
    Physical Address................: 000C.85B0.C004
    Link-local IPv6 Address.........: FE80::20C:85FF:FEB0:C004
    IPv6 Address....................: ::
    IPv4 Address....................: 192.168.1.2
    Subnet Mask.....................: 255.255.255.0
    Default Gateway.................: ::
                                     0.0.0.0        
   ````
  
   ````  
    MAC-адрес компьютера PC-A:000C.85B0.C004
    MAC-адрес компьютера PC-B:0010.1194.6594

   ````
b. MAC-адреса коммутаторов

   ```` 
   S1>show interface f0/1
   FastEthernet0/1 is up, line protocol is up (connected)
   Hardware is Lance, address is 0003.e497.b701 (bia 0003.e497.b701)
   ````
   ````
   S2>show interface f0/1
   FastEthernet0/1 is up, line protocol is up (connected)
   Hardware is Lance, address is 00e0.f9b4.7301 (bia 00e0.f9b4.7301)
   ````
   ````
    МАС-адрес коммутатора S1 Fast Ethernet 0/1:0003.e497.b701
    МАС-адрес коммутатора S2 Fast Ethernet 0/1:00e0.f9b4.7301
    
   ````
#### Шаг 2. Просмотрите таблицу МАС-адресов коммутатора.
   a.	Подключитесь к коммутатору S2 через консоль и войдите в привилегированный режим EXEC.
   b.	В привилегированном режиме EXEC введите команду show mac address-table и нажмите клавишу ввода.
   ````
   S2#show mac-address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

   1    0003.e497.b701    DYNAMIC     Fa0/1
   ````
   Записаны ли в таблице МАС-адресов какие-либо МАС-адреса?
   Какие МАС-адреса записаны в таблице? 
   С какими портами коммутатора они сопоставлены и каким устройствам принадлежат? 
   ````
   В таблице МАС-адресов содержится один адрес коммутатора S1, подключенного к порту Fa0/1.
   ````
   Если вы не записали МАС-адреса сетевых устройств в шаге 1, как можно определить, каким устройствам принадлежат МАС-адреса, используя только выходные данные команды show mac address-table? Работает ли это решение в любой ситуации?
   ````
   Таблица МАС-адресов содержит номера портов, к которым подключены устройства,
   по ним можно определить, какому устройству принадлежит МАС-адрес. Если устройство подключено не напрямую, 
   а через промежуточный коммутатор, то по номеру порта однозначно определить МАС-адрес не получится. 
   ````
#### Шаг 3. Очистите таблицу МАС-адресов коммутатора S2 и снова отобразите таблицу МАС-адресов.
    
   a.	В привилегированном режиме EXEC введите команду clear mac address-table dynamic и нажмите клавишу Enter.
   
   b.	Снова быстро введите команду show mac address-table.
   Указаны ли в таблице МАС-адресов адреса для VLAN 1? Указаны ли другие МАС-адреса?
   ````
   Таблица МАС адресов очистилась. МАС-адреса отсутствуют.
   ````
   Через 10 секунд введите команду show mac address-table и нажмите клавишу ввода. Появились ли в таблице МАС-адресов новые адреса?
   
    Появился МАС-адрес коммутатора S1.
#### Шаг 4. С компьютера PC-B отправьте эхо-запросы устройствам в сети и просмотрите таблицу МАС-адресов коммутатора.    

   a.	На компьютере PC-B откройте командную строку и еще раз введите команду arp -a.
   ````
   C:\>arp -a
   No ARP Entries Found
   ````
   Не считая адресов многоадресной и широковещательной рассылки, сколько пар IP- и МАС-адресов устройств было получено через протокол ARP?
   ````
    По протоколу ARP обмена данными не выполнялся.
   ````
   b.	Из командной строки PC-B отправьте эхо-запросы на компьютер PC-A, а также коммутаторы S1 и S2.
   
  ````
   Pinging 192.168.1.11 with 32 bytes of data:

Request timed out.
Reply from 192.168.1.11: bytes=32 time<1ms TTL=255
Reply from 192.168.1.11: bytes=32 time<1ms TTL=255
Reply from 192.168.1.11: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.1.11:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>ping 192.168.1.12

Pinging 192.168.1.12 with 32 bytes of data:

Request timed out.
Reply from 192.168.1.12: bytes=32 time<1ms TTL=255
Reply from 192.168.1.12: bytes=32 time<1ms TTL=255
Reply from 192.168.1.12: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.1.12:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>ping 192.168.1.1

Pinging 192.168.1.1 with 32 bytes of data:

Reply from 192.168.1.1: bytes=32 time=5ms TTL=128
Reply from 192.168.1.1: bytes=32 time<1ms TTL=128
Reply from 192.168.1.1: bytes=32 time<1ms TTL=128
Reply from 192.168.1.1: bytes=32 time<1ms TTL=128

Ping statistics for 192.168.1.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 5ms, Average = 1ms
  ````
   От всех ли устройств получены ответы? 
   
    Получены ответы от всех устройств.
    
 c.	Подключившись через консоль к коммутатору S2, введите команду show mac address-table. 
 ````
 S2#show mac-address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

   1    0003.e497.b701    DYNAMIC     Fa0/1
   1    000c.85b0.c004    DYNAMIC     Fa0/16
   1    0010.1194.6594    DYNAMIC     Fa0/1
   ````
 Добавил ли коммутатор в таблицу МАС-адресов дополнительные МАС-адреса? Если да, то какие адреса и устройства?
 ````
 Коммутатор добавил в таблицу МАС-адреса PC-A и PC-B.
 ````
 На компьютере PC-B откройте командную строку и еще раз введите команду arp -a.
 ````
 C:\>arp -a
  Internet Address      Physical Address      Type
  192.168.1.1           0010.1194.6594        dynamic
  192.168.1.11          0009.7c66.54ce        dynamic
  192.168.1.12          0030.a39d.9d63        dynamic
  ````
 Появились ли в ARP-кэше компьютера PC-B дополнительные записи для всех сетевых устройств, которым были отправлены эхо-запросы?
 ````
 В ARP-кэше РС-В появились три пары IP и МАС адресов для всех устройств, котрым был отправлен эхо-запрос.
````
#### Вопрос для повторения
В сетях Ethernet данные передаются на устройства по соответствующим МАС-адресам. Для этого коммутаторы и компьютеры динамически создают ARP-кэш и таблицы МАС-адресов. Если компьютеров в сети немного, эта процедура выглядит достаточно простой. Какие сложности могут возникнуть в крупных сетях?
````
В крупных сетях большое количество ARP запросов снижает пропускную способность сети для основного трафика.
Поэтому рекомендуется ограничивать количество хостов в одной сети примерно на уровне 300 единиц 
и разделять крупные сети на подсети, используя маршрутизаторы.

````
  
    
