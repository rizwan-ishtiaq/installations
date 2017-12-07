# Compile tools
yum update<br/>
yum groupinstall "Development tools" -y<br/>
yum install libxml2-devel<br/>

# Download source
cd /usr/local/src/<br/>
wget http://www.kannel.org/download/1.4.4/gateway-1.4.4.tar.gz<br/>
tar -zxvf gateway-1.4.4.tar.gz<br/>

# Bison version (kannel will NOT compile if version is > 3.0)
bison --version<br/>
#bison 2.7 will be good choice<br/>
cd /usr/local/src<br/>
wget http://ftp.gnu.org/gnu/bison/bison-2.7.tar.gz<br/>
tar -zxvf bison-2.7.tar.gz<br/>
cd bison-2.7<br/>
./configure --prefix=/usr/local/bison27<br/>
make<br/>
make install<br/>
#There can be some warring messages, you can igonore them

# Use Bison 2.7
export PATH=/usr/local/bison27:$PATH<br/>

# Compile & Install Kannel with MySQL support
cd /usr/local/src/gateway-1.4.4<br/>
./configure  --prefix=/etc/kannel --with-mysql --with-mysql-dir=/usr/include/mysql --enable-start-stop-daemon<br/>
make<br/>
make install<br/>
#There can be some warring messages, you can igonore them

# Comiple & Install SQLBOX
cd /usr/local/src/gateway-1.4.4/addons<br/>
cd sqlbox<br/>
./bootstrap<br/>
./configure --with-kannel-dir=/etc/kannel<br/>
make<br/>
make install<br/>
A good complete reference document can be found at following<br/>
https://sourceforge.net/p/kannelappliance/wiki/Configuration/attachment/sqlbox-userguide.pdf

# Install fake smsc
The fake SMS center should compile at the same time as main Kannel compiles. But it's binnary will not copy as a part of install so we have to do it manually<br/>
cd /usr/local/src/gateway-1.4.4/test<br/>
cp fakesmsc /usr/local/sbin/<br/>
