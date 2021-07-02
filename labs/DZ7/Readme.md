### Домашнее задание №7
### Лабораторная работа. Развертывание коммутируемой сети с резервными каналами
	
#### Топология

 ![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ7/%D1%82%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F%20%D0%B4%D0%B77.bmp)
 
#### Таблица адресации
Устройство |	Интерфейс |	IP-адрес	| Маска подсети 
---|---|---|---
S1	| VLAN 1 |	192.168.1.1 |	255.255.255.0 
S2	| VLAN 1 |  192.168.1.2 |	255.255.255.0 
S3	| VLAN 1 |	192.168.1.3 |	255.255.255.0 
	
  #### Цели  
Часть 1. Создание сети и настройка основных параметров устройства   
Часть 2. Выбор корневого моста  
Часть 3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов  
Часть 4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов  

### Часть 1.Создание сети и настройка основных параметров устройства.  
#### Шаг 1.Создайте сеть согласно топологии.  
Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.

 ![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ7/%D1%82%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F%20%D0%A0%D0%A2%20%D0%B4%D0%B77.bmp)  
#### Шаг 2.	Выполните инициализацию и перезагрузку коммутаторов.  
#### Шаг 3.	Настройте базовые параметры каждого коммутатора.  
a.	Отключите поиск DNS.  
b.	Присвойте имена устройствам в соответствии с топологией.  
c.	Назначьте class в качестве зашифрованного пароля доступа к привилегированному режиму.  
d.	Назначьте cisco в качестве паролей консоли и VTY и активируйте вход для консоли и VTY каналов.  
e.	Настройте logging synchronous для консольного канала.  
f.	Настройте баннерное сообщение дня (MOTD) для предупреждения пользователей о запрете несанкционированного доступа.  
g.	Задайте IP-адрес, указанный в таблице адресации для VLAN 1 на всех коммутаторах.  
h.	Скопируйте текущую конфигурацию в файл загрузочной конфигурации.  

	Switch>  
	Switch>en  
	Switch#configure terminal   
	Enter configuration commands, one per line.  End with CNTL/Z.  
	Switch(config)#hostname S1  
	S1(config)#no ip domain-lookup  
	S1(config)#enable secret class  
	S1(config)#line console 0  
	S1(config-line)#password cisco  
	S1(config-line)#logging synchronous   
	S1(config-line)#exit  
	S1(config)#line vty 0 15  
	S1(config-line)#password cisco  
	S1(config-line)#login  
	S1(config-line)#exit  
	S1(config)#banner motd @Autorized Access Only@  
	S1(config)#int vlan1  
	S1(config-if)#ip address 192.168.1.1 255.255.255.0  
	S1(config-if)#no shutdown   

	S1(config-if)#  
	%LINK-5-CHANGED: Interface Vlan1, changed state to up  

	%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up  
	S1(config-if)#exit  
	S1(config)#^Z  
	S1#  

	S1#copy running-config startup-config   
	Destination filename [startup-config]?   
	Building configuration...  
	[OK]  
	S1#  
	
	Switch>  
	Switch>enable     
	Switch#configure terminal     
	Enter configuration commands, one per line.  End with CNTL/Z.  
	Switch(config)#hostname S2  
	S2(config)#no ip domain-lookup   
	S2(config)#enable secret class   
	S2(config)#line console 0  
	S2(config-line)#password cisco  
	S2(config-line)#logging synchronous   
	S2(config-line)#exit  
	S2(config)#line vty 0 15  
	S2(config-line)#password cisco  
	S2(config-line)#login  
	S2(config-line)#exit  
	S2(config)#banner motd @Autorizaition Acceess Only@  
	S2(config)#int vlan1  
	S2(config-if)#ip address 192.168.1.2 255.255.255.0  
	S2(config-if)#no shutdown 
	
	S2(config-if)#  
	%LINK-5-CHANGED: Interface Vlan1, changed state to up  

	%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up  

	S2(config-if)#  
	S2(config-if)#exit  
	S2(config)#^Z  
	S2#  
	%SYS-5-CONFIG_I: Configured from console by console  

	S2#copy running-config startup-config   
	Destination filename [startup-config]?   
	Building configuration...  
	[OK]  
	
	Switch>  
	Switch>enable   
	Switch#conf terminal   
	Enter configuration commands, one per line.  End with CNTL/Z.  
	Switch(config)#hostname S3  
	S3(config)#no ip domain-lookup  
	S3(config)#enable secret class  
	S3(config)#line console 0  
	S3(config-line)#password cisco  
	S3(config-line)#logging synchronous   
	S3(config-line)#exit  
	S3(config)#line vty 0 15  
	S3(config-line)#password cisco  
	S3(config-line)#login  
	S3(config-line)#exit  
	S3(config)#banner motd @Autorization Acceess Only@  
	S3(config)#int vlan1  
	S3(config-if)#ip address 192.168.1.3 255.255.255.0  
	S3(config-if)#no shutdown 

	S3(config-if)#  
	%LINK-5-CHANGED: Interface Vlan1, changed state to up  

	%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up  

	S3(config-if)#  
	S3(config-if)#exit  
	S3(config)#^Z  
	S3#  
	%SYS-5-CONFIG_I: Configured from console by console  

	S3#copy running-config startup-config   
	Destination filename [startup-config]?   
	Building configuration...  
	[OK]  
	S3#  

#### Шаг 4:	Проверьте связь.  
Проверьте способность компьютеров обмениваться эхо-запросами.  
Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S2?	______________
	
	Autorized Access Only  

	S1>ping 192.168.1.2  

	Type escape sequence to abort.  
	Sending 5, 100-byte ICMP Echos to 192.168.1.2, timeout is 2 seconds:  
	..!!!  
	Success rate is 60 percent (3/5), round-trip min/avg/max = 0/0/0 ms  
	
Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S3?	______________  

	S1>ping 192.168.1.3  

	Type escape sequence to abort.  
	Sending 5, 100-byte ICMP Echos to 192.168.1.3, timeout is 2 seconds:  
	..!!!  
	Success rate is 60 percent (3/5), round-trip min/avg/max = 0/0/0 ms  

Успешно ли выполняется эхо-запрос от коммутатора S2 на коммутатор S3?	______________  
	
	Autorizaition Acceess Only  

	S2>ping 192.168.1.3  

	Type escape sequence to abort.  
	Sending 5, 100-byte ICMP Echos to 192.168.1.3, timeout is 2 seconds:  
	..!!!  
	Success rate is 60 percent (3/5), round-trip min/avg/max = 0/0/0 ms  
