### Домашнее задание №6
### Лабораторная работа. Внедрение маршрутизации между виртуальными локальными сетями.
#### Топология.
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ6/%D1%82%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F%20%D0%B4%D0%B76.bmp)
#### Таблица адресации
Устройство |интерфейс| IPv-адрес|маска подсети|шлюз по умолчанию|
---|---|---|---|---
R1|G0/0/1.10|192.168.10.1|255.255.255.0	|-|
|  |G0/0/1.20|192.168.20.1|255.255.255.0	|-|
|  |G0/0/1.30|192.168.30.1|255.255.255.0	|-|
| |G0/0/1.1000|||-|
S1|VLAN10|192.168.10.11|255.255.255.0|192.168.10.1
S2|VLAN10|192.168.10.12|255.255.255.0|192.168.10.1
PC-A|NIC|192.168.20.3|255.255.255.0|192.168.20.1
PC-B|NIC|192.168.30.3|255.255.255.0|192.168.30.1
#### Таблица VLAN
VLAN|Имя|Назначенный интерфейс|
---|---|---
10|Управление|S1:VLAN10
| ||S2VLAN10
20|Sales|S1:F0/6
30|Operation|S2:F0/18
999|Parking_Lot|C1:F0/2-4,F0/7-24,G0/1-2
| ||C2:F0/2-17,F0/18-24,G0/1-2
### 	Задачи
Часть 1. Создание сети и настройка основных параметров устройства.  
Часть 2. Создание сетей VLAN и назначение портов коммутатора.  
Часть 3. Настройка транка 802.1Q между коммутаторами.  
Часть 4. Настройка маршрутизации между сетями VLAN.  
Часть 5. Проверка, что маршрутизация между VLAN работает.  

### Часть 1. Настройка основных параметров устройств
#### Шаг 1. Создайте сеть согласно топологии.
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ6/%D1%82%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F%20PT%20%D0%B4%D0%B76.bmp)
#### Шаг 2. Настройте базовые параметры для маршрутизатора.  
  a.	Подключитесь к маршрутизатору с помощью консоли и активируйте привилегированный режим EXEC.  
  b.	Войдите в режим конфигурации.  
  c.	Назначьте маршрутизатору имя устройства.  
  d.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.  
  e.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.  
  f.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.  
  g.	Установите cisco в качестве пароля виртуального терминала и активируйте вход.  
  h.	Зашифруйте открытые пароли.  
  i.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.  
  j.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.  
  k.	Настройте на маршрутизаторе время.  
  
        Router>enable  
        Router#configure terminal
        Router(config)#hostname R1 
        R1(config)#no ip domain-lookup  
        R1(config)#enable secret class 
        R1(config)#line console 0   
        R1(config-line)#password cisco   
        R1(config-line)#login   
        R1(config-line)#exit   
        R1(config)#line vty 0 15   
        R1(config-line)#password cisco   
        R1(config-line)#login   
        R1(config-line)#exit   
        R1(config)#service password-encryption    
        R1(config)#banner motd ^Authorized Access Only^   
        R1#copy running-config startup-config    
        R1#clock set 19:00:00 10 june 2021   
    
  
