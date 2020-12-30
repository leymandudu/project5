# MYQSL SERVER-CLIENT PROJECT
____
## Step-by-step Procedure
___

* Installed Linux Server for both Client and Server

* Installed mysql on Server
    * Installing MySQL 8.0 on CentOS 8
    * package manager as root or user with sudo privileges:

    ```
    sudo dnf install @mysql
    ```
    
>   The @mysql module installs MySQL and all dependencies.

![INSTALLING MYSQL](/Users/abuhaneefah/Desktop/installingsql.png "INSTALLING MYSQL")


* Once the installation is completed, I start the MySQL service and enabled it to automatically start on boot by running the following command:

```
sudo systemctl enable --now mysqld
```
![SYSTEMCTL ENABLE OUTPUT](/Users/abuhaneefah/Desktop/systemctlenable.png "SYSTEMCTL ENABLE OUTPUT")

* To check whether the MySQL server is running, I used:

```
sudo systemctl status mysqld
```

> Below is the output:

```
mysqld.service - MySQL 8.0 database server
Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
Active: active (running) since Thu 2020-12-24 22:09:39 EST; 15s ago
...
```

* Securing MySQL: To secure the mysql server, I ran the mysql_secure_installation script that performs several security-related operations and sets the MySQL root password:

```
sudo mysql_secure_installation
```

> I followed the system prompt to complete the secure system validation process.

![SECURE SETTING](/Users/abuhaneefah/Desktop/secure-setting.png "SECURE SETTING")


* Perform restart with

```
sudo systemctl restart mysqld
```

* To access the MYSQL Database, I used the below command

```
mysql -u root -p
```

* Enter the root password

> This is the output:

```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.21 Source distribution

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```

![LOGIN TO MYSQL](/Users/abuhaneefah/Desktop/login-mysql.png "LOGIN TO MYSQL")

* To view the database you’ve created simply issue the following command:
```
SHOW DATABASES;
```
> This is the output;

```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.01 sec)
```

![SHOW DATABASES](/Users/abuhaneefah/Desktop/showdatabases.png "SHOW DATABASES")


## Creating and configuring mysql instance on Server to allow remote connection
____

Configuring the database instance
This section discusses how to create a new database instance. Although a new database instance is recommended.
To configure a MySQL database instance:
* I logged in to the database server as root or any user.

* Entered the MySQL root user’s password when prompted.

* Entered the following commands in the order shown to create a database instance named project5 with username leye:

1. Create new database
```
create database project5;
```
> Output:
```
Query OK, 1 row affected (0.05 sec)
```
2. Create a new user

```
create user 'leye'@'localhost' IDENTIFIED BY 'p@ssw0rd';
```
> Output:
```
Query OK, 0 rows affected (0.02 sec)
```

3. Grant access to the new user on the database
```
GRANT ALL ON project5.* TO 'leye'@'localhost';
```
> Output:
```
Query OK, 0 rows affected (0.02 sec)
```

4. Flush privilege
```
FLUSH PRIVILEGES;
```
> Output:
```
Query OK, 0 rows affected (0.01 sec)
```

* Enter exit to quit the command prompt.

* To view the database you’ve created simply issue the following command:
```
SHOW DATABASES;
```
> This is the output;

```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| project5           |
| sys                |
+--------------------+
5 rows in set (0.00 sec)
```

* Verify the database:
```
mysql -u leye -p
```

## Granting remote access to the mysql Server user on the Client Server
___
* Login the the msql server as root
```
mysql -u root -p
```

* Grant permission to the desired user to use for remote connection on the client server
```
GRANT ALL PRIVILEGES ON *.* TO ‘leye’@‘192.168.1.69’;
```

* Use the flush privilege command 
```
FLUSH PRIVILEGES;
```

* Ping both server and client IP to confirm connectivity
```
ping 192.168.1.68
```

![PINGING SERVER FROM CLIENT](/Users/abuhaneefah/Desktop/pingingserver.png "PINGING SERVER FROM CLIENT")

```
ping 192.168.1.69
```

![PINGING CLIENT FROM SERVER](/Users/abuhaneefah/Desktop/pingingremote.png "PINGING CLIENT FROM SERVER")

## Remotely Connecting to from the Client Server
___

* After fulfilling all configuration processes, below is the output of the remote connection to the main server;
```
[root@serverB-mysql-client ~]# mysql -u leye -h 192.168.1.68 -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 17
Server version: 8.0.21 Source distribution

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| project5           |
| sys                |
+--------------------+
5 rows in set (0.01 sec)

mysql> 
```

![SHOW DATABASE ON REMOTE](/Users/abuhaneefah/Desktop/showdbonremote.png "SHOW DATABASE ON REMOTE")

## Issues Encountered and Resolution

___

1. I was unable to grant access to both root and my created user to access the server remotely.

> Error message below:
```
ERROR 1410 (42000): You are not allowed to create a user with GRANT
```

2. I modified the /etc/my.cnf to include **bind-address**

```
bind-address    0.0.0.0
```
This allow the server to accept connection from any IP address.

3. Added the destination port to the **IP-Table** of the Server.

```
sudo iptables -A INPUT -p tcp --destination-port 3306 -j ACCEPT
```

4. Added the destination port to the **Firewall** to allow TCP connection.

```
sudo firewall-cmd --permanent --zone=public --add-port=3306/tcp
```

5. Reloaded the **Firewall** to implement the new settings.
```
sudo firewall-cmd --reload
```

6. Then restarted the **mysql server** and connect remotely.