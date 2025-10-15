# Домашнее задание к занятию «Уязвимости и атаки на информационные системы - Павлов Дмитрий  

## Задание 1. 
Скачайте и установите виртуальную машину Metasploitable: https://sourceforge.net/projects/metasploitable/.  
Это типовая ОС для экспериментов в области информационной безопасности, с которой следует начать при анализе уязвимостей.  
Просканируйте эту виртуальную машину, используя nmap.  
Попробуйте найти уязвимости, которым подвержена эта виртуальная машина.  
Сами уязвимости можно поискать на сайте https://www.exploit-db.com/.  
Для этого нужно в поиске ввести название сетевой службы, обнаруженной на атакуемой машине, и выбрать подходящие по версии уязвимости.  
Ответьте на следующие вопросы:  
- Какие сетевые службы в ней разрешены?  
- Какие уязвимости были вами обнаружены? (список со ссылками: достаточно трёх уязвимостей)  
Приведите ответ в свободной форме. 
## Решение  
## Службы которые разрешены:  
21/tcp   open  ftp         vsftpd 2.3.4  
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)  
23/tcp   open  telnet      Linux telnetd  
25/tcp   open  smtp        Postfix smtpd  
53/tcp   open  domain      ISC BIND 9.4.2  
80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)  
111/tcp  open  rpcbind     2 (RPC #100000)  
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)  
445/tcp  open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)  
512/tcp  open  exec        netkit-rsh rexecd  
513/tcp  open  login       OpenBSD or Solaris rlogind  
514/tcp  open  shell       Netkit rshd  
1099/tcp open  java-rmi    GNU Classpath grmiregistry  
1524/tcp open  bindshell   Metasploitable root shell  
2049/tcp open  nfs         2-4 (RPC #100003)   
2121/tcp open  ftp         ProFTPD 1.3.1   
3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5  
5432/tcp open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7  
5900/tcp open  vnc         VNC (protocol 3.3)  
6667/tcp open  irc         UnrealIRCd  
8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)  
8180/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1  
## Обнаруженные уязвимости:  
vsftpd 2.3.4 - Backdoor Command Execution ( https://www.exploit-db.com/exploits/49757 )  
Samba 3.0.20 < 3.0.25rc3 - 'Username' map script' Command Execution ( https://www.exploit-db.com/exploits/16320 )   
Samba < 3.0.20 - Remote Heap Overflow ( https://www.exploit-db.com/exploits/7701 )  
ProFTPd 1.2 < 1.3.0 (Linux) - 'sreplace' Remote Buffer Overflow (Metasploit) ( https://www.exploit-db.com/exploits/16852 )  
ProFTPd IAC 1.3.x - Remote Command Execution ( https://www.exploit-db.com/exploits/15449 )  
MySQL 5.0.x - IF Query Handling Remote Denial of Service ( https://www.exploit-db.com/exploits/30020 )  
MySQL 5.0.x - Single Row SubSelect Remote Denial of Service ( https://www.exploit-db.com/exploits/29724 )  
PostgreSQL 8.2/8.3/8.4 - UDF for Command Execution ( https://www.exploit-db.com/exploits/7855 )  
Apache 1.4/2.2.x - APR 'apr_fnmatch()' Denial of Service ( https://www.exploit-db.com/exploits/35738 )  
Apache Tomcat < 5.5.17 - Remote Directory Listing ( https://www.exploit-db.com/exploits/2061 )   
## Задание 2.  
Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.  
Запишите сеансы сканирования в Wireshark.  
Ответьте на следующие вопросы:  
- Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?  
- Как отвечает сервер?  
Приведите ответ в свободной форме.    
## Решение  
## Сканирование через nmap  
SYN  
![скриншот к заданию 2](/pic/pic01.png) 
FIN  
![скриншот к заданию 2](/pic/pic02.png)  
Xmas  
![скриншот к заданию 2](/pic/pic03.png)  
UDP (устанавливал дополнительные флаги -T4 шаблон установок времени (агрессивный) -F (быстрое сканирование))  
![скриншот к заданию 2](/pic/pic04.png)  
## Чем отличатся эти режимы сканирование:  
SYN: Отправляет пакет с флагом SYN (как первая часть рукопожатия). Самый быстрый и скрытный.
FIN: Отправляет пакет с флагом FIN (как будто завершает соединение). Пытается обойти простые фаерволы.
Xmas: Отправляет пакет с тремя флагами сразу: FIN, PSH, URG. Нестандартный пакет.
UDP: Отправляет пустые UDP-пакеты (без флагов). Работает медленнее, потому что ждет ответа. (устанавливал дополнительные флаги -T4 шаблон установок времени (агрессивный) -F (быстрое сканирование))
## Как отвечает сервер:  
Для SYN открытый порт отвечает SYN-ACK.
Для FIN и Xmas открытый порт молчит.
Для SYN и FIN закрытый порт — RST.
Для UDP открытый — тишина или сервисный ответ, закрытый — ICMP Port Unreachable.
