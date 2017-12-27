# Installations
installation of different softwares

## kannel 1.4.4
Goto kannel-1.4.4 directory. This kannel is installed as a root user on centos 7 as a service with SQL BOX (means can use database to send sms, receive dlr and queue dlrs). Some on important points are following
<ul>
  <li>CentOS Linux release 7.4.1708 (Core)  [x86_64], kernel: 3.10.0-693.5.2.el7.x86_64</li>
  <li>with mysql Ver 14.14 Distrib 5.7.20, for linux-glibc2.12 (x86_64) using  EditLine wrapper</li>
  <li>Send messages from mysql table to smsc</li>
  <li>Fake SMSC</li>
  <li>After Installation with default, will start following default components</li>
  <ul>
    <li>bearerbox</li>
    <li>smsbox</li>
    <li>sqlbox</li>
    <li>spool dlrs</li>
    <li>fakesmsc</li>
  </ul>
</ul>
Working will be: read messages from send_sms table and send it to attached smsc. When Dlr received by smsc update sent_sms table. kannel will use /var/spool as a storage for processing.

NOTE: I also installed it on **Ubuntu 14.04.5 LTS (GNU/Linux 4.4.0-31-generic x86_64)**. See *[read me](kannel-1.4.4/0-readme.md)* file for more details.
