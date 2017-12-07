# Congratulation
Installation and configuration is completed now. It is time to test

# Port scan
netstat -tunap | grep box<br/>
Output should be like following<br/>
tcp        0      0 0.0.0.0:13005           0.0.0.0:*               LISTEN      4616/sqlbox    <br/>     
tcp        0      0 0.0.0.0:10000           0.0.0.0:*               LISTEN      4588/bearerbox      <br/>
tcp        0      0 0.0.0.0:13013           0.0.0.0:*               LISTEN      4601/smsbox         <br/>
tcp        0      0 0.0.0.0:13000           0.0.0.0:*               LISTEN      4588/bearerbox      <br/>
tcp        0      0 0.0.0.0:13001           0.0.0.0:*               LISTEN      4588/bearerbox      <br/>
tcp        0      0 127.0.0.1:54018         127.0.0.1:13001         ESTABLISHED 4616/sqlbox         <br/>
tcp        0      0 127.0.0.1:10000         127.0.0.1:47918         ESTABLISHED 4588/bearerbox      <br/>
tcp        0      0 127.0.0.1:13001         127.0.0.1:54016         ESTABLISHED 4588/bearerbox      <br/>
tcp        0      0 127.0.0.1:13001         127.0.0.1:54018         ESTABLISHED 4588/bearerbox      <br/>
tcp        0      0 127.0.0.1:54016         127.0.0.1:13001         ESTABLISHED 4601/smsbox<br/>

# Process scan
ps aux | grep kannel <br/>
root      4586  0.0  0.0  66328  2360 ?        Ss   04:47   0:00 /usr/local/sbin/bearerbox -v 4 --parachute --daemonize --pid-file /var/run/kannel/kannel_bearerbox.pid /etc/kannel/conf/kannel.conf<br/>
root      4588  0.0  0.0 680972  5276 ?        Sl   04:47   0:00 /usr/local/sbin/bearerbox -v 4 --parachute --daemonize --pid-file /var/run/kannel/kannel_bearerbox.pid /etc/kannel/conf/kannel.conf<br/>
root      4599  0.0  0.0  65940  2364 ?        Ss   04:47   0:00 /usr/local/sbin/smsbox -v 4 --parachute --daemonize --pid-file /var/run/kannel/kannel_smsbox.pid /etc/kannel/conf/kannel.conf<br/>
root      4601  0.0  0.0 459328  2792 ?        Sl   04:47   0:00 /usr/local/sbin/smsbox -v 4 --parachute --daemonize --pid-file /var/run/kannel/kannel_smsbox.pid /etc/kannel/conf/kannel.conf<br/>
root      4614  0.0  0.0  65764  2364 ?        Ss   04:47   0:00 /usr/local/sbin/sqlbox -v 4 --parachute --daemonize --pid-file /var/run/kannel/kannel_sqlbox.pid /etc/kannel/conf/sqlbox.conf<br/>
root      4616  0.0  0.0 213632  3416 ?        Sl   04:47   0:00 /usr/local/sbin/sqlbox -v 4 --parachute --daemonize --pid-file /var/run/kannel/kannel_sqlbox.pid /etc/kannel/conf/sqlbox.conf<br/>
root      4621  0.0  0.0  65720  5004 ?        S    04:47   0:00 /usr/local/sbin/fakesmsc -H localhost --port 10000 -F /var/log/kannel/fakesmsc.log -p /var/run/kannel/kannel_fakesmsc.pid -m 0 100 300 text fakesmsc<br/>

# Send sms test
Connect to mysql and use kannel database<br/>
INSERT INTO send_sms (momt, sender, receiver, msgdata, sms_type, smsc_id,dlr_mask) VALUES ('MT', 'TESTID', '9995323922', 'Hello world', 2,'FAKE-10',1);<br/>
select * from sent_sms;<br/>
+--------+------+--------+------------+---------+-------------+------------+-----------+---------+---------+------+----------+--------+------+--------+----------+----------+----------+----------+---------+------+---------+------+---------+---------+-------+-----------+--------------------------------------+----------+<br/>
| sql_id | momt | sender | receiver   | udhdata | msgdata     | time       | smsc_id   | service | account | id   | sms_type | mclass | mwi  | coding | compress | validity | deferred | dlr_mask | dlr_url | pid  | alt_dcs | rpi  | charset | boxc_id | binfo | meta_data | foreign_id                           | dlr_sync |<br/>
+--------+------+--------+------------+---------+-------------+------------+-----------+---------+---------+------+----------+--------+------+--------+----------+----------+----------+----------+---------+------+---------+------+---------+---------+-------+-----------+--------------------------------------+----------+<br/>
|     1  | MT   | TESTID | 9995323922 | NULL    | Hello+world |       NULL | FAKE-10   | NULL    | NULL    | NULL |        2 |   NULL | NULL |   NULL |     NULL |     NULL |     NULL |        1 | NULL    | NULL |    NULL | NULL | NULL    | sqlbox  | NULL  | NULL      | 10                                   |        0 |<br/>
|     2  | DLR  | TESTID | 9995323922 | NULL    | NULL        | 1512640381 | FAKE-10   | NULL    | NULL    | NULL |        3 |   NULL | NULL |   NULL |     NULL |     NULL |     NULL |        1 | NULL    | NULL |    NULL | NULL | NULL    | sqlbox  | NULL  | NULL      | 6d126e36-cfdb-4863-b528-98b6ba833cfc |        0 |<br/>
+--------+------+--------+------------+---------+-------------+------------+-----------+---------+---------+------+----------+--------+------+--------+----------+----------+----------+----------+---------+------+---------+------+---------+---------+-------+-----------+--------------------------------------+----------+<br/>
