
### Домашнее задание №1 
### Лабораторная работа. Базовая настройка коммутатора. 
#### Топология.
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ1/%D1%82%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F.png)
#### Таблица адресации
Устройство |интерфейс| IP-адрес/префикс|
---|---|---
S1|VLAN1|192.168.1.2 /24
PC-A|NIC|192.168.1.10 /24
### Задачи
#### Часть 1. Проверка конфигурации коммутатора по умолчанию
#### Часть 2. Создание сети и настройка основных параметров устройства
* Настройте базовые параметры коммутатора.
*	Настройте IP-адрес для ПК.
#### Часть 3. Проверка сетевых подключений
* Отобразите конфигурацию устройства.
* Протестируйте сквозное соединение, отправив эхо-запрос.
* Протестируйте возможности удаленного управления с помощью Telnet.
#### Общие сведения/сценарий
На коммутаторах Cisco можно настроить особый IP-адрес, который называют виртуальным интерфейсом коммутатора (SVI). SVI или адрес управления можно использовать для удаленного доступа к коммутатору в целях отображения или настройки параметров. Если для SVI сети VLAN 1 назначен IP-адрес, то по умолчанию все порты в сети VLAN 1 имеют доступ к IP-адресу управления SVI. 
В ходе данной лабораторной работы вам предстоит построить простую топологию, используя Ethernet-кабель локальной сети, и получить доступ к коммутатору Cisco, используя консольное подключение и методы удаленного доступа. Перед настройкой базовых параметров коммутатора нужно проверить настройки коммутатора по умолчанию. В число таких основных параметров коммутации входят имя устройства, описание интерфейса, локальные пароли, объявление дня (MOTD), IP-адрес и статический MAC-адрес. Необходимо также показать использование IP-адреса управления для удаленного управления коммутатором. Топология включает один коммутатор и один узел с использованием только портов Ethernet и консольных портов.

**Примечание.** В лабораторной работе используются коммутаторы Cisco Catalyst 2960s с операционной системой Cisco IOS 15.2(2) (образ lanbasek9). Допускается использование других моделей коммутаторов и других версий Cisco IOS. В зависимости от модели устройства и версии Cisco IOS доступные команды и результаты их выполнения могут отличаться от тех, которые показаны в лабораторных работах. 

**Примечание:** Убедитесь, что все настройки коммутатора удалены и загрузочная конфигурация отсутствует. Если вы не уверены, обратитесь к инструктору. Процедура инициализации и перезагрузки коммутатора описана в приложении А.


#### Необходимые ресурсы
*	1 коммутатор (Cisco 2960 с ПО Cisco IOS версии 15.2(2) с образом lanbasek9 или аналогичная модель)
*	1 ПК (под управлением Windows с программой эмуляции терминала, например, Tera Term)
*	1 консольный кабель для настройки устройства на базе Cisco IOS через консольный порт.
*	1 кабель Ethernet, как показано в топологии.
### Часть 1. Создание сети и проверка настроек коммутатора по умолчанию
В первой части лабораторной работы вам предстоит настроить топологию сети и проверить настройку коммутатора по умолчанию.
#### Шаг 1. Создайте сеть согласно топологии.

a.	Подсоедините консольный кабель, как показано в топологии. На данном этапе не подключайте кабель Ethernet компьютера PC-A. 

**Примечание.** При использовании Netlab отключите интерфейс F0/6 на коммутаторе S1. Это имеет такой же эффект, как отсоединение компьютера PC-A от коммутатора S1.

![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ1/%D0%BA%D0%BE%D0%BD%D1%81%D0%BE%D0%BB%D1%8C%20%D1%81%D1%85%D0%B5%D0%BC%D0%B0%20%D0%BF%D0%BE%D0%B4%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D0%B8%D1%8F.bmp)

b.	Установите консольное подключение к коммутатору с компьютера PC-A с помощью Tera Term или другой программы эмуляции терминала. 

![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ1/%D0%BF%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D1%8B%20%D0%BF%D0%BE%D0%B4%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D0%B8%D1%8F.bmp)

Вопрос:
Почему нужно использовать консольное подключение для первоначальной настройки коммутатора? Почему нельзя подключиться к коммутатору через Telnet или SSH?

