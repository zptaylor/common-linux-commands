
# Common Linux commands

*These commands are just what I have to keep typing over and over.*

## [Maven](MAVEN.md)

## [Fortify](FORTIFY.md)

## General

### set JAVA_HOME

```cmd
export JAVA_HOME=/app/[app]/java/latest
```

### change user account to tomcat

```cmd
dzdo su tomcat
```

### set permissions for APP war file

```cmd
dzdo chmod 7777 /tmp/[app].war
```

### set owner for APP war file to tomcat:tomcat

```cmd
dzdo chown tomcat:tomcat /tmp/[app].war
```

### find all instances of "aStringHere" in bash_histories

```cmd
dzdo -u root grep -e "$pattern" -r / --include "*.bash_history" | grep "aStringHere"
```

### transfer APP war to another server

```cmd
scp /tmp/[app].war [user]@[ip]:/tmp/
```

### show count of all sessions destroyed on Jan 04 in Tomcat log

```cmd
cat /app/[app]/tomcat/latest/logs/catalina.out | grep "Jan 04 .* Destroy" -c
```

### show last 6000 lines of catalina.out in reader

```cmd
tail -6000 /app/[app]/tomcat/latest/logs/catalina.out | less
```

### show all java processes running on host

```cmd
ps -ef | grep java
```

### extract key to pkcs12 as keystore.p12, then pull private key to key.pem, then convert to PKCS8

```cmd
keytool -importkeystore -srckeystore identity.jks -destkeystore keystore.p12 -deststoretype PKCS12
openssl pkcs12 -in keystore.p12  -nodes -nocerts -out key.pem
openssl pkcs8 -topk8 -inform PEM -outform DER -in pkcs8 -topk8 -inform PEM -outform DER -in key.pem -nocrypt -out key.pkcs8
```

### chmod 755 (rwx-rx-rx) all directorys in /app/

```cmd
sudo find /app/ -type d -exec chmod 755 {} \;
```

### chmod a+rw all files in /app/

```cmd
sudo find /app/ -type f -exec chmod a+rw {} \;
```

## MONGO

>**Always start cfg service first** then rs(replicaset) service and then mongos!!!

### start mongo on the DB server

```cmd
dzdo -u mongod bash -c "numactl --interleave=all /u01/mongo/mongodb-latest/bin/mongod -f /u01/mongo/etc/mongod_[app]_cfg_sqa_29003.conf"
dzdo -u mongod bash -c "numactl --interleave=all /u01/mongo/mongodb-latest/bin/mongod -f /u01/mongo/etc/mongod_[app]_rs_sqa_30010.conf"
```

### start mongo connector daemon on the app server

```cmd
dzdo -u mongod bash -c "numactl --interleave=all /u01/mongo/mongodb-latest/bin/mongos -f /u01/mongo/etc/mongos_[app]_sqa_27003.conf"
```

## Network

### show all nat redirection rules in firewall on a linux box

```cmd
dzdo iptables -t nat -L -n
```

### forward 443/444 to 8443/8444

```cmd
dzdo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 8443
dzdo iptables -t nat -A PREROUTING -p tcp --dport 444 -j REDIRECT --to-port 8444
```

### delete the first iptable rule

```cmd
dzdo iptables -t nat -D PREROUTING 1
```

## keytool

### import certificate into trusted store

```cmd
/app/[app]/java/latest/bin/keytool -import -noprompt -trustcacerts -file ./cert.cer -alias [app].va.gov -keystore  /app/[app]/keys/trust.jks -storepass [passhere]
```

### check certs on destination server

```cmd
dzdo openssl s_client -connect [app].va.gov:443
```

## WebLogic

### to start the admin server

```cmd
cd /u01/app/oracle/user_projects/domains/[app]/ ; rm nohup.out ; nohup ./startWebLogic.sh &
```

### start the node manager

```cmd
cd /u01/app/oracle/user_projects/domains/[app]/bin/;  rm nohup.out ; nohup ./startNodeManager.sh & 
```
