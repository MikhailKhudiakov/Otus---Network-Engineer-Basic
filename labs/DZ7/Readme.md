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
 Сеть с требуемой топологией, созданная в PacketTracer.  
 ![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ7/%D1%82%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F%20%D0%A0%D0%A2%20%D0%B4%D0%B77.bmp)  
 Создание сети с требуемой топологией средствами лабораторной установки Termilab.  
 a.Исходная сеть:
 ![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ7/termlab%20%D1%82%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F.bmp)  
 b.Сеть, приведенная к требуемому виду, путем отключения ненужных элементов и переименования коммутаторов:
 ![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ7/%D0%BF%D1%80%D0%B8%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D0%BD%D0%B0%D1%8F%20termlab%20%D1%82%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F%20S1%2C2%2C3.bmp)  
 Примечание. Для выполнения лабораторной работы использовалась сеть лаборатории Termilab, так как функции протокола STP не работают в PacketTracer должным образом.  
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

### Часть 2:	Определение корневого моста

#### Шаг 1:	Отключите все порты на коммутаторах.
	S1(config)#interface range fa0/1-24,g0/1-2  
	S1(config-if-range)#shutdown   


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

	%LINK-5-CHANGED: Interface FastEthernet0/18, changed state to administratively down

	%LINK-5-CHANGED: Interface FastEthernet0/19, changed state to administratively down

	%LINK-5-CHANGED: Interface FastEthernet0/20, changed state to administratively down

	%LINK-5-CHANGED: Interface FastEthernet0/21, changed state to administratively down

	%LINK-5-CHANGED: Interface FastEthernet0/22, changed state to administratively down

	%LINK-5-CHANGED: Interface FastEthernet0/23, changed state to administratively down

	%LINK-5-CHANGED: Interface FastEthernet0/24, changed state to administratively down

	%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to administratively down

	%LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to administratively down
	S1(config-if-range)#
	%LINK-5-CHANGED: Interface FastEthernet0/1, changed state to administratively down

	%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to down

	%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to administratively down

	%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/2, changed state to down

	%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down

	%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/3, changed state to down

	%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to administratively down

	%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/4, changed state to down

	%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to down

	S1(config-if-range)#end
	
	
	
#### Шаг 2:	Настройте подключенные порты в качестве транковых.

	S3(config)#int range f0/1-24,g0/1-2  
	S3(config-if-range)#switchport mode trunk  
	S3(config-if-range)#end  

	S1(config)#int range f0/1-24,g0/1-2  
	S1(config-if-range)#switchport mode trunk  
	S1(config-if-range)#end  

	S2(config)#int range f0/1-24,g0/1-2  
	S2(config-if-range)#switchport mode trunk  
	S2(config-if-range)#end  
	S2#  
#### Шаг 3:	Включите порты F0/2 и F0/4 на всех коммутаторах.
	
	S1(config)#int range f0/2,f0/4  
	S1(config-if-range)#no shutdown  

	%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to down  
	S1(config-if-range)#  
	%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to up  

	%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/2, changed state to up  

	%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up  

	S1(config-if-range)#end  
	S1#  

	S3(config)#int range f0/2,f0/4  
	S3(config-if-range)#no shutdown  

	S3(config-if-range)#  
	%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to up  

	%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/2, changed state to up  

	%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up  

	%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to up  

	%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/4, changed state to up  

	S3(config-if-range)#  
	
#### Шаг 4:	Отобразите данные протокола spanning-tree.

	S1#show spanning-tree   
	VLAN0001  
  	Spanning tree enabled protocol ieee  
  	Root ID    Priority    32769  
             	Address     1c1d.8686.1d80    
             	This bridge is the root  
             	Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
  	Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)  
             	Address     1c1d.8686.1d80  
             	Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
             	Aging Time  15  sec  
	Interface           Role Sts Cost      Prio.Nbr Type  
	------------------- ---- --- --------- -------- --------------------------------  
	Fa0/2               Desg FWD 19        128.2    P2p   
	Fa0/4               Desg FWD 19        128.4    P2p   

	
	S2#show spanning-tree   
	VLAN0001  
 	Spanning tree enabled protocol ieee  
 	Root ID    Priority    32769  
             	Address     1c1d.8686.1d80  
             	Cost        19  
            	 Port        2 (FastEthernet0/2)  
            	 Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
 	Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)  	
             	Address     c025.5c31.8c80  
             	Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
             	Aging Time  300 sec  
	Interface           Role Sts Cost      Prio.Nbr Type  
	------------------- ---- --- --------- -------- --------------------------------  
	Fa0/2               Root FWD 19        128.2    P2p   
	Fa0/4               Desg FWD 19        128.4    P2p   
	S2#  
	
	S3#show spanning-tree 		
	VLAN0001  
  	Spanning tree enabled protocol ieee  
 	Root ID    Priority    32769  
             	Address     1c1d.8686.1d80  
             	Cost        19  
             	Port        4 (FastEthernet0/4)  
             	Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
  	Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)  
            	 Address     ecc8.82dd.4980  
             	Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
             	Aging Time  15  sec  
	Interface           Role Sts Cost      Prio.Nbr Type  
	------------------- ---- --- --------- -------- --------------------------------  
	Fa0/2               Altn BLK 19        128.2    P2p   
	Fa0/4               Root FWD 19        128.4    P2p   
	

