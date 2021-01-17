


## Tomcat 9 install ##

> How to Install Apache Tomcat 9 on Linux

Apache Tomcat is an open source Java server. You need Tomcat when you want to deploy and execute a Java application that is written in any of the Java technologies including Java Servlet, JSP, etc. This document shows how to install the latest Apache Tomcat version 9.x on Linux platform, Install JDK/JRE and deploy a java web application on Tomcat. 
Pre-req: Install Java 8 For Apache Tomcat 9 to be installed and configured properly, you need to have Java version 8 installed on your system. 
To verify the java version on your system, execute the following. 

```sh
$ java -version java version 
```
“1.8.0_131” Java(TM) SE Runtime Environment (build 1.8.0_131-b11) Java HotSpot(TM) 64-Bit Server VM (build 25.131-b11, mixed mod



Create tomcat User If you are using our standard “wasadmin” id su to this id. 
```sh
$ su – wasadmin 
```
Else , as root, create a user called tomcat and assign a password as shown below. 
```sh
$ adduser tomcat $ passwd tomcat 
```
Next, su to this newly created tomcat user. 
```sh
$ su - tomcat
```

### Download Apache Tomcat 9 

Go to Apache Tomcat 9 download page [here][1] 
Under Core, click on “tar.gz” link to download the tar.gz version of the latest tomcat. In this example, we are downloading this file: apache-tomcat-9.0.36.tar.gz

Install Apache Tomcat First, untar the tar.gz file as shown below using tar command under /opt/mw/ 
```sh
$ tar xvfz apache-tomcat-9.0.36.tar.gz
```
This will create a directory apache-tomcat with the version number in it. Change this directory name to tomcat90 
```sh
$ mv apache-tomcat-9.0.36 tomcat90 
$ chown -R wasadmin:wasadmin tomcat90 
$ chmod -R 755 tomcat90
```
cd to this new directory. 
```sh
$ cd tomcat90 
```
Apache Tomcat 9 Directories You’ll see the following files and directories under this apache-tomcat directory. 
```sh
$ pwd /opt/mw/tomcat90 
$ ls -ltr 
```
> total 144  
drwxr-xr-x 2 wasadmin wasadmin 4096 Jun 3 13:10 work  
drwxr-xr-x 2 wasadmin wasadmin 4096 Jun 3 13:10 logs  
drwxr-xr-x 7 wasadmin wasadmin 4096 Jun 3 13:11 webapps -rwxr-xr-x 1 wasadmin wasadmin 16262 Jun 3 13:13 RUNNING.txt -rwxr-xr-x 1 wasadmin wasadmin 6898 Jun 3 13:13 RELEASE-NOTES  
-rwxr-xr-x 1 wasadmin wasadmin 3255 Jun 3 13:13 README.md  
-rwxr-xr-x 1 wasadmin wasadmin 2333 Jun 3 13:13 NOTICE  
-rwxr-xr-x 1 wasadmin wasadmin 57092 Jun 3 13:13 LICENSE -rwxr-xr-x 1 wasadmin wasadmin 5409 Jun 3 13:13 CONTRIBUTING.md  
drwxr-xr-x 2 wasadmin wasadmin 4096 Jun 3 13:13 conf  
-rwxr-xr-x 1 wasadmin wasadmin 18982 Jun 3 13:13 BUILDING.txt drwxr-xr-x 2 wasadmin wasadmin 4096 Jun 24 14:27 temp  
drwxr-xr-x 2 wasadmin wasadmin 4096 Jun 24 14:27 lib  
drwxr-xr-x 2 wasadmin wasadmin 4096 Jun 24 14:27 bin  

Set Apache Tomcat CATALINA_HOME The home environment variable that is used inside the Apache tomcat script is called as CATALINA_HOME. Catalina refers to Tomcat. Set this variable to the full directory of the apache tomcat that we extracted earlier as shown below. 
```sh
$ export CATALINA_HOME=/opt/mw/tomcat90 
```
Apart from setting it on the command line, make sure you add the above line to tomcat/wasadmin’s bash_profile also as shown below. This will make sure this variable is set every time you login as tomcat/wasadmin user. 
```sh
$ vi ~/.bash_profile 
$ export CATALINA_HOME=/opt/mw/tomcat90 
```
Log-out and login as tomcat user to verify that this environment variable is set properly. 
```sh
$ set | grep CATALINA CATALINA_HOME=/opt/mw/tomcat90
```
Startup Apache Tomcat To start Apache tomcat, we’ll use the script called catalina.sh that is located in the bin directory of CATALINA_HOME. So, execute catalina.sh start as shown below to start Apache tomcat on your system. 
```sh
$CATALINA_HOME/bin/catalina.sh start
```
> $ /opt/mw/tomcat90/bin/Catalina.sh start  
Using CATALINA_BASE: /opt/mw/tomcat90  
Using CATALINA_HOME: /opt/mw/tomcat90  
Using CATALINA_TMPDIR: /opt/mw/tomcat90/temp  
Using JRE_HOME: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64/jre/bin/java  
Using CLASSPATH: /opt/mw/tomcat90/bin/bootstrap.jar:/opt/mw/tomcat90/bin/tomcat-juli.jar Tomcat started.  

Use grep command to verify whether the tomcat java process is started and running in the background as shown below. 
```sh
$ ps -ef | grep -i tomcat 
```
> wasadmin 12137 1 1 10:09 pts/3 00:00:03 /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64/jre//bin/java -Djava.util.logging.config.file=/opt/mw/tomcat90/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Djdk.tls.ephemeralDHKeySize=2048 -Djava.protocol.handler.pkgs=org.apache.catalina.webresources -Dorg.apache.catalina.security.SecurityListener.UMASK=0027 -Dignore.endorsed.dirs= -classpath /opt/mw/tomcat90/bin/bootstrap.jar:/opt/mw/tomcat90/bin/tomcat-juli.jar -Dcatalina.base=/opt/mw/tomcat90 -Dcatalina.home=/opt/mw/tomcat90 -Djava.io.tmpdir=/opt/mw/tomcat90/temp org.apache.catalina.startup.Bootstrap start

> **Note:** : If it didn’t start, then you might have to set the JAVA_HOME location properly. Instructions are provided at the end of this document.

View Tomcat Log files On the Tomcat is started, you can view the log files to make sure whether tomcat started properly. This will also tell you whether all your web applications are deployed without any issues. The log file is catalina.out as shown below. 

```sh
$ cd $CATALINA_HOME/logs 
$ cd /opt/mw/tomcat90/logs 
$ more catalina.out 
```
> 25-Jun-2020 10:04:49.644 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server version name: Apache Tomcat/9.0.36  
25-Jun-2020 10:04:49.648 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server built: Jun 3 2020 17:07:09 UTC  
25-Jun-2020 10:04:49.648 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server version number: 9.0.36.0  
25-Jun-2020 10:04:49.648 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Name: Linux  
25-Jun-2020 10:04:49.648 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Version: 3.10.0-1062.18.1.el7.x86_64  
25-Jun-2020 10:04:49.648 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Architecture: amd64  
25-Jun-2020 10:04:49.648 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Java Home: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64/jre 25-Jun-2020 10:04:49.648 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log JVM Version: 1.8.0_242-b08  
25-Jun-2020 10:04:49.649 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log JVM Vendor: Oracle Corporation  
25-Jun-2020 10:04:49.649 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_BASE: /opt/mw/tomcat90  
25-Jun-2020 10:04:49.649 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_HOME: /opt/mw/tomcat90  
25-Jun-2020 10:04:49.651 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.util.logging.config.file=/opt/mw/tomcat90/conf/logging.properties  
25-Jun-2020 10:09:35.597 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory Deployment of web application directory [/opt/mw/tomcat90/webapps/examples] has finished in [207] ms 25-Jun-2020 10:09:35.597 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory Deploying web application directory [/opt/mw/tomcat90/webapps/HelloWorld]  
25-Jun-2020 10:09:35.612 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory Deployment of web application directory [/opt/mw/tomcat90/webapps/HelloWorld] has finished in [15] ms 25-Jun-2020 10:09:35.612 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory Deploying web application directory [/opt/mw/tomcat90/webapps/manager] 25-Jun-2020 10:09:35.632 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory Deployment of web application directory [/opt/mw/tomcat90/webapps/manager] has finished in [19] ms  
25-Jun-2020 10:09:35.632 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory Deploying web application directory [/opt/mw/tomcat90/webapps/ROOT]  
25-Jun-2020 10:09:35.644 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory Deployment of web application directory [/opt/mw/tomcat90/webapps/ROOT] has finished in [11] ms  
25-Jun-2020 10:09:35.648 INFO [main] org.apache.coyote.AbstractProtocol.start Starting ProtocolHandler [“http-nio-8080”]  
25-Jun-2020 10:09:35.657 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in [621] milliseconds wasadmin@elvmae036:TEST:logs>  

Apache Tomcat UI – Default Home Page When everything is up and running, you should be able to go to the following URL To view the tomcat home page. 
http://{your-ip-address}:8080 
http://elvmae036.nwie.net:8080/

###**Listener port**  
By default, Apache tomcat is installed to run on port 8080. If you already have some other application on your system that is running on port 8080, then you can change this by modifying this port value from 8080 to something else in the following server.xml file: 
```sh
vi $CATALINA_HOME/conf/server.xml 
```

###**Environment Variable**  
> - JAVA_HOME 
> - JRE_HOME 

It is important to understand how these two environment variable will affect your Apache Tomcat: If you have multiple Java installed on your system, then you may want Apache tomcat to use a specific version of Java instead of the default version. For this you need to set either your JRE_HOME or JAVA_HOME appropriately. 
> **Note:**

> - JRE_HOME – This is for Java Runtime Environment. Use this environment variable to specify the location of the Java JRE 8 on your system.
> - JAVA_HOME – This is for Java Development Kit. Use this to specify the location of Java JDK 8 on your system.

If you have JDK installed, it is best to use JAVA_HOME, as this will give you additional startup options that are not allowed when you use only JRE. For some reason, if you’ve set both JRE_HOME and JAVA_HOME, then tomcat will use JRE_HOME during startup. The best way to specify these environment variables is in your setenv.sh script which is located under 
```sh
$CATALINA_HOME/bin/setenv.sh 
```
Add your JAVA_HOME environment variable to the setenv.sh file as shown below. 
```sh
$ cd $CATALINA_HOME/bin 
$ vi setenv.sh 
JAVA_HOME= /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64/jre/ 
```
Now when you start Tomcat as shown below, you can see that it is using the JAVA_HOME environment’s value to start the tomcat. 

> $CATALINA_HOME/bin/catalina.sh start  
Using CATALINA_BASE: /opt/mw/tomcat90  
Using CATALINA_HOME: /opt/mw/tomcat90  
Using CATALINA_TMPDIR: /opt/mw/tomcat90/temp  
Using JRE_HOME: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64/jre/  
Using CLASSPATH: /opt/mw/tomcat90/bin/bootstrap.jar:/opt/mw/tomcat90/bin/tomcat-juli.jar Tomcat started.  

> **Note:** Even though it says JRE_HOME in the above output, notice how the value of this is actually the value of JAVA_HOME that we set in the setenv.sh file. Also, from the catalina.out log file, you’ll see the following during startup, which confirms that this is using this new value that we set. 
*INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Java Home: : /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64/jre*

###**Install Java**  
How to Install Java 8 JRE and JDK from RPM file on Linux JRE stands for Java Runtime Environment. JDK stands for Java Development Kit. In most situations, if you want to run a Java application, you just need to install Only JRE. But, if you are doing some development work, or compiling an application that requires Java SDK, then you have to install JDK. This document explains how to install JRE only, JDK only, and both JRE JDK together. 

#### JRE Only  
Download Java 8 JRE only [here][2] is the direct download link for JRE 8 Download
For **64-bit** linux, download the jre-8u131-linux-x64.rpm file, which is under “Linux x64” 
For **32-bit** linux, download the jre-8u131-linux-i586.rpm file, which is under “Linux x86” 
Install Java 8 JRE only on this server, currently there is no java installed. 
```sh
$ java -version 
-bash: java: command not found
$ rpm -qa | grep -i jre 
Install the downloaded jre rpm file as shown below. 
$ rpm -ivh jre-8u131-linux-x64.rpm –test Preparing… ################# [100%]
$ rpm -ivh jre-8u131-linux-x64.rpm Preparing… ################# [100%] Updating / installing… 1:jre1.8.0_131-1.8.0_131-fcs ################# [100%] Unpacking JAR files… plugin.jar… javaws.jar… deploy.jar… rt.jar… jsse.jar… charsets.jar… localedata.jar…
```
Verify to make sure it is installed successfully. 
In this example, as we see, this has installed the 1.8.0 version of java. 
```sh
$ java -version 
java version “1.8.0_131” Java(TM) SE Runtime Environment (build 1.8.0_131-b11) Java HotSpot(TM) 64-Bit Server VM (build 25.131-b11, mixed mode)
$ rpm -qa | grep -i jre jre1.8.0_131-1.8.0_131-fcs.x86_64 
```
#### JDK Only  
Download Java 8 JDK only if you are installing JDK, you typically don’t have to install JRE separately as all the binary files that are included with JRE is also included with JDK. [Here][3] is the direct download link for JDK 8 Download 
For **64-bit** linux, download the jdk-8u131-linux-x64.rpm file, which is under “Linux x64” 
For **32-bit** linux, download the jdk-8u131-linux-i586.rpm file, which is under “Linux x86” 
Install Java 8 JDK Only Install the Java 8 JDK on your system as shown below. 
```sh
$ rpm -ivh jdk-8u131-linux-x64.rpm –test Preparing… ################ [100%]
$ rpm -ivh jdk-8u131-linux-x64.rpm Preparing… ################ [100%] Updating / installing… 1:jdk1.8.0_131-2000:1.8.0_131-fcs ################ [100%] Unpacking JAR files… tools.jar… plugin.jar… javaws.jar… deploy.jar… rt.jar… jsse.jar… charsets.jar… localedata.jar…
```
Make sure that the jdk rpm is successfully installed. 
```sh
$ rpm -qa | grep -i jdk 
jdk1.8.0_131-1.8.0_131-fcs.x86_64 
```
Java 8 JRE and JDK will be installed under  /usr/java directory as shown below. 
> $ ls -l /usr/java/  
lrwxrwxrwx. 1 root root 16 Jun 1 16:55 default -> /usr/java/latest drwxr-xr-x. 9 root root 4096 Jun 1 17:03 jdk1.8.0_131  
drwxr-xr-x. 7 root root 4096 Jun 1 16:55 jre1.8.0_131  
lrwxrwxrwx. 1 root root 22 Jun 1 17:03 latest -> /usr/java/jdk1.8.0_131  

The above ls output indicates that you can install multiple versions of jre or jdk on the same machine, as each and every version of the installation will get its own directory name with the version number in it. The java executable is used from the JRE location (and not from JDK location). When you have multiple java installed, to identify which version of the java executable is used system-wide, do the following: As shown below, the java executable is pointing to /usr/bin/java 
> $ whereis java  
java: /usr/bin/java /usr/lib/java /etc/java /usr/share/java /usr/share/man/man1/java.1.gz  

The /usr/bin/java is really pointing to the java in /etc/alternatives directory. 

> $ ls -l /usr/bin/java  
lrwxrwxrwx 1 root root 22 Apr 19 01:55 /usr/bin/java -> /etc/alternatives/java  

Finally, as you see here, the etc alternatives java is pointing to the java executable from the Java 8 JRE that we installed. 

> $ ls -l /etc/alternatives/java  
lrwxrwxrwx 1 root root 73 Apr 19 01:55 /etc/alternatives/java -> /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64/jre/bin/java  

###**Deployment**
How to deploy a Java web application on Tomcat 
The below two methods are the most common ways about how to deploy a Java web application on Tomcat. 
**1)** **Copying web application archive file (.war).** 
In this method, the web application is packed as a WAR file. You may generate the WAR file using a tool or IDE like Eclipse, or someone just sent you the file. Copy the WAR file into 
```sh
$CATALINA_HOME/webapps 
cp /tmp/HelloWorld.war /opt/mw/tomcat90/webapps 
```
Restart the server. Whenever Tomcat is started, it will unpack the WAR file it found in the webapps directory and launch the application in that manner. 
>$ $CATALINA_HOME/bin/catalina.sh stop  
$ $CATALINA_HOME/bin/catalina.sh start  
wasadmin@elvmae036:TEST: HelloWorld > pwd /opt/mw/tomcat90/webapps/HelloWorld  
wasadmin@elvmae036:TEST: HelloWorld > ls -ltr  
-rwxr-xr-x 1 wasadmin wasadmin  636 Jul 30  2007 index.html  
-rwxr-xr-x 1 wasadmin wasadmin  376 Jul 30  2007 hello.jsp  
drwxr-xr-x 4 wasadmin wasadmin 4096 Jun 25 10:07 WEB-INF  
drwxr-xr-x 2 wasadmin wasadmin 4096 Jun 25 10:07 META-INF  
drwxr-xr-x 2 wasadmin wasadmin 4096 Jun 25 10:07 images  

