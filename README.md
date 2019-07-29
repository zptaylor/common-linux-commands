#Common Linux commands
##WebLogic
###to start the admin server PRE
cd /u01/app/oracle/user_projects/domains/vic-ppd/ ; rm nohup.out ; nohup ./startWebLogic.sh & 
###to start the node manager PRE 
cd /u01/app/oracle/user_projects/domains/vic-ppd/bin/;  rm nohup.out ; nohup ./startNodeManager.sh & 

###to start the admin server DEV
cd /u01/app/oracle/user_projects/domains/vic-dev/ ; rm nohup.out ; nohup ./startWebLogic.sh & 
###to start the node manager DEV 
cd /u01/app/oracle/user_projects/domains/vic-dev/bin/;  rm nohup.out ; nohup ./startNodeManager.sh & 

##General
###set JAVA_HOME
export JAVA_HOME=/app/vic/tomcat/java/latest

###change user account to tomcat
dzdo su tomcat

###set permissions for VIC.war file
dzdo chmod 7777 /tmp/VIC.war

###set owner for VIC.war file
dzdo chown tomcat:tomcat /tmp/VIC.war

###run deployment script
/tmp/deployVIC.sh

###shortcut to tail -f the catalina.out log
/tmp/showlog.sh

###shortcut to shutdown tomcat
/tmp/shutdownTomcat.sh

###shortcut to startup tomcat
/tmp/startupTomcat.sh

###show history of all previously run commands
cat ~/.bash_history

###find all instances of openssl in bash_histories
dzdo -u root grep -e "$pattern" -r / --include "*.bash_history" | grep openssl

###transfer VIC.war to 402 from 401
scp /tmp/VIC.war vaaacbdcdop@172.23.24.89:/tmp/

###run deployment script as tomcat
dzdo -u tomcat nano /tmp/deployVIC.sh

###connect to 402
ssh 172.23.24.89

###use tomcat user to change permissions to read/write for all accounts on /tmp directory recursively
dzdo -u tomcat chmod 7777 /tmp -Rvf

###use vaaacbdcdop user to change permissions to read/write for all accounts on /tmp directory recursively
dzdo -u vaaacbdcdop chmod 7777 /tmp -Rvf

###show count of all sessions destroyed on Jan 04
cat /app/vic/tomcat/apache-tomcat-9.0.0.M22/logs/catalina.out | grep "Jan 04 .* Destroy" -c

###show count of all sessions destroyed on Jan 04
cat /app/vic/tomcat/apache-tomcat-9.0.0.M22/logs/catalina.out | grep "Jan 04 .* Destroy" -c

###show last 6000 lines of catalina.out in reader
tail -6000 /app/vic/tomcat/apache-tomcat-9.0.0.M22/logs/catalina.out | less

###show all java processes running on host
ps -ef | grep java

###extract key to pkcs12 as keystore.p12, then pull private key to key.pem, then convert to PKCS8
keytool -importkeystore -srckeystore identity.jks -destkeystore keystore.p12 -deststoretype PKCS12
openssl pkcs12 -in keystore.p12  -nodes -nocerts -out key.pem
openssl pkcs8 -topk8 -inform PEM -outform DER -in pkcs8 -topk8 -inform PEM -outform DER -in key.pem -nocrypt -out ppd.iam.pkcs8


## MONGO
>**Always start cfg service first** then rs(replicaset) service and then mongos!!!

### vaausdbsbdc802 start mongo 802 DB

dzdo -u mongod bash -c "numactl --interleave=all /u01/mongo/mongodb-latest/bin/mongod -f /u01/mongo/etc/mongod_vic_cfg_sqa_29003.conf"
dzdo -u mongod bash -c "numactl --interleave=all /u01/mongo/mongodb-latest/bin/mongod -f /u01/mongo/etc/mongod_vic_rs_sqa_30010.conf"

###vaausappbdc802 start mongo 802 APP

dzdo -u mongod bash -c "numactl --interleave=all /u01/mongo/mongodb-latest/bin/mongos -f /u01/mongo/etc/mongos_vic_sqa_27003.conf"

###vaausdbsbdc801 start mongo 801 DB
dzdo -u mongod bash -c "numactl --interleave=all /u01/mongo/mongodb-latest/bin/mongod -f /u01/mongo/etc/mongod_vic_cfg_dev_29003.conf"
dzdo -u mongod bash -c "numactl --interleave=all /u01/mongo/mongodb-latest/bin/mongod -f /u01/mongo/etc/mongod_vic_rs_dev_30010.conf"

###vaausappbdc801 start mongo 801 APP
dzdo -u mongod bash -c "numactl --interleave=all /u01/mongo/mongodb-latest/bin/mongos -f /u01/mongo/etc/mongos_vic_dev_27003.conf"

##Network
###show all nat redirection rules in firewall on a linux box
dzdo iptables -t nat -L -n

###forward 443/444 to 8443/8444
dzdo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 8443
dzdo iptables -t nat -A PREROUTING -p tcp --dport 444 -j REDIRECT --to-port 8444

###delete the first iptable rule.
dzdo iptables -t nat -D PREROUTING 1

##Connecting to HM in ssh and copying over their key to the known hosts file for use by VIC:
###Make sure this is done on the server that will be connecting to this file, or copy over the known_hosts file to the one that will.

#keytool
##import certificate into trusted store
/app/vic/java/latest/bin/keytool -import -noprompt -trustcacerts -file ./vaww.vrs-preprod.va.gov.cer -alias vaww.vrs-preprod.va.gov -keystore  /app/vic/keys/ppd.trust.jks -storepass rhel5r00ls

#
dzdo openssl s_client -connect vaausvrsweb40.aac.va.gov:443