```
Консольное подключение используется для первоначальной настройки ip-адреса. 
Первоначальное подключение по Telnet или SSH невозможно, т.к.  ip-адрес неизвестен или не настроен.
```
#### Шаг 2. Проверьте настройки коммутатора по умолчанию.
На данном этапе вам нужно проверить такие параметры коммутатора по умолчанию, как текущие настройки коммутатора, данные IOS, свойства интерфейса, сведения о VLAN и флеш-память.
Все команды IOS коммутатора можно выполнять из привилегированного режима. Доступ к привилегированному режиму нужно ограничить с помощью пароля, чтобы предотвратить неавторизованное использование устройства — через этот режим можно получить прямой доступ к режиму глобальной конфигурации и командам, используемым для настройки рабочих параметров. Пароли можно будет настроить чуть позже.

К привилегированному набору команд относятся команды пользовательского режима, а также команда configure, при помощи которой выполняется доступ к остальным командным режимам. Введите команду enable, чтобы войти в привилегированный режим EXEC.

a.	Предположим, что коммутатор не имеет файла конфигурации, сохраненного в энергонезависимой памяти (NVRAM). Консольное подключение к коммутатору с помощью Tera Term или другой программы эмуляции терминала предоставит доступ к командной строке пользовательского режима EXEC в виде Switch>. Введите команду enable, чтобы войти в привилегированный режим EXEC.

Откройте окно конфигурации
Обратите внимание, что измененная в конфигурации строка будет отражать привилегированный режим EXEC.
Убедитесь, что на коммутаторе находится пустой файл конфигурации по умолчанию, с помощью команды show running-config привилегированного режима EXEC. Если конфигурационный файл был предварительно сохранен, его нужно удалить. В зависимости от модели коммутатора и версии IOS ваша конфигурация может слегка отличаться. Тем не менее, настроенных паролей или IP-адресов в конфигурации быть не должно. Выполните очистку настроек и перезагрузите коммутатор, если ваш коммутатор имеет настройки, отличные от настроек по умолчанию.

b.	Изучите текущий файл running configuration.

![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ1/show%20running-config.bmp)
![show run config](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ1/show%20running-config%202.bmp)

Вопросы:
Сколько интерфейсов FastEthernet имеется на коммутаторе 2960?
```
На коммутаторе имеется 24 интерфейса FastEthernet.
```
Сколько интерфейсов Gigabit Ethernet имеется на коммутаторе 2960?
```
На коммутаторе имеется 2 интерфейса GigabitEthernet.
```
Каков диапазон значений, отображаемых в vty-линиях?
```
В vty линиях два диапазона значений от 0 до 4 и от 5 до 15. Всего 16 линий.
```
c.	Изучите файл загрузочной конфигурации (startup configuration), который содержится в энергонезависимом ОЗУ (NVRAM).
![startup config](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ1/show%20startup-config.bmp)

Вопрос:
Почему появляется это сообщение?
```
Появляется сообщение «startup- config  is not present» т.к. ещё ни разу не выполнялось
сохранение файла конфигурации и коммутатор находится в исходном состоянии с заводскими настройками.
```
d.	Изучите характеристики SVI для VLAN 1.

![SVI](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ1/show%20ip%20int%20vlan1.bmp)

![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ1/show%20interface2.bmp)

Вопросы:
Назначен ли IP-адрес сети VLAN 1?
```
IP адрес не назначен («no ip adress»)
```
Какой MAC-адрес имеет SVI? Возможны различные варианты ответов.
```
Hardware is Lance, address is 00d0.5897.d501 (bia 00d0.5897.d501)
```
Данный интерфейс включен?
```
Интерфейс выключен. 
FastEthernet0/1 is down, line protocol is down (disabled)
```
e.	Изучите IP-свойства интерфейса SVI сети VLAN 1.

![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ1/show%20ip%20int%20brief.bmp)


Вопрос:
Какие выходные данные вы видите?
```
Интерфейс  Vlan1 в находится исходном выключенном состоянии. IP-адрес не назначен.
Vlan1 is administratively down, line protocol is down. IP unassigned.
```
f.	Подсоедините кабель Ethernet компьютера PC-A к порту 6 на коммутаторе и изучите IP-свойства интерфейса SVI сети VLAN 1. Дождитесь согласования параметров скорости и дуплекса между коммутатором и ПК.
**Примечание.** При использовании Netlab включите интерфейс F0/6 на коммутаторе S1.

