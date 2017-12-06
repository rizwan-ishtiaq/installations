# Comile tools
yum update
yum groupinstall "Development tools" -y
yum install libxml2-devel

# Download source
cd /usr/local/src/
wget http://www.kannel.org/download/1.4.4/gateway-1.4.4.tar.gz
tar -zxvf gateway-1.4.4.tar.gz

# Bison version (kannel will NOT compile if version is > 3.0)
bison --version
#bison 2.7 will be good choice
cd /user/local/src
wget http://ftp.gnu.org/gnu/bison/bison-2.7.tar.gz
tar -zxvf bison-2.7.tar.gz
cd bison-2.7
./configure --prefix=/usr/local/bison27
make
make install
#There can be some warring messages, you can igonore them

# Use Bison 2.7
export PATH=/usr/local/bison27:$PATH

# Compile & Install Kannel with MySQL support
cd /usr/local/src/gateway-1.4.4
./configure  --prefix=/etc/kannel --enable-start-stop-daemon
make
make install
