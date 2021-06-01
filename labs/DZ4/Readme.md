### Домашнее задание №4 
### Лабораторная работа. Настройка IPv6 адресов на сетевых устройствах. 
#### Топология.
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ4/%D1%82%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F%20%D0%B4%D0%B74.bmp)
#### Таблица адресации
Устройство |интерфейс| IPv6-адрес|длина префикса|шлюз по умолчанию|
---|---|---|---|---
R1|G0/0/0|2001:db8:acad:a::1 |64|-
R1|G0/0/1|2001:db8:acad:1::1 |64|-
S1|VLAN1|2001:db8:acad:1::b|64|-
PC-A|NIC|2001:db8:acad:1::3|64|fe80::1
PC-B|NIC|2001:db8:acad:a::3|64|fe80::1
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