> **Note:** Later if you want to update changes for the application, you must both replace the WAR 	file and delete the application’s unpacked directory, and then restart Tomcat.
http://elvmae036.nwie.net:8080/HelloWorld/  
 
  **2)**    **Copying unpacked web application directory**
In this method, you have the web application in its unpacked form.
Copy the application’s directory from its location into $CATALINA_HOME/webapps directory.
```sh
cp -r /tmp/HelloWorld /opt/mw/tomcat90/webapps/
```
Restart the server, the application is deployed with the context path is name of the directory 	you copied.

>$ $CATALINA_HOME/bin/catalina.sh stop  
$ $CATALINA_HOME/bin/catalina.sh start  
wasadmin@elvmae036:TEST: HelloWorld > pwd  
/opt/mw/tomcat90/webapps/ HelloWorld  
wasadmin@elvmae036:TEST: HelloWorld > ls -ltr  
-rwxr-xr-x 1 wasadmin wasadmin  636 Jul 30  2007 index.html  
-rwxr-xr-x 1 wasadmin wasadmin  376 Jul 30  2007 hello.jsp  
drwxr-xr-x 4 wasadmin wasadmin 4096 Jun 25 10:07 WEB-INF  
drwxr-xr-x 2 wasadmin wasadmin 4096 Jun 25 10:07 META-INF  
drwxr-xr-x 2 wasadmin wasadmin 4096 Jun 25 10:07 images  
> **Note:** If you want to update changes for the application, you must replace the corresponding 	files under its document root directory.
http://elvmae036.nwie.net:8080/HelloWorld/

  [1]: http://tomcat.apache.org/download-90.cgi
  [2]: https://www.oracle.com/java/technologies/javase-jre8-downloads.html
  [3]: https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html 
    
