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
