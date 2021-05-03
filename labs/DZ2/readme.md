
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
  а.Настройте имена утройств в соответствии с топологией.
  
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

