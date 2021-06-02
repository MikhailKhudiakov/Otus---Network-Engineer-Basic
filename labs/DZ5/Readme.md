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

### Часть 3. Настройка коммутатора для доступа по протоколу SSH
#### Шаг 1. Настройте основные параметры коммутатора.
Откройте окно конфигурации
a.	Подключитесь к коммутатору с помощью консольного подключения и активируйте привилегированный режим EXEC.
```
Switch>enable
```
b.	Войдите в режим конфигурации.
```
Switch#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
```
c.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.
```
Switch(config)#no ip domain-lookup 
```
d.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
```
Switch(config)#enable secret class
```
e.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.
```
Switch(config)#line console 0
Switch(config-line)#password cisco
Switch(config-line)#login 
Switch(config-line)#exit
```
f.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.
```
Switch(config)#line vty 0 15
Switch(config-line)#password cisco
Switch(config-line)#login
Switch(config-line)#exit
```
g.	Зашифруйте открытые пароли.
```
Switch(config)#service password-encryption 
```
h.	Создайте баннер, который предупреждает о запрете несанкционированного доступа.
```
Switch(config)#banner motd ^ Authorized Access Only!^
```
i.	Настройте и активируйте на коммутаторе интерфейс VLAN 1, используя информацию, приведенную в таблице адресации.
 ```
Switch(config)#int vlan1
Switch(config-if)#ip address 192.168.1.11 255.255.255.0
Switch(config-if)#no shutdown
%LINK-5-CHANGED: Interface Vlan1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
%IP-4-DUPADDR: Duplicate address 192.168.1.11 on Vlan1, sourced by 0001.97B2.973C

Switch(config)#ip default-gateway 192.168.1.1
```
j.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
```
Switch#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
### Шаг 2. Настройте коммутатор для соединения по протоколу SSH.
a.	Настройте имя устройства, как указано в таблице адресации.
```
Switch(config)#hostname S1
```
b.	Задайте домен для устройства.
```
S1(config)#ip domain-name domain-name.com
```
c.	Создайте ключ шифрования с указанием его длины.
```
S1(config)#crypto key generate rsa
The name for the keys will be: S1.domain-name.com
Choose the size of the key modulus in the range of 360 to 2048 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]

```
d.	Создайте имя пользователя в локальной базе учетных записей.
```
S1(config)#username admin secret Adm1nP@55
*Mar 1 0:29:48.346: %SSH-5-ENABLED: SSH 1.99 has been enabled
```
e.	Активируйте протоколы Telnet и SSH на линиях VTY.
```
S1(config)#line vty 0 15
S1(config-line)#transport input telnet
S1(config-line)#transport input ssh
```
f.	Измените способ входа в систему таким образом, чтобы использовалась проверка пользователей по локальной базе учетных записей.
```
S1(config-line)#login local
```
#### Шаг 3. Установите соединение с коммутатором по протоколу SSH.
Запустите программу Tera Term на PC-A, затем установите подключение по протоколу SSH к интерфейсу SVI коммутатора S1.
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ5/ssh%20S1.bmp)
Вопрос:
Удалось ли вам установить SSH-соединение с коммутатором?
### Часть 4. Настройка протокола SSH с использованием интерфейса командной строки (CLI) коммутатора
#### Шаг 1. Посмотрите доступные параметры для клиента SSH в Cisco IOS.
S1#ssh ?
  -l  Log in using this user name
  -v  Specify SSH Protocol Version

#### Шаг 2. Установите с коммутатора S1 соединение с маршрутизатором R1 по протоколу SSH.
a.	Чтобы подключиться к маршрутизатору R1 по протоколу SSH, введите команду –l admin. 
```
S1#ssh -l admin 192.168.1.1

Password: 

 Authorize personel only

R1>
```
b.	Чтобы вернуться к коммутатору S1, не закрывая сеанс SSH с маршрутизатором R1, нажмите комбинацию клавиш Ctrl+Shift+6. Отпустите клавиши Ctrl+Shift+6 и нажмите x.
```
R1>
S1#
```
c.	Чтобы вернуться к сеансу SSH на R1, нажмите клавишу Enter в пустой строке интерфейса командной строки. Чтобы увидеть окно командной строки маршрутизатора, нажмите клавишу Enter еще раз.
```
S1#
[Resuming connection 1 to 192.168.1.1 ... ]

R1>
```
d.	Чтобы завершить сеанс SSH на маршрутизаторе R1, введите в командной строке маршрутизатора команду exit.
```
R1>exit

[Connection to 192.168.1.1 closed by foreign host]
S1#
```
Вопрос:
Какие версии протокола SSH поддерживаются при использовании интерфейса командной строки?
```
S1#show ip ssh
SSH Enabled - version 1.5

R1#show ip ssh
SSH Enabled - version 2.0
Authentication timeout: 120 secs; Authentication retries: 3

```
Закройте окно настройки.
	Вопрос для повторения
Как предоставить доступ к сетевому устройству нескольким пользователям, у каждого из которых есть собственное имя пользователя?

