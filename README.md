# Guacamole
To install All packages of Guacamole with Mysql Connect (5.51)
This script will only work with centOS 7.x. Dont waste your time to run with other distros.

Original File : >> wget http://sourceforge.net/projects/guacamoleinstallscript/files/CentOS/guacamole-install-script.sh
Before running change the version to "0.9.14"


Modified script by Ghanshyam Dusane : https://github.com/ghanshyamdusane/Guacamole-0.9.14.git
unzip the package
yum install epel-release wget -y
chmod +x guacamole-install-script.sh
./guacamole-install-script.sh


Enter the root password for MariaDB: remote.rpm
Enter the Guacamole DB name: guadb
Enter the Guacamole DB username: guauser
Enter the Guacamole DB password: remote.rpm
Enter the Java KeyStore password (least 6 characters): remote.rpm
Do you wish to Install the Proxy feature (Nginx)?: Yes
Enter the Guacamole Server IP addres or hostame (default localhost): 
Enter the URI path (default guacamole):



MariaDB [guadb]> select * from guacamole_connection;
+---------------+-------------------+-----------+----------+------------+----------------+-------------------------+-----------------+--------------------------+-------------------+---------------+
| connection_id | connection_name   | parent_id | protocol | proxy_port | proxy_hostname | proxy_encryption_method | max_connections | max_connections_per_user | connection_weight | failover_only |
+---------------+-------------------+-----------+----------+------------+----------------+-------------------------+-----------------+--------------------------+-------------------+---------------+
|             2 | W2012             |      NULL | rdp      |       NULL | NULL           | NULL                    |            NULL |                     NULL |              NULL |             0 |
|             3 | Test              |      NULL | ssh      |       NULL | NULL           | NULL                    |            NULL |                     NULL |              NULL |             0 |
|             4 | RHEL              |      NULL | ssh      |       NULL | NULL           | NULL                    |            NULL |                     NULL |              NULL |             0 |
|             5 | Jenkins & Ansible |      NULL | ssh      |       NULL | NULL           | NULL                    |            NULL |                     NULL |              NULL |             0 |
|             6 | Terraform         |      NULL | ssh      |       NULL | NULL           | NULL                    |            NULL |                     NULL |              NULL |             0 |
+---------------+-------------------+-----------+----------+------------+----------------+-------------------------+-----------------+--------------------------+-------------------+---------------+
5 rows in set (0.00 sec)

MariaDB [guadb]> select * from guacamole_connection_parameter;
+---------------+----------------+-----------------+
| connection_id | parameter_name | parameter_value |
+---------------+----------------+-----------------+
|             2 | hostname       | 13.127.184.236  |
|             2 | ignore-cert    | true            |
|             2 | password       | Pass@1234567    |
|             2 | port           | 3389            |
|             2 | security       | any             |
|             2 | username       | Administrator   |
|             3 | color-scheme   | green-black     |
|             3 | font-size      | 14              |
|             5 | hostname       | 172.31.1.154    |
|             5 | port           | 22              |
|             6 | hostname       | 172.31.31.12    |
|             6 | port           | 22              |
+---------------+----------------+-----------------+
12 rows in set (0.00 sec)

MariaDB [guadb]>



#Native Installation steps#

yum install cairo-devel libjpeg-devel libpng-devel uuid-devel freerdp-devel pango-devel libssh2-devel libssh-dev --enablerepo=*
yum install java

Download latest package from Gucamole website
wget http://www-us.apache.org/dist/guacamole/0.9.14/source/guacamole-server-0.9.14.tar.gz
wget http://www-eu.apache.org/dist/guacamole/0.9.14/binary/guacamole-0.9.14.war

Move the server package to installation directory

tar -xzf guacamole-server-0.9.14.tar.gz
cd guacamole-server-0.9.14/
./configure --with-init-dir=/etc/init.d
make
make install
ldconfig


Donwload the latest Apache package and move the following package in webapps directory

wget http://www-eu.apache.org/dist/guacamole/0.9.14/binary/guacamole-0.9.14.war
mkdir /etc/guacamole
mkdir /usr/share/tomcat/.guacamole

vi /etc/guacamole/guacamole.properties

guacd-hostname: localhost
guacd-port:    4822
user-mapping:    /etc/guacamole/user-mapping.xml
auth-provider:    net.sourceforge.guacamole.net.basic.BasicFileAuthenticationProvider
basic-user-mapping:    /etc/guacamole/user-mapping.xml

ln -s /etc/guacamole/guacamole.properties /usr/share/tomcat/.guacamole/

vi /etc/guacamole/user-mapping.xml
<user-mapping>
<authorize 
username="tecmint" 
password="8383339b9c90775ac14693d8e620981f" 
encoding="md5">
<connection name="RHEL 7">
<protocol>ssh</protocol>
<param name="hostname">192.168.0.18</param>
<param name="port">22</param>
<param name="username">gacanepa</param>
</connection>
<connection name="Windows 10">
<protocol>rdp</protocol>
<param name="hostname">192.168.0.19</param>
<param name="port">3389</param>
</connection>
</authorize>
</user-mapping>

service tomcat start
/etc/init.d/guacd start
