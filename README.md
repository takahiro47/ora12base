
# project ora12base

The ora12base project, is a relevantly simple vagrant project to automate setup an Oracle 12c environment running on Centos 6.6. I am starting this project using the opscode_centos-6.6_chef-provisionerless.box, I chose this image as this virtual machine can extend upto nearly 40G.

### From your host machine

Install a Vagrant plugin which automatically installs the host's VirtualBox Guest Additions on the guest system.
https://github.com/dotless-de/vagrant-vbguest

```
vagrant gem install vagrant-vbguest
```

Setup a linux machine.

```
vagrant up
vagrant ssh
```

### Then run as root the pre-install 12c software (mostly rpm driven):

```
sudo -i
/vagrant/root_post_vagrant_build.sh
```

### Now we can install 12c software as oracle

```
su - oracle
/vagrant/oracle_post_vagrant_build.sh
```

### The final step of which is 12c software root.sh commands (i.e. as root again)

As root user:

``` 
/etc/oraInventory/orainstRoot.sh
/u01/product/12.1.0/db_1/root.sh
```

### Now via dbca we can create a 12c database

As root user:

```
rm /tmp/linuxamd64_12c_database_*zip
```

As oracle user:

```
dbca -silent -createDatabase -responseFile  /vagrant/dbca.rsp
```

### note if you have issues and need to cleanup after any 12c software failures

As oracle user:

```
rm -Rf /etc/oraInventory/*
rm -Rf /u01/product/12.1.0/db_1/*
```

### Connect to Oracle Database as sysdba

As oracle user:

```
$ sqlplus / as sysdba
```

The command results will be:

```
$ sqlplus / as sysdba

SQL*Plus: Release 12.1.0.2.0 Production on Sat Dec 5 17:39:59 2015

Copyright (c) 1982, 2014, Oracle.  All rights reserved.


Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL> show con_name

CON_NAME
------------------------------
orcl12c

SQL> CONN SYS AS SYSDBA

Enter password: qwe123
Connected.

SQL>
```

Enjoy!

   

