# Congratulation
Installation and configuration is completed now. It is time to test

## Service check
`service kannel status`
```
Status Kannel bearerbox [12029]                            [RUNNING]
Status Kannel smsbox [12042]                               [RUNNING]
Status Kannel sqlbox  [12056]                              [RUNNING]
Status Kannel fakesmsc [12065]                             [RUNNING]
```
`systemctl status kannel`
```
● kannel.service - SYSV: Kannel server daemon
   Loaded: loaded (/etc/rc.d/init.d/kannel; bad; vendor preset: disabled)
   Active: active (running) since Thu 2017-12-28 10:26:21 +03; 44s ago
     Docs: man:systemd-sysv-generator(8)
  Process: 12007 ExecStop=/etc/rc.d/init.d/kannel stop (code=exited, status=0/SUCCESS)
  Process: 12023 ExecStart=/etc/rc.d/init.d/kannel start (code=exited, status=0/SUCCESS)
   CGroup: /system.slice/kannel.service
           ├─12029 /usr/local/sbin/bearerbox -v 4 --parachute --daemonize --pid-file /var/run/kannel/kannel_bearerbox.pid /etc/kannel/conf/kan...
           ├─12031 /usr/local/sbin/bearerbox -v 4 --parachute --daemonize --pid-file /var/run/kannel/kannel_bearerbox.pid /etc/kannel/conf/kan...
           ├─12042 /usr/local/sbin/smsbox -v 4 --parachute --daemonize --pid-file /var/run/kannel/kannel_smsbox.pid /etc/kannel/conf/kannel.co...
           ├─12045 /usr/local/sbin/smsbox -v 4 --parachute --daemonize --pid-file /var/run/kannel/kannel_smsbox.pid /etc/kannel/conf/kannel.co...
           ├─12056 /usr/local/sbin/sqlbox -v 4 --parachute --daemonize --pid-file /var/run/kannel/kannel_sqlbox.pid /etc/kannel/conf/sqlbox.co...
           ├─12059 /usr/local/sbin/sqlbox -v 4 --parachute --daemonize --pid-file /var/run/kannel/kannel_sqlbox.pid /etc/kannel/conf/sqlbox.co...
           └─12065 /usr/local/sbin/fakesmsc -H localhost --port 10000 -F /var/log/kannel/fakesmsc.log -p /var/run/kannel/kannel_fakesmsc.pid -...
```
## Port scan
`netstat -tunap | grep box`

Output should be like following
```
tcp        0      0 0.0.0.0:13005           0.0.0.0:*               LISTEN      12059/sqlbox        
tcp        0      0 0.0.0.0:10000           0.0.0.0:*               LISTEN      12031/bearerbox     
tcp        0      0 0.0.0.0:13013           0.0.0.0:*               LISTEN      12045/smsbox        
tcp        0      0 0.0.0.0:13000           0.0.0.0:*               LISTEN      12031/bearerbox     
tcp        0      0 0.0.0.0:13001           0.0.0.0:*               LISTEN      12031/bearerbox     
tcp        0      0 127.0.0.1:13001         127.0.0.1:55236         ESTABLISHED 12031/bearerbox     
tcp        0      0 127.0.0.1:55236         127.0.0.1:13001         ESTABLISHED 12045/smsbox        
tcp        0      0 127.0.0.1:10000         127.0.0.1:54640         ESTABLISHED 12031/bearerbox     
tcp        0      0 127.0.0.1:55240         127.0.0.1:13001         ESTABLISHED 12059/sqlbox        
tcp        0      0 127.0.0.1:13001         127.0.0.1:55240         ESTABLISHED 12031/bearerbox
```

## Process id files
`ls -l /var/run/kannel/`
```
-rw-r--r--. 1 root root 6 Dec 28 10:26 kannel_bearerbox.pid
-rw-r--r--. 1 root root 6 Dec 28 10:26 kannel_fakesmsc.pid
-rw-r--r--. 1 root root 6 Dec 28 10:26 kannel_smsbox.pid
-rw-r--r--. 1 root root 6 Dec 28 10:26 kannel_sqlbox.pid
```

