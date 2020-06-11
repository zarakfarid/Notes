# Java Commands
## Java Version: 
```Shell
javap -verbose MyClass | grep “major"


* Java 1.2 uses major version 46
* Java 1.3 uses major version 47
* Java 1.4 uses major version 48
* Java 5 uses major version 49
* Java 6 uses major version 50
* Java 7 uses major version 51
* Java 8 uses major version 52
```
## SCP: 
```Shell
scp ana@192.168.90.177:/home/ana/zarak/prodCornellNameCommunity.json Downloads/

scp ana@192.168.90.177:/home/ana/zarak/prodCornellNameLocal.json Downloads/

scp ana@cloud3.artstor.acit.com:/home/ana/adam/apache-tomcat-6.0.37/bin/startup.sh Downloads/

scp ana@refreshprod.artstor.acit.com:/home/ana/adam/apache-tomcat-8.0.28 /home/ana/adam/

scp /home/ana/zarak/AVES254.js -i /home/ana/PlanningTestInstance.pem ana@192.168.97.62:/home/ana/zarak/

scp  result.txt-i /home/ana/PlanningTestInstance.pem ana@192.168.90.77:/home/ana/zarak/

scp -i /home/ana/PlanningTestInstance.pem ana@cloud3.artstor.acit.com:/home/ana/zarak/mongocsv/test.csv Downloads/
```

## SCRIPTS AND COMMANDS: 
```Shell
export LC_ALL="en_US.UTF-8"


ssh -nNT -L 5433:localhost:5432 -l ana -i /Users/Zarak/Downloads/PlanningTestInstance.pem 


tar -zcvf apache-tomcat-6.0.37.tar.gz /home/ana/adam/apache-tomcat-6.0.37


For JAVA Cache Clearing : sync && echo 3 | sudo tee /proc/sys/vm/drop_caches


chown -R ana:tomcat /home/ana/aves_migration
```


## Test case: 
```
mvn -Dtest=UsersServiceImpl#testCreateUser test
```
