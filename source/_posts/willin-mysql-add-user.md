title: "Willin: Mysql Add User and Grant Privileges"
date: 2015-03-20 14:03:25
categories: Dev
tags: [mysql]
---


Login into mysql console:

```
mysql -u root -p
```

### 1. Add A User

```
insert into mysql.user(Host,User,Password) values("localhost","USER",password("PASSWORD"));
flush privileges;
```

### 2. Grant Privileges

```
grant all privileges on DATABASEX.* to USER@localhost identified by 'PASSWORD';
flush privileges;
```

Options:

* ALL
* ALTER
* CREATE
* DROP
* SELECT
* UPDATE
* DELETE


### Optional: Change Password

```
update mysql.user set password=password('NEW PASSWORD') where User="USER" and Host="localhost";
flush privileges;
```

### Optional: Delete A User

```
DELETE FROM user WHERE User="USER" and Host="localhost";
flush privileges;
```

