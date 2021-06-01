### Домашнее задание №5 
### Лабораторная работа. Доступ к сетевым устройствам по протоколу SSH.
#### Топология.
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ5/%D1%82%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F.bmp)
#### Таблица адресации
Устройство |интерфейс| IPv-адрес|маска подсети|шлюз по умолчанию|
---|---|---|---|---
R1|G0/0/1|192.168.1.1|255.255.255.0	|-|
S1|VLAN1|192.168.1.11|255.255.255.0|192.168.1.1
PC-A|NIC|192.168.1.3|255.255.255.0|192.168.1.1

### 	Задачи
  Часть 1. Настройка основных параметров устройства
  
  Часть 2. Настройка маршрутизатора для доступа по протоколу SSH
  
  Часть 3. Настройка коммутатора для доступа по протоколу SSH
  
  Часть 4. SSH через интерфейс командной строки (CLI) коммутатора

### Часть 1. Настройка основных параметров устройств
#### Шаг 1. Создайте сеть согласно топологии.
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ5/%D1%82%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F-2.bmp)
#### Шаг 2. Выполните инициализацию и перезагрузку маршрутизатора и коммутатора.
```
Router#show flash

System flash directory:
File  Length   Name/status
  3   486899872isr4300-universalk9.03.16.05.S.155-3.S5-ext.SPA.bin
  2   28282    sigdef-category.xml
  1   227537   sigdef-default.xml
[487155691 bytes used, 2761893909 available, 3249049600 total]
3.17338e+06K bytes of processor board System flash (Read/Write)

Router#reload
```
#### Шаг 3. Настройте маршрутизатор.
  a.	Подключитесь к маршрутизатору с помощью консоли и активируйте привилегированный режим EXEC.
  
  b.	Войдите в режим конфигурации.
  
  c.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.
  
  d.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
  
  e.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.
  
  f.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.
  
  g.	Зашифруйте открытые пароли.
  
  h.	Создайте баннер, который предупреждает о запрете несанкционированного доступа.
  
```
Router>enable
Router#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#enable secret class
Router(config)#line console 0
Router(config-line)#password cisco
Router(config-line)#login
Router(config-line)#end
Router#
%SYS-5-CONFIG_I: Configured from console by console
Router(config)#line vty 0 15
Router(config-line)#password cisco
Router(config-line)#login
Router(config-line)#end
Router#
%SYS-5-CONFIG_I: Configured from console by console
Router#conf t
Router(config)#service password-encryption
Router(config)#no ip domain-lookup
Router(config)#banner motd @ Unauthorized access is strictly prohibited @
```
i.	Настройте и активируйте на маршрутизаторе интерфейс G0/0/1, используя информацию, приведенную в таблице адресации.
```
Router(config)#int g0/0/1
Router(config-if)#ip address 192.168.1.11 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up
```
j.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
```
Router#copy  running-config startup-config 
```
#### Шаг 4. Настройте компьютер PC-A.
a.	Настройте для PC-A IP-адрес и маску подсети.
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ5/ip%20PC-A.bmp)

b.	Настройте для PC-A шлюз по умолчанию.
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ5/def%20gateway.bmp)

#### Шаг 5. Проверьте подключение к сети.
Пошлите с PC-A команду Ping на маршрутизатор R1. Если эхо-запрос с помощью команды ping не проходит, найдите и устраните неполадки подключения.
```
C:\>ping 192.168.1.1

Pinging 192.168.1.1 with 32 bytes of data:

Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.1.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
### Часть 2. Настройка маршрутизатора для доступа по протоколу SSH
#### Шаг 1. Настройте аутентификацию устройств.
При генерации ключа шифрования в качестве его части используются имя устройства и домен. Поэтому эти имена необходимо указать перед вводом команды crypto key.
Откройте окно конфигурации
a.	Задайте имя устройства.
```
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname R1
```
b.	Задайте домен для устройства.
```
R1(config)#ip domain-name domain-name.com
```
#### Шаг 2. Создайте ключ шифрования с указанием его длины.
```
R1(config)#crypto key generate RSA
The name for the keys will be: R1.domain-name.com
Choose the size of the key modulus in the range of 360 to 2048 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
```
#### Шаг 3. Создайте имя пользователя в локальной базе учетных записей.
```
R1(config)#username admin secret Adm1nP@55 
*Mar 2 2:7:35.902: %SSH-5-ENABLED: SSH 1.99 has been enabled
```
#### Шаг 4. Активируйте протокол SSH на линиях VTY.
a.	Активируйте протоколы Telnet и SSH на входящих линиях VTY с помощью команды transport input.
```
R1(config)#line vty 0 15
R1(config-line)#transport input telnet
R1(config-line)#transport input ssh
```
b.	Измените способ входа в систему таким образом, чтобы использовалась проверка пользователей по локальной базе учетных записей.
```
R1(config-line)#login local 
R1(config-line)#exit
```
#### Шаг 5. Сохраните текущую конфигурацию в файл загрузочной конфигурации.
```
R1#copy running-config startup-config 
```
#### Шаг 6. Установите соединение с маршрутизатором по протоколу SSH.
a.	Запустите Tera Term с PC-A.

b.	Установите SSH-подключение к R1. 

![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ5/ssh.bmp)