#### Шаг 3. Настройте базовые параметры каждого коммутатора.
a.	Присвойте коммутатору имя устройства.  
b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.  
c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.  
d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.  
e.	Установите cisco в качестве пароля виртуального терминала и активируйте вход.  
f.	Зашифруйте открытые пароли.  
g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.  
h.	Настройте на коммутаторах время.  
i.	Сохранение текущей конфигурации в качестве начальной.  
  
    Switch>enable  
    Switch#configure terminal  
    Enter configuration commands, one per line.  End with CNTL/Z.   
    Switch(config)#hostname S1    
    S1(config)#no ip domain-lookup  
    S1(config)#enable secret class  
    S1(config)#line console 0  
    S1(config-line)#password cisco  
    S1(config-line)#login  
    S1(config-line)#exit  
    S1(config)#line vty 0 15  
    S1(config-line)#password cisco  
    S1(config-line)#login  
    S1(config-line)#exit  
    S1(config)#service password-encryption  
    S1(config)#banner motd ^Autorized Access Only^  
    S1#  
    %SYS-5-CONFIG_I: Configured from console by console  
    S1#copy running-config startup-config   
    S1#clock set 19:00:00 10 june 2021  
    
    
    Switch>enable
    Switch#configure terminal 
    Enter configuration commands, one per line.  End with CNTL/Z.
    Switch(config)#hostname S2
    S2(config)#no ip domain-lookup
    S2(config)#enable secret class
    S2(config)#line console 0
    S2(config-line)#password cisco
    S2(config-line)#login
    S2(config-line)#exit
    S2(config)#line vty 0 15
    S2(config-line)#password cisco
    S2(config-line)#login
    S2(config-line)#exit
    S2(config)#service password-encryption 
    S2(config)#banner motd ^Autorized Access Only^
    S2(config)#^Z
    S2#
    %SYS-5-CONFIG_I: Configured from console by console
    S2#clock set 19:00:00 10 june 2021
    S2#copy running-config startup-config 
    
    
#### Шаг 4. Настройте узлы ПК.

### Часть 2. Создание сетей VLAN и назначение портов коммутатора
#### Шаг 1. Создайте сети VLAN на коммутаторах.  
a.	Создайте и назовите необходимые VLAN на каждом коммутаторе из таблицы выше.  

    S1>en
    Password:   
    S1#configure terminal   
    Enter configuration commands, one per line.  End with CNTL/Z.  
    S1(config)#vlan 10  
    S1(config-vlan)#name Managment  
    S1(config-vlan)#end  

    S1#configure terminal   
    Enter configuration commands, one per line.  End with CNTL/Z.  
    S1(config)#vlan 20    
    S1(config-vlan)#name Sales  

    S1#conf t  
    Enter configuration commands, one per line.  End with CNTL/Z.  
    S1(config)#vlan 30  
    S1(config-vlan)#name Operations  
    S1(config-vlan)#end  

    S1#conf t  
    Enter configuration commands, one per line.  End with CNTL/Z.  
    S1(config)#vlan 999  
    S1(config-vlan)#name Parking_Lot  
    S1(config-vlan)#end  
  
  
    S2>en  
    Password:   
    S2#conf t  
    Enter configuration commands, one per line.  End with CNTL/Z.  
    S2(config)#vlan 10  
    S2(config-vlan)#name Managment  
    S2(config-vlan)#^Z  
    S2#  
    %SYS-5-CONFIG_I: Configured from console by console  

    S2#conf t  
    Enter configuration commands, one per line.  End with CNTL/Z.  
    S2(config)#vlan 20  
    S2(config-vlan)#name Sales  
    S2(config-vlan)#^Z  
    S2#  
    %SYS-5-CONFIG_I: Configured from console by console  

    S2#conf t  
    Enter configuration commands, one per line.  End with CNTL/Z.  
    S2(config)#vlan 30   
    S2(config-vlan)#name Operations  
    S2(config-vlan)#^Z  
    S2#  
    %SYS-5-CONFIG_I: Configured from console by console  

    S2#conf t  
    Enter configuration commands, one per line.  End with CNTL/Z.  
    S2(config)#vlan 999  
    S2(config-vlan)#name Parking_Lot  
    S2(config-vlan)#^Z  
 
 
 
 
b.	Настройте интерфейс управления и шлюз по умолчанию на каждом коммутаторе, используя информацию об IP-адресе в таблице адресации.   
    
    S1>en  
    Password:   
    S1#conf t  
    Enter configuration commands, one per line.  End with CNTL/Z.  
    S1(config)#int vlan10  
    S1(config-if)#  
    %LINK-5-CHANGED: Interface Vlan10, changed state to up  

    S1(config-if)#ip address 192.168.10.11 255.255.255.0  
    S1(config-if)#no shutdown   

    S1(config)#ip default-gateway 192.168.10.1  
    
    
    S2>en
    Password: 
    S2#conf t
    Enter configuration commands, one per line.  End with CNTL/Z.
    S2(config)#interface vlan10
    S2(config-if)#
    %LINK-5-CHANGED: Interface Vlan10, changed state to up

    S2(config-if)#ip address 192.168.10.12 255.255.255.0
    S2(config-if)#no shutdown 
    S2(config-if)#end

    S2(config)#ip default-gateway 192.168.10.1

    
    