![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ1/%D0%BA%D0%BE%D0%BD%D1%81%D0%BE%D0%BB%D1%8C%20%2B%20ethernet%20%D1%81%D1%85%D0%B5%D0%BC%D0%B0.bmp)

Вопрос:
Какие выходные данные вы видите?

![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ1/state%20to%20up%200-6.bmp)
```
Появляется сообщение об изменении состояния линии 5 на включенное.
```
g.	Изучите сведения о версии ОС Cisco IOS на коммутаторе.

![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ1/show%20version%201.bmp)
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ1/Show%20version%202.bmp)

Вопросы:
Под управлением какой версии ОС Cisco IOS работает коммутатор?
```
Версия OC: Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4.
```
Как называется файл образа системы?
```
Файл образа системы: System image file is "flash:c2960-lanbasek9-mz.150-2.SE4.bin".
```
Какой базовый MAC-адрес назначен коммутатору?
```
00:17:59:A7:51:80
```
h.	Изучите свойства по умолчанию интерфейса FastEthernet, который используется компьютером PC-A.

Switch# show interface f0/6 

![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ1/show%20int%20f0-6.bmp)

Вопрос:
Интерфейс включен или выключен?
Что нужно сделать, чтобы включить интерфейс?
```
Интерфейс включен: FastEthernet0/6 is up, line protocol is up (connected).
```
Какой MAC-адрес у интерфейса?
```
МАС-адрес интерфейса: 00d0.5897.d506
```
Какие настройки скорости и дуплекса заданы в интерфейсе?
```
Настройка скорости: Full-duplex, 100Mb/s
```
i.	Изучите параметры сети VLAN по умолчанию на коммутаторе.
```
Switch> show vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23, Fa0/24
                                                Gig0/1, Gig0/2
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1    enet  100001     1500  -      -      -        -    -        0      0
1002 fddi  101002     1500  -      -      -        -    -        0      0   
1003 tr    101003     1500  -      -      -        -    -        0      0   
1004 fdnet 101004     1500  -      -      -        ieee -        0      0   
1005 trnet 101005     1500  -      -      -        ibm  -        0      0   

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------

Remote SPAN VLANs
------------------------------------------------------------------------------

Primary Secondary Type              Ports
------- --------- ----------------- ------------------------------------------
Switch>
```
Какое имя присвоено сети VLAN 1 по умолчанию?
```
Имя по умолчанию: default
```
Какие порты расположены в сети VLAN 1?
```
Порты в сети VLAN1: Fa0/1, Fa0/2, Fa0/3, Fa0/4, Fa0/5, Fa0/6, Fa0/7,
Fa0/8, Fa0/9, Fa0/10, Fa0/11, Fa0/12, Fa0/13, Fa0/14, Fa0/15, Fa0/16,
Fa0/17, Fa0/18, Fa0/19, Fa0/20, Fa0/21, Fa0/22, Fa0/23, Fa0/24, Gig0/1, Gig0/2
```
Активна ли сеть VLAN 1?
```
Сеть VLAN1 активна: Status active
```
К какому типу сетей VLAN принадлежит VLAN по умолчанию?
```
Тип сети VLAN1: Type enet – Ethernet
```
j.	Изучите флеш-память.

Выполните одну из следующих команд, чтобы изучить содержимое флеш-каталога.

Switch# show flash 

Switch# dir flash: 

В конце имени файла указано расширение, например .bin. Каталоги не имеют расширения файла.

```
Switch#show flash
Directory of flash:/

    1  -rw-     4670455          <no date>  2960-lanbasek9-mz.150-2.SE4.bin

64016384 bytes total (59345929 bytes free)

```
Вопрос:
Какое имя присвоено образу Cisco IOS?
```
Образу Cisco IOS присвоено имя 2960-lanbasek9-mz.150-2.SE4.bin
```
### Часть 2. Настройка базовых параметров сетевых устройств
Во второй части необходимо будет настроить основные параметры коммутатора и компьютера.

#### Шаг 1. Настройте базовые параметры коммутатора.

a.	В режиме глобальной конфигурации скопируйте следующие базовые параметры конфигурации и вставьте их в файл на коммутаторе S1. 

no ip domain-lookup

hostname S1

service password-encryption

enable secret class

banner motd #