В схему ниже запишите роль и состояние (Sts) активных портов на каждом коммутаторе в топологии.
 
  ![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ7/%D1%81%D1%85%D0%B5%D0%BC%D0%B0%20%D1%81%20%D0%9C%D0%90%D0%A1.bmp) 
  
Какой коммутатор является корневым мостом? 
	
	Коммутатор S1 является корневым мостом.  
	
Почему этот коммутатор был выбран протоколом spanning-tree в качестве корневого моста?

	Коммутатор S1 был выбран корневым мостом, т.к. имеет самый низкий MAC- адрес при равенстве остальных параметров на всех 
	коммутаторах (приоритетов моста и расширенного идентификатора системы - номера VLAN -priority 32768 sys-id-ext 1).

Какие порты на коммутаторе являются корневыми портами? 
	
	Корневыми являются порты S2 Fa0/2 и S3 Fa0/4, которые имеют наименьшую стоимость пути к корневому мосту.

Какие порты на коммутаторе являются назначенными портами? 

	Назначенными портами являются оба порта корневого коммутатора S1 и порт Fa0/4 S2
	
Какой порт отображается в качестве альтернативного и в настоящее время заблокирован? 

	Порт Fa0/2 S3 выбран альтернативным и заблокирован.
	
Почему протокол spanning-tree выбрал этот порт в качестве невыделенного (заблокированного) порта?

	Порт Fa0/2 S3 выбран альтернативным, т.к. при равенстве стоимости пути к корневому мосту и BID коммутатор S3 имеет МАС-адрес
	выше, чем коммутатор S2.
### Часть 3

#### Шаг 1:	Определите коммутатор с заблокированным портом.

	S3#show spanning-tree 		
	VLAN0001  
  	Spanning tree enabled protocol ieee  
 	Root ID    Priority    32769  
             	Address     1c1d.8686.1d80  
             	Cost        19  
             	Port        4 (FastEthernet0/4)  
             	Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
  	Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)  
            	 Address     ecc8.82dd.4980  
             	Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
             	Aging Time  15  sec  
	Interface           Role Sts Cost      Prio.Nbr Type  
	------------------- ---- --- --------- -------- --------------------------------  
	Fa0/2               Altn BLK 19        128.2    P2p   
	Fa0/4               Root FWD 19        128.4    P2p   
	
#### Шаг 2:	Измените стоимость порта.

Помимо заблокированного порта, единственным активным портом на этом коммутаторе является порт,
выделенный в качестве порта корневого моста. Уменьшите стоимость этого порта корневого моста до 18,
выполнив команду spanning-tree cost 18 режима конфигурации интерфейса.
		
	S3(config)#int fa0/2  
	S3(config-if)#spanning-tree cost 18  
	S3(config-if)#end
	
#### Шаг 3:	Просмотрите изменения протокола spanning-tree.

	S3#show spanning-tree   
	VLAN0001  
  	Spanning tree enabled protocol ieee  
  	Root ID    Priority    32769  
             Address     1c1d.8686.1d80    
             Cost        18    
             Port        4 (FastEthernet0/4)    
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec     
  	Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)    
             Address     ecc8.82dd.4980       
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec      
             Aging Time  15  sec    
	Interface           Role Sts Cost      Prio.Nbr Type  
	------------------- ---- --- --------- -------- --------------------------------  
	Fa0/2               Desg LIS 19        128.2    P2p   
	Fa0/4               Root FWD 18        128.4    P2p     
	S3#
	
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ7/%D0%A7%D0%B0%D1%81%D1%82%D1%8C%203%20%D1%88%D0%B0%D0%B3%203_1.bmp)
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ7/%D0%A7%D0%B0%D1%81%D1%82%D1%8C%203%20%D1%88%D0%B0%D0%B3%203_2.bmp)
![](https://github.com/MikhailKhudiakov/Otus---Network-Engineer-Basic/blob/main/labs/DZ7/%D0%A7%D0%B0%D1%81%D1%82%D1%8C%203%20%D1%88%D0%B0%D0%B3%203_3.bmp)
Почему протокол spanning-tree заменяет ранее заблокированный порт на назначенный порт и блокирует порт, который был назначенным портом на другом коммутаторе?
	
	Протокол STP заменяет и блокирует ранее назначенный порт, т.к. появляется порт с меньшей стоимотью пути к корневому мосту.