c.	Назначьте все неиспользуемые порты коммутатора VLAN Parking_Lot, настройте их для статического режима доступа и административно деактивируйте их.    
     
    S1#conf t  
    Enter configuration commands, one per line.  End with CNTL/Z.  
    S1(config)#interface range f0/2-4,f0/7-24,g0/1-2  
    S1(config-if-range)#switchport mode access   
    S1(config-if-range)#switchport access vlan 999  
    S1(config-if-range)#shutdown   

    %LINK-5-CHANGED: Interface FastEthernet0/2, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/4, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/7, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/8, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/9, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/10, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/11, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/12, changed state to administratively down
  
    %LINK-5-CHANGED: Interface FastEthernet0/13, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/14, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/15, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/16, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/17, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/18, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/19, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/20, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/21, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/22, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/23, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/24, changed state to administratively down

    %LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to administratively down

    %LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to administratively down
    S1(config-if-range)#end  
    
    S2#conf t  
    Enter configuration commands, one per line.  End with CNTL/Z.  
    S2(config)#interface range f0/2-17,f0/19-24,g0/1-2  
    S2(config-if-range)#switchport mode access  
    S2(config-if-range)#switchport access vlan 999  
    S2(config-if-range)#shutdown

    %LINK-5-CHANGED: Interface FastEthernet0/2, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/4, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/5, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/6, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/7, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/8, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/9, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/10, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/11, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/12, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/13, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/14, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/15, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/16, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/17, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/19, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/20, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/21, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/22, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/23, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/24, changed state to administratively down

    %LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to administratively down

    %LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to administratively down
    S2(config-if-range)#end
   
   #### Шаг 2. Назначьте сети VLAN соответствующим интерфейсам коммутатора.  
a.	Назначьте используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настройте их для режима статического доступа. 
    
    S1#conf t  
    Enter configuration commands, one per line.  End with CNTL/Z.  
    S1(config)#int f0/6  
    S1(config-if)#switchport mode access   
    S1(config-if)#switchport access vlan 20  
    S1(config-if)#end  
    
    S2#conf t  
    Enter configuration commands, one per line.  End with CNTL/Z.  
    S2(config)#interface f0/18  
    S2(config-if)#switchport mode access   
    S2(config-if)#switchport access vlan 30  
    S2(config-if)#end  
    
b.	Убедитесь, что VLAN назначены на правильные интерфейсы.
    
    S1#show vlan
    VLAN Name                             Status    Ports
    ---- -------------------------------- --------- -------------------------------
    1    default                          active    Fa0/1, Fa0/5
    10   Managment                        active    
    20   Sales                            active    Fa0/6
    30   Operations                       active    
    999  Parking_Lot                      active    Fa0/2, Fa0/3, Fa0/4, Fa0/7
                                                Fa0/8, Fa0/9, Fa0/10, Fa0/11
                                                Fa0/12, Fa0/13, Fa0/14, Fa0/15
                                                Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24, Gig0/1, Gig0/2
                                                
    S2#show vlan
    VLAN Name                             Status    Ports
    ---- -------------------------------- --------- -------------------------------
    1    default                          active    Fa0/1
    10   Managment                        active    
    20   Sales                            active    
    30   Operations                       active    Fa0/18
    999  Parking_Lot                      active    Fa0/2, Fa0/3, Fa0/4, Fa0/5
                                                Fa0/6, Fa0/7, Fa0/8, Fa0/9
                                                Fa0/10, Fa0/11, Fa0/12, Fa0/13
                                                Fa0/14, Fa0/15, Fa0/16, Fa0/17
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24, Gig0/1, Gig0/      
                                                
### Часть 3. Конфигурация магистрального канала стандарта 802.1Q между коммутаторами

