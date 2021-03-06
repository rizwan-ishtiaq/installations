# Welcome to Install kannel from source
kannel is installed as a root user on centos 7 as a service with SQL BOX (means can use database to send sms, receive dlr and queue dlrs)

## Prerequisites
1. lynx web browswer should be installed, kannel init script will need this
2. Assume Mysql already installed (prefered 5.7) *[install mysql community](https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/)*
3. mysql-community-devel package should also be installed with mysql installtion

## Follow in order
1. Start with *[Installation](1-installation-steps.md)*
2. After that *[Configurations](2-configuration-steps.md)*
3. Finally *[Testing kannel](3-test-installation.md)*

## Ubuntu Installation / Changes
1. start with `sudo su -`
2. `apt-get update`
3. My envoirnment was missing following tools
4. `apt-get install build-essential m4 libxml2 libxml2-dev libmysqlclient-dev`
5. Now follow steps from *[bison-version](1-installation-steps.md#bison-version)*
6. After configuration, service will not start, because of two reasons
  1. Ubuntu requires systemd service to register kannel as service. For help *[see](https://www.freedesktop.org/software/systemd/man/systemd.service.html)*
  2. Bash kannel script required little changes to run into ubuntu
  
You can still run kannel manually see file *[manual-run-command](manual-run-command)* for help.
