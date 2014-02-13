# inspect libnss_mysql && nscd

## installed

 * CentOS6.5
 * libnss_mysql
 * nscd
 * mysqld

## usage

```
$ vagrant up
$ vagrant ssh

# /usr/bin/id -> nscd -> libnss_mysql -> mysqld
[vagrant@vagrant-centos65 ~]$ id cinergi
uid=5000(cinergi) gid=5000(foobaz) groups=5000(foobaz),5010(hogehoge)
```

