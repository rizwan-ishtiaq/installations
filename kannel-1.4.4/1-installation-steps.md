## Compile tools
```
yum update
yum groupinstall "Development tools" -y
yum install libxml2-devel
```
## Bison version
`bison --version`

> *bison 2.7* will be good choice if your version is > 3.0 because kannel will NOT compile if version is > 3.0
```
cd /usr/local/src
wget http://ftp.gnu.org/gnu/bison/bison-2.7.tar.gz
tar -zxvf bison-2.7.tar.gz
cd bison-2.7
./configure --prefix=/usr/local/bison27
make
make install
```

### Use Bison 2.7 (if needed)
`export PATH=/usr/local/bison27/bin:$PATH`

## Download kannel source
```
cd /usr/local/src/
wget http://www.kannel.org/download/1.4.4/gateway-1.4.4.tar.gz
tar -zxvf gateway-1.4.4.tar.gz
```

## Compile & Install Kannel with MySQL support
```
cd /usr/local/src/gateway-1.4.4
./configure  --prefix=/etc/kannel --with-mysql --with-mysql-dir=/usr/include/mysql --enable-start-stop-daemon
#There can be some warring messages while make, you can igonore them
make
make install
```
## Comiple & Install SQLBOX
```
cd /usr/local/src/gateway-1.4.4/addons
cd sqlbox
./bootstrap
./configure --with-kannel-dir=/etc/kannel --prefix=/etc/kannel
#note it will be looking file /usr/include/mysql/mysql.h
make
make install
```
A good complete reference *[pdf](https://sourceforge.net/p/kannelappliance/wiki/Configuration/attachment/sqlbox-userguide.pdf)* document can be used to look into more detail.


## Install fake smsc
The fake SMS center should compile at the same time as main Kannel compiles. But it's binnary will not copy as a part of install so we have to do it manually
```
cd /usr/local/src/gateway-1.4.4/test
cp fakesmsc /etc/kannel/sbin/
```
## Executables in usr local sbin
`ln -s /etc/kannel/sbin/* /usr/local/sbin/`
