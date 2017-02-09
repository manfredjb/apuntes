##Reiniciar contraseña de 

C:\Program Files\MySQL\MySQL Server 5.7\bin>mysqld --init-file=C:\\tmp\\mysql-in
it.txt

C:\Program Files\MySQL\MySQL Server 5.7\bin>mysql -root -p
Enter password:
^C
C:\Program Files\MySQL\MySQL Server 5.7\bin>mysql -u root -p
Enter password: *****
ERROR 2003 (HY000): Can't connect to MySQL server on 'localhost' (10060)

C:\Program Files\MySQL\MySQL Server 5.7\bin>ping localhost

Pinging 10g.oracle.com [10.28.165.67] with 32 bytes of data:
Request timed out.
Request timed out.
Request timed out.
Request timed out.

Ping statistics for 10.28.165.67:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

C:\Program Files\MySQL\MySQL Server 5.7\bin>ping localhost

Pinging MJUAREZ.cr.pwc [::1] with 32 bytes of data:
Reply from ::1: time<1ms
Reply from ::1: time<1ms
Reply from ::1: time<1ms
Reply from ::1: time<1ms

Ping statistics for ::1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\Program Files\MySQL\MySQL Server 5.7\bin>mysql -u root -p
Enter password: *****
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.12-log MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>





----------------------------







UPDATE mysql.user
    SET authentication_string = PASSWORD('admin'), password_expired = 'N'
    WHERE User = 'root';
FLUSH PRIVILEGES;

UPDATE mysql.user SET host = '192.168.200.245' WHERE user = 'root';
UPDATE mysql.db SET host = '192.168.200.245' WHERE user = 'root';
FLUSH PRIVILEGES;
