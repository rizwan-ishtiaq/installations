# Create Dir
mkdir -p /etc/kannel/conf<br/>
mkdir -p /etc/kannel/smscs<br/>
mkdir -p /var/log/kannel<br/>
mkdir -p /var/spool/kannel/store<br/>
mkdir -p /var/spool/kannel/dlr<br/>

# Create / Copy Configuration Files into /etc/kannel/conf
kannel.conf<br/>
sqlbox.conf<br/>
opensmppbox.conf<br/>
dlr-mysql.conf <br/>
smpplogins.txt <br/>
Change the parameters values according to your system

# /etc/default/kannel file
Description: The way it is meant to work is that each /etc/init.d/test script first sources /etc/default/test before starting/stopping the test service. The purpose of the file is to provide extra options for starting the service, or override certain aspects of the service's startup.
<ol>
  <li>vi /etc/default/kannel</li>
  <li>press i for insert mode</li>
  <li>Copy and paste following line</li>
  <ul>
    <li>#START_WAPBOX=0</li>
    <li>START_SMSBOX=1</li>
    <li>START_SQLBOX=1</li>
    <li>#START_OPENSMPPBOX=1</li>
    <li>START_FAKESMSC=1</li>
  </ul>
  <li>press esc and save file using :x or :wq</li>
</ol>

# Create / Copy smsc Files into /etc/kannel/smscs
fake.conf<br/>

# kannel as service
copy file kannel into /etc/init.d/<br/>
chmod a+x /etc/init.d/kannel<br/>
serivce kannel start<br/>
or<br/>
systemctl start kannel<br/>
