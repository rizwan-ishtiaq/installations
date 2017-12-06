# Create / Copy Configuration Files into /etc/kannel/conf
kannel.conf<br/>
sqlbox.conf<br/>
opensmppbox.conf<br/>
dlr-mysql.conf <br/>
Change the parameters values according to your system

# Create / Copy smsc Files into /etc/kannel/smsc
fake.conf<br/>
smsc1.conf<br/>

# kannel as service
copy file kannel into /etc/init.d/<br/>
serivce kannel start<br/>
or<br/>
systemctl start kannel<br/>