Unauthorized access is strictly prohibited. #

```
Switch(config)#no ip domain-lookup
Switch(config)#hostname S1
S1(config)#service password-encryption
S1(config)#enable secret class
S1(config)#banner motd #
Enter TEXT message.  End with the character '#'.
Unauthorized access is strictly prohibited. #

```

b.	Назначьте IP-адрес интерфейсу SVI на коммутаторе. Благодаря этому вы получите возможность удаленного управления коммутатором.
Прежде чем вы сможете управлять коммутатором S1 удаленно с компьютера PC-A, коммутатору нужно назначить IP-адрес. Согласно конфигурации по умолчанию коммутатором можно управлять через VLAN 1. Однако в базовой конфигурации коммутатора не рекомендуется назначать VLAN 1 в качестве административной VLAN.
```
Unauthorized access is strictly prohibited. 

S1>en
Password: 
S1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#interface vlan1
S1(config-if)#ip address 192.168.1.2 255.255.255.0
S1(config-if)#no shutdown

S1(config-if)#
%LINK-5-CHANGED: Interface Vlan1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
```

c.	Доступ через порт консоли также следует ограничить  с помощью пароля. Используйте cisco в качестве пароля для входа в консоль в этом задании. Конфигурация по умолчанию разрешает все консольные подключения без пароля. Чтобы консольные сообщения не прерывали выполнение команд, используйте параметр logging synchronous.

S1(config)# line con 0

S1(config-line)# logging synchronous 
```
S1(config)#line console 0
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end
S1(config-line)#logging synchronous
S1#
%SYS-5-CONFIG_I: Configured from console by console
```

d.	Настройте каналы виртуального соединения для удаленного управления (vty), чтобы коммутатор разрешил доступ через Telnet. Если не настроить пароль VTY, будет невозможно подключиться к коммутатору по протоколу Telnet.
```
S1#
S1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#
S1(config)#line vty 0 15
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end
S1#
%SYS-5-CONFIG_I: Configured from console by console
```
Вопрос:
Для чего нужна команда login?
```
Команда login нужна для активации пароля.
```

#### Шаг 2. Настройте IP-адрес на компьютере PC-A.

Назначьте компьютеру IP-адрес и маску подсети в соответствии с таблицей адресации.

Назначьте компьютеру IP-адрес и маску подсети в соответствии с таблицей адресации.
1)	Перейдите в Панель управления. (Control Panel)
2)	В представлении «Категория» выберите « Просмотр состояния сети и задач».
3)	Щелкните Изменение параметров адаптера на левой панели.
4)	Щелкните правой кнопкой мыши интерфейс Ethernet и выберите «Свойства» .
5)	Выберите Протокол Интернета версии 4 (TCP/IPv4) > Свойства.
6)	Выберите Использовать следующий IP-адрес и введите IP-адрес и маску подсети  и нажмите ОК.

![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ1/ip%20PC1.bmp)

![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ1/ip%20PC.bmp)
### Часть 3. Проверка сетевых подключений
В третьей части лабораторной работы вам предстоит проверить и задокументировать конфигурацию коммутатора, протестировать сквозное соединение между компьютером PC-A и коммутатором S1, а также протестировать возможность удаленного управления коммутатором.

#### Шаг 1. Отобразите конфигурацию коммутатора.
Используйте консольное подключение на компьютере PC-A для отображения и проверки конфигурации коммутатора. Команда show run позволяет постранично отобразить всю текущую конфигурацию. Для пролистывания используйте клавишу пробела.

a.	Пример конфигурации приведен ниже. Параметры, которые вы настроили, выделены желтым. Другие параметры конфигурации — значения IOS по умолчанию.
Откройте окно конфигурации.

```
S1>en
Password: 
S1#show run
Building configuration...

Current configuration : 1318 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname S1
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
!
!
no ip domain-lookup
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
!
interface FastEthernet0/2
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 ip address 192.168.1.2 255.255.255.0
!
banner motd ^C
Unauthorized access is strictly prohibited. ^C
!
!
!
line con 0
 password 7 0822455D0A16
 logging synchronous
 login
!
line vty 0 4
 password 7 0822455D0A16
 login
line vty 5 15
 password 7 0822455D0A16
 login
!
!
!
!
end
  ```
  
b.	Проверьте параметры VLAN 1.

