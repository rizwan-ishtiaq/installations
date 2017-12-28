## Create Dir
```
mkdir -p /etc/kannel/conf
mkdir -p /etc/kannel/smscs
mkdir -p /var/log/kannel
mkdir -p /var/run/kannel
mkdir -p /var/spool/kannel/store
mkdir -p /var/spool/kannel/dlr
```
## Create / Copy Configuration Files into /etc/kannel/conf
> You can do it manually by using below files (links) or use wget
* *[kannel.conf](kannel.conf)*
* *[sqlbox.conf](sqlbox.conf)*
* *[opensmppbox.conf](opensmppbox.conf)*
* *[dlr-mysql.conf](dlr-mysql.conf)*
* *[smpplogins.txt](smpplogins.txt)*

**OR**
```
cd /etc/kannel/conf
wget https://raw.githubusercontent.com/rizwan-ishtiaq/installations/master/kannel-1.4.4/kannel.conf
wget https://raw.githubusercontent.com/rizwan-ishtiaq/installations/master/kannel-1.4.4/sqlbox.conf
wget https://raw.githubusercontent.com/rizwan-ishtiaq/installations/master/kannel-1.4.4/opensmppbox.conf
wget https://raw.githubusercontent.com/rizwan-ishtiaq/installations/master/kannel-1.4.4/dlr-mysql.conf
wget https://raw.githubusercontent.com/rizwan-ishtiaq/installations/master/kannel-1.4.4/smpplogins.txt
```
***Change the parameters values according to your system***
# /etc/default/kannel file
> Description: The way it is meant to work is that each /etc/init.d/test script first sources /etc/default/test before starting/stopping the test service. The purpose of the file is to provide extra options for starting the service, or override certain aspects of the service's startup.
1. vi /etc/default/kannel
2. press i for insert mode
3. Copy and paste following line
  * #START_WAPBOX=0
  * START_SMSBOX=1
  * START_SQLBOX=1
  * #START_OPENSMPPBOX=1
  * START_FAKESMSC=1
4. press esc and save file using :x or :wq

## Create / Copy smsc Files into /etc/kannel/smscs
*[fake.conf](fake.conf)*

**OR**
```
cd /etc/kannel/smscs
wget https://raw.githubusercontent.com/rizwan-ishtiaq/installations/master/kannel-1.4.4/fake.conf
```
## Creating (Database/table/user)
> I am creating database `kannel` with user `kannel` and password `kaNneL%123`. But you **should** change it. If you decide to change user name and password, you need to change it in two files *[dlr-mysql.conf](dlr-mysql.conf)* and *[database-dml.sql](database-dml.sql)*. Note mysql 5.7 by default have plugin *validate_password* which will not allow you to have simple password. Run file will root user.
```
cd /tmp
wget https://raw.githubusercontent.com/rizwan-ishtiaq/installations/master/kannel-1.4.4/database-dml.sql
mysql -uroot -p < database-dml.sql
```
## kannel as service
create/copy file *[kannel](kannel)* into /etc/init.d/ manually

**OR**
```
cd /etc/init.d/
wget https://raw.githubusercontent.com/rizwan-ishtiaq/installations/master/kannel-1.4.4/kannel
```
`chmod a+x /etc/init.d/kannel`
### Starting kannel service
`service kannel start` or `systemctl start kannel`
