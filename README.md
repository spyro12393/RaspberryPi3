# Notes.
###### tags: `Mariadb` `RaspberryPi`

## Basic Linux Command
- `nslookup`: Translate domain name to ip address.
- `netstat`: Showing all activing ports.

# Kali-linux on rp3
## Basics
- Installing OSs. (ARM)
    - [Noob](https://www.raspberrypi.org/downloads/noobs/)
    - [Kali-linux](https://www.offensive-security.com/kali-linux-arm-images/) 
- Listing all online users
```javascript=
//list all online users.
$ w
//kill user.
$ pkill -kill -t [tty]
```

## Installing ssh-server
1. install
```javascript=
$ apt-get install openssh-server
```
2. auto-running ssh
```javascript=
$ update-rc.d -f ssh remove
$ update-rc.d -f ssh defaults
$ update-rc.d -f ssh enable 2 3 4 5
```
3. reconfiguring
```javascript=
rm /etc/ssh/ssh_host_*
$ sudo dpkg-reconfigure openssh-server
```

4. allow root remote
```javascript=
$ nano /etc/ssh/sshd_config
```
- `change PermitRootLogin prohibit-password` to
- `change PermitRootLogin yes`

5. restart

```javascript=
$ service ssh restart
```

6. using other account to login

```javascript=
$ ssh root@[host_ip]
$ [passwd]
```

7. you can change greeting words.

```javascript=
$ nano /etc/motd
```
- remember to restart: `service ssh restart`

8. Install kali-linux-full

```javascript=
$ apt-get update
$ apt-get install kali-linux-full
```

## User autherization
1. `useradd -u [uid] -g [group] [name]`
2. `userdel -r [usrname]`   ([-r] deletes all root directory)
3. Upgrading sudo permission
```javascript=
$ sudo visudo
/*

find: root ALL=(ALL:ALL) ALL or root ALL=(ALL) ALL
insert: [UserName] ALL=(ALL:ALL) ALL or [UserName] ALL=(ALL) ALL

 */
```
:::danger
When updating "sudoers list", there may be a "sudo file crash" problem. When something happens, use:
```javascript=
$ pkexec visudo
```
to try to recover.
:::


## Mariadb
### Basics
- Login:     `mysql -u root -p[passwd]`
- Changing Password:  
```javascript=
use mysql;
update user set password=PASSWORD("your new password") where User='root';
flush privileges;
quit
```


### Inside "Mariadb"
#### 1. Database
- Versions:  `select version();`
- Create:    `create database [db_name];`
- Show:      `show databases;`
- Using DB:  `use [db_name];`
- Droping:   `drop database database_name;`

#### 2. Tables
- Create:    `create table [table_name] (SQL attribute);`
    - SQL attributes:
        1. Same as SQL querys.
        2. AUTO_INCREMENT
- Show:      `show tables;`
- Delete:    `truncate table table_name;`
- Droping:   `drop table table_name;`
- Show columns:
    - `show columns from table_name from database_name;`
    - `show columns from database_name.table_name;`
    - `describe table_name;`
    - `desc table_name;`

#### 3. Authorizations
- Show users and root: `select user, host from mysql.user;`
- Create users: `create user 'username'@'hostname' identified by 'user-name-password';`
- Authorize user: 
    - `grant all on dbname.* to 'username'@'localhost';`
    - `grant SELECT,INSERT,UPDATE,DELETE ON dbname.* to 'username'@'hostname';`

- Show: `show grants for 'user'@'hostname';`
- Show currentuser: 
    - `show grants;`
    - `SHOW GRANTS FOR CURRENT_USER;`
    - `SHOW GRANTS FOR CURRENT_USER();`
- Revoke privilige:
    - `revoke all privileges on dbname.table from 'username'@'hostname'`
    - `revoke all privileges, grant option from 'username'@'hostname';`

- Delete user: `drop user 'username'@'hostname';`




:::info
When using apt-get, please use `apt-get update` to update it first. Or else `ssh` maybe fail.
:::

---

# Centos on pi3
- Look up version
    - `$ cat /etc/redhat-release`
- Default account/passwd
    - root/centos

- Download using `yum`
    - eg. sudo yum install open-ssh^*^

## SSH-server config
[Using ssh-server](https://dotblogs.com.tw/michaelchen/2015/01/06/centos_install_ssh)

- Transfering Files using SSH
```javascript=
$ scp /home/samtang/*.php user1@192.168.1.100:/var/www/
```

## Configuring Static eth0
{%gist spyro12393/c9b7a57c9130bcb79e3f9b9e0dcd46d6%}

## Auto-run python Program
[web](https://lz5z.com/Python%E5%AE%9A%E6%97%B6%E4%BB%BB%E5%8A%A1%E7%9A%84%E5%AE%9E%E7%8E%B0%E6%96%B9%E5%BC%8F/)


## Running programs in Background
1. Use "bg" and "fg".
```javascript=
$ ^z   // After you "stop the program".
$ bg   // Run in background.
$ fg   // Bring background program back.
```
2. Or you can add "&" after the command to run in background.
```javascript=
$ python example.py &
```

> How to keep running the progam after quitting from host using ssh?
>> [name=JJ Yeh][color=red]

## Apache Server
- Installation
```javascript=
$ sudo yum install httpd
$ systemctl start httpd

// Start on boot.
$ systemcel enable httpd
```
- Generate a SSL webserver
    - Self-signed digital certificate.
[web](https://wiki.centos.org/zh-tw/HowTos/Https)



## MariaDB
[Installation](https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-centos-7)
### Basic Installation
```javascript=
// Install
$ sudo yum install mariadb-server
$ sudo systemctl start mariadb

// Check status, if activate(running) then fine.
$ sudo systemctl status mariadb

// MariaDB starts at BOOT
$ sudo systemctl enable mariadb
```

### Securing MariaDB
```javascript=
$ sudo mysql_secure_installation
```

### Login
```javascript=
$ mysqladmin -u root -p version
```

---

# Python
## Timing and Execute
[How to execute a program on schedule?](https://lz5z.com/Python%E5%AE%9A%E6%97%B6%E4%BB%BB%E5%8A%A1%E7%9A%84%E5%AE%9E%E7%8E%B0%E6%96%B9%E5%BC%8F/)














`\xE5\xA5\xB9\xE6\xA0\xB9\xE6\x9C\xAC\xE4\xB9\x9F\xE6\xB2\x92\xE5\xB7\xAE`