S1# show interface vlan 1 
```
S1#show interface vlan 1
Vlan1 is up, line protocol is up
  Hardware is CPU Interface, address is 0001.63ca.8227 (bia 0001.63ca.8227)
  Internet address is 192.168.1.2/24
  MTU 1500 bytes, BW 100000 Kbit, DLY 1000000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 21:40:21, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     1682 packets input, 530955 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicast)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     563859 packets output, 0 bytes, 0 underruns
     0 output errors, 23 interface resets
     0 output buffer failures, 0 output buffers swapped out

S1#
```

Какова полоса пропускания этого интерфейса?
```
Полоса пропускания интерфейса VLAN1 100Mbit (bandwidth = BW 100000 Kbit).
```
В каком состоянии находится VLAN 1?
```
VLAN1 включен, Vlan1 is up.
```

В каком состоянии находится канальный протокол?
```
Канальный протокол включен,  line protocol is up.
```

Закройте окно настройки.

### Шаг 2. Протестируйте сквозное соединение, отправив эхо-запрос.

a.	В командной строке компьютера PC-A с помощью утилиты ping проверьте связь сначала с адресом PC-A.

C:\> ping 192.168.1.10 

```
Packet Tracer PC Command Line 1.0
C:\>ping 192.168.1.10 

Pinging 192.168.1.10 with 32 bytes of data:

Reply from 192.168.1.10: bytes=32 time=3ms TTL=128
Reply from 192.168.1.10: bytes=32 time=19ms TTL=128
Reply from 192.168.1.10: bytes=32 time=19ms TTL=128
Reply from 192.168.1.10: bytes=32 time=18ms TTL=128

Ping statistics for 192.168.1.10:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 3ms, Maximum = 19ms, Average = 14ms

C:\>
```

b.	Из командной строки компьютера PC-A отправьте эхо-запрос на административный адрес интерфейса SVI коммутатора S1.

C:\> ping 192.168.1.2
```
C:\>ping 192.168.1.2

Pinging 192.168.1.2 with 32 bytes of data:

Request timed out.
Reply from 192.168.1.2: bytes=32 time<1ms TTL=255
Reply from 192.168.1.2: bytes=32 time<1ms TTL=255
Reply from 192.168.1.2: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.1.2:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>
```

Поскольку компьютеру PC-A нужно преобразовать МАС-адрес коммутатора S1 с помощью ARP, время ожидания передачи первого пакета может истечь. Если эхо-запрос не удается, найдите и устраните неполадки базовых настроек устройства. Проверьте как физические кабели, так и логическую адресацию.

#### Шаг 3. Проверьте удаленное управление коммутатором S1.
После этого используйте удаленный доступ к устройству с помощью Telnet. В этой лабораторной работе устройства PC-A и S1 расположены рядом. В производственной сети коммутатор может находиться в коммутационном шкафу на последнем этаже, в то время как административный компьютер находится на первом этаже. На данном этапе вам предстоит использовать Telnet для удаленного доступа к коммутатору S1 через его административный адрес SVI. Telnet — это не безопасный протокол, но вы можете использовать его для проверки удаленного доступа. В случае с Telnet вся информация, включая пароли и команды, отправляется через сеанс в незашифрованном виде. В последующих лабораторных работах вы будете использовать протокол SSH для удаленного доступа к сетевым устройствам.

a.	Откройте Tera Term или другую программу эмуляции терминала с возможностью Telnet. 

b.	Выберите сервер Telnet и укажите адрес управления SVI для подключения к S1.  Пароль: cisco.

c.	После ввода пароля cisco вы окажетесь в командной строке пользовательского режима. Для перехода в исполнительский режим EXEC введите команду enable и используйте секретный пароль class.

d.	Сохраните конфигурацию.

e.	Чтобы завершить сеанс Telnet, введите exit.

![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ1/telnet.bmp)

### Вопросы для повторения
1.	Зачем необходимо настраивать пароль VTY для коммутатора?
```
Пароли VTY необходимо настраивать для обеспечения доступа к коммутатору по протоколу Telnet
```
2.	Что нужно сделать, чтобы пароли не отправлялись в незашифрованном виде?
```
Чтобы пароли не отправлялись в незашифрованном виде в режиме глобальной конфигурации используется команда 
service password-encryption
```

