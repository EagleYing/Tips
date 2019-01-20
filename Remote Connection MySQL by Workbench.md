# Remote Connection MySQL by Workbench

### Basics

Firstly you need MySQL server and MySQLWorkbench, I suggest a quite stable version [MySQL 5.7](https://dev.mysql.com/downloads/mysql/5.7.html#downloads). As for Workbench I suggest [MySQLWorkbench 6.3](https://downloads.mysql.com/archives/workbench/) because higher version may cause compatibility issues.

### Configure remote connection for MySQL

Edit `mysql.cnf`, I found `mysqld.cnf` under`/etc/mysql/mysql.conf.d`, find `bind-address = 127.0.0.1` and comment it out

`#bind-address = 127.0.0.1`

On your server, `mysql -u username -p` 

Add new mysql user

`GRANT ALL ON *.* to user@'IP' IDENTIFIED BY 'password'; `

'IP' I suggest use '%' which means allow all ip connections. Then refresh system

`FLUSH PRIVILEGES;`

`exit` and restart the mysql service

`sudo service mysql restart `

### Operate Workbench

Click the plus button on the home page, fill in the server IP address, username, password(your mysql password on server), wish you success!