#### Шаг 1. Вручную настройте магистральный интерфейс F0/1 на коммутаторах S1 и S2.
a.	Настройка статического транкинга на интерфейсе F0/1 для обоих коммутаторов.
    
    S1(config)#interface fa0/1  
    S1(config-if)#switchport mode trunk  
    S1(config-if)#    
    %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to down

    %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan10, changed state to up   
    S1(config-if)#end  
    
    S2(config)#interface fa0/1  
    S2(config-if)#switchport mode trunk   
    S2(config-if)#  
    %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to down

    %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan10, changed state to up
    S2(config-if)#end
    
b.	Установите native VLAN 1000 на обоих коммутаторах.
    
    S1(config)#vlan 1000  
    S1(config-vlan)#exit   
    S1(config)#interface fa0/1  
    S1(config-if)#switchport trunk native vlan 1000  
    S1(config-if)#  
    %CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/1 (1000), with S2 FastEthernet0/1 (1).  
    
    S2(config)#vlan 1000  
    S2(config-vlan)#exit  
    S2(config)#interface fa0/1  
    S2(config-if)#switchport trunk native vlan 1000  
    S2(config-if)#%SPANTREE-2-UNBLOCK_CONSIST_PORT: Unblocking FastEthernet0/1 on VLAN1000. Port consistency restored.  
  
  
c.	Укажите, что VLAN 10, 20, 30 и 1000 могут проходить по транку.  
    
    S1(config)#int fa0/1  
    S1(config-if)#switchport trunk allowed vlan 10,20,30,1000  
    S1(config-if)#end  
    
    S2(config)#int fa0/1  
    S2(config-if)#switchport trunk allowed vlan 10,20,30,1000  
    S2(config-if)#end  
    
d.	Проверьте транки, native VLAN и разрешенные VLAN через транк.
    
    S1#show interfaces fa0/1 switchport   
    Name: Fa0/1  
    Switchport: Enabled  
    Administrative Mode: trunk  
    Operational Mode: trunk  
    Administrative Trunking Encapsulation: dot1q  
    Operational Trunking Encapsulation: dot1q  
    Negotiation of Trunking: Off  
    Access Mode VLAN: 1 (default)  
    Trunking Native Mode VLAN: 1000 (VLAN1000)  
    Voice VLAN: none  
    Administrative private-vlan host-association: none  
    Administrative private-vlan mapping: none  
    Administrative private-vlan trunk native VLAN: none  
    Administrative private-vlan trunk encapsulation: dot1q  
    Administrative private-vlan trunk normal VLANs: none  
    Administrative private-vlan trunk private VLANs: none  
    Operational private-vlan: none  
    Trunking VLANs Enabled: 10,20,30,1000  
    Pruning VLANs Enabled: 2-1001  
    Capture Mode Disabled  
    Capture VLANs Allowed: ALL  
    Protected: false  
    Unknown unicast blocked: disabled  
    Unknown multicast blocked: disabled  
    Appliance trust: none  

### Шаг 2. Вручную настройте магистральный интерфейс F0/5 на коммутаторе S1.  
a.	Настройте интерфейс S1 F0/5 с теми же параметрами транка, что и F0/1. Это транк до маршрутизатора.
      
      S1(config)#interface fa0/5  
      S1(config-if)#switchport mode trunk  
      S1(config-if)#switchport trunk native vlan 1000  
      S1(config-if)#switchport trunk allowed vlan 10,20,30,1000  
      S1(config-if)#end 
      
b.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
      
      S1#copy running-config startup-config  
      S2#copy running-config startup-config
      
c.	Проверка транкинга.
    
    S1#show interfaces trunk   
    Port        Mode         Encapsulation  Status        Native vlan  
    Fa0/1       on           802.1q         trunking      1000  

    Port        Vlans allowed on trunk  
    Fa0/1       10,20,30,1000  

    Port        Vlans allowed and active in management domain  
    Fa0/1       10,20,30,1000  

    Port        Vlans in spanning tree forwarding state and not pruned  
    Fa0/1       10,20,30,1000  


Вопрос:
Что произойдет, если G0/0/1 на R1 будет отключен?  
`Если G0/0/1 будет отключен, шлюз по умолчанию для S1, S2, РС-А, РС-В будет недоступен,`