## Process scan
`ps aux | grep kannel`
```
root     12029  0.0  0.0  40940   952 ?        Ss   10:26   0:00 /usr/local/sbin/bearerbox -v 4 --parachute --daemonize --pid-file /var/run/kannel/kannel_bearerbox.pid /etc/kannel/conf/kannel.conf
root     12031  0.0  0.1 589980  1416 ?        Sl   10:26   0:00 /usr/local/sbin/bearerbox -v 4 --parachute --daemonize --pid-file /var/run/kannel/kannel_bearerbox.pid /etc/kannel/conf/kannel.conf
root     12042  0.0  0.0  40548   968 ?        Ss   10:26   0:00 /usr/local/sbin/smsbox -v 4 --parachute --daemonize --pid-file /var/run/kannel/kannel_smsbox.pid /etc/kannel/conf/kannel.conf
root     12045  0.0  0.1 433936  1380 ?        Sl   10:26   0:00 /usr/local/sbin/smsbox -v 4 --parachute --daemonize --pid-file /var/run/kannel/kannel_smsbox.pid /etc/kannel/conf/kannel.conf
root     12056  0.0  0.0  40372   960 ?        Ss   10:26   0:00 /usr/local/sbin/sqlbox -v 4 --parachute --daemonize --pid-file /var/run/kannel/kannel_sqlbox.pid /etc/kannel/conf/sqlbox.conf
root     12059  0.0  0.1 122720  1712 ?        Sl   10:26   0:00 /usr/local/sbin/sqlbox -v 4 --parachute --daemonize --pid-file /var/run/kannel/kannel_sqlbox.pid /etc/kannel/conf/sqlbox.conf
root     12065  0.0  0.2  40332  2256 ?        S    10:26   0:00 /usr/local/sbin/fakesmsc -H localhost --port 10000 -F /var/log/kannel/fakesmsc.log -p /var/run/kannel/kannel_fakesmsc.pid -m 0 100 300 text fakesmsc
```

## Send sms test
Connect to mysql and use kannel database

`INSERT INTO send_sms (momt, sender, receiver, msgdata, sms_type, smsc_id,dlr_mask) VALUES ('MT', 'TESTID', '9995323922', 'Hello world', 2,'FAKE-10',1);`

`select * from sent_sms;`
```
+--------+------+--------+------------+---------+-------------+------------+-----------+---------+---------+------+----------+--------+------+--------+----------+----------+----------+----------+---------+------+---------+------+---------+---------+-------+-----------+--------------------------------------+----------+
| sql_id | momt | sender | receiver   | udhdata | msgdata     | time       | smsc_id   | service | account | id   | sms_type | mclass | mwi  | coding | compress | validity | deferred | dlr_mask | dlr_url | pid  | alt_dcs | rpi  | charset | boxc_id | binfo | meta_data | foreign_id                           | dlr_sync |
+--------+------+--------+------------+---------+-------------+------------+-----------+---------+---------+------+----------+--------+------+--------+----------+----------+----------+----------+---------+------+---------+------+---------+---------+-------+-----------+--------------------------------------+----------+
|     1  | MT   | TESTID | 9995323922 | NULL    | Hello+world |       NULL | FAKE-10   | NULL    | NULL    | NULL |        2 |   NULL | NULL |   NULL |     NULL |     NULL |     NULL |        1 | NULL    | NULL |    NULL | NULL | NULL    | sqlbox  | NULL  | NULL      | 10                                   |        0 |
|     2  | DLR  | TESTID | 9995323922 | NULL    | NULL        | 1512640381 | FAKE-10   | NULL    | NULL    | NULL |        3 |   NULL | NULL |   NULL |     NULL |     NULL |     NULL |        1 | NULL    | NULL |    NULL | NULL | NULL    | sqlbox  | NULL  | NULL      | 6d126e36-cfdb-4863-b528-98b6ba833cfc |        0 |
+--------+------+--------+------------+---------+-------------+------------+-----------+---------+---------+------+----------+--------+------+--------+----------+----------+----------+----------+---------+------+---------+------+---------+---------+-------+-----------+--------------------------------------+----------+
```
