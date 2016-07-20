#Installation guide for SocIoTal Communities Manager

This guide is intended to provide an easy way to have your own working instance of the SocIoTal Communities Manager (ComM), ready to be linked to other SocIoTal components, such the SocIoTal Context Manager (CM) and the SocIoTal Identity Manager (IdM). The master piece of the SocIoTal ComM is distributed as a WAR file, ready to be deployed on top of a Web Application Server. Distributes version has been tested on Tomcat (Tomcat7) and JBoss (JBoss AS 7.1) but other Web App servers supporting WAR files could be used. This guide will make use of Tomcat (Tomcat version 7).

SocIoTal ComM requires an instance of the SocIoTal IdM to store users. It should be linked to the same instance of the SocIoTal IdM (if exists) used for Capability Management, in order to share the same set of users. If there is no instance of SocIoTal IdM available, this one could be replaced by an available instance of FIWARE KeyRock IdM, either, already running in e.g. FI-LAB or installed by the user. Here you can find information about FIWARE KeyRock and how to install it \[[*http://catalogue.fiware.org/enablers/identity-management-keyrock*](http://catalogue.fiware.org/enablers/identity-management-keyrock)\].

Current installation guide has been run on an Ubuntu Server 14.04 LTS 64 bits (Trusty Tahr). This tutorial has been tested last time on 18/07/2016. Since then, some links might be changed.

Operating System: UBUNTU 14.04 LTE SERVER 64 bits + Base components
===================================================================

This section prepares a basic (and clean) Ubuntu Server 14.04 64 bits existing installation to get the required software to run SocIoTal CM. In particular, this section will install JAVA OpenSDK 1.7 and Tomcat 7. The firs action is to update the system, in order to get access to latest versions of components:

Install JAVA
------------

\# sudo apt-get update

In a terminal, use the following command to install OpenJDK Java Development Kit:

\# sudo apt-get install default-jdk

If everything went fine, checking the installed version should report:

\# java -version

java version "1.7.0\_101"

OpenJDK Runtime Environment (IcedTea 2.6.6) (7u101-2.6.6-0ubuntu0.14.04.1)

OpenJDK 64-Bit Server VM (build 24.95-b01, mixed mode)

OpenJDK 1.7 version is enough to support SocIoTal CM. Later versions of JDK should be also supported, but have not been tested.

Install Apache Tomcat (v7)
--------------------------

This tutorial, and current version of the SocIoTal Cloud Instance, runs with Tomcat version 7. No further tests have been done with later versions of this Web Application Server. SocIoTal CM has been also tested on JBoss AS 7.1.

To install Tomcat7 on our Ubuntu:

\# sudo apt-get install tomcat7

To control (upload wars) remotely:

\# sudo apt install tomcat7-docs tomcat7-admin

Access to the manager application is protected by default: you need to define a user with the role "manager-gui" in **/etc/tomcat7/tomcat-users.xml** before you can access it.

\# sudo vi /etc/tomcat7/tomcat-users.xml

Then write this and save the file

&lt;role rolename="manager-gui"/&gt;

&lt;role rolename="admin-gui"/&gt;

&lt;role rolename="admin"/&gt;

&lt;user username="username" password="password" roles="admin,admin-gui,manager-gui"/&gt;

Finally, restart tomcat service:

\#sudo service tomcat7 restart

### Configure Tomcat7 to support https connections

If you want to configure your Tomcat7 to support https connections (and so the SocIoTal CM), you can find detailed documentation through this link \[[*https://tomcat.apache.org/tomcat-7.0-doc/ssl-howto.html*](https://tomcat.apache.org/tomcat-7.0-doc/ssl-howto.html)\]. A sort direct howto is shown below:

First, get a valid PKCS12 certificate. If you still don’t get one, you can create an auto-signed certificate, useful for testing or trustworthy deployments using OpenSSL \[[*https://help.ubuntu.com/12.04/serverguide/certificates-and-security.html\#creating-a-self-signed-certificate*](https://help.ubuntu.com/12.04/serverguide/certificates-and-security.html%23creating-a-self-signed-certificate)\]

Once you have your .p12 file plus you password, you can configure Tomcat to enable HTTPS using your certificate. For this example, we’re using 8443 port, but other could be used. To do so, edit the tomcat 7 server.xml file:

\#sudo vi /etc/tomcat7/server.xml

Then, write this and save the file

&lt;Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"

maxThreads="200" scheme="https" secure="true"

keystoreType="PKCS12"

keystoreFile="/etc/tomcat7/yourcertfile.p12" keystorePass="yourpasswd"

clientAuth="false" sslProtocol="TLS"/&gt;

Finally, restart tomcat service:

\#sudo service tomcat7 restart

Install SocIoTal Communities Manager
====================================

SocIoTal Communities Manager is presented as a WAR file to be deployed in a Web Application Server. Currently, JBoss AS 7.1 and Tomcat7 have been tested (both successfully) but, initially would be possible to deploy them in any Web AS supporting WARs. The WAR package includes the .jar objects to run all the CM methods documented plus all the required external libraries, in order to ensure its execution in most of the platforms where it could be deployed. The corresponding WAR file can be downloaded from SocIoTal GitHub \[[*https://github.com/sociotal/CommunitiesManager/blob/master/deployment/application/SocIoTal\_COMM\_V1.war*](https://github.com/sociotal/CommunitiesManager/blob/master/deployment/application/SocIoTal_COMM_V1.war)\].

Before deploy it in the Tomcat, its configuration file should be created in /server/SocIoTal\_COMM:

\#sudo mkdir /server

\#sudo mkdir /server/SocIoTal\_COMM

\#sudo vi /server/SocIoTal\_COMM/Configfile

Then, write down the configuration you require for your deployment:

\#SocIoTal Communities Manager Configuration File

\#\#\#\#\# KeyRock/SocIoTal IdM \#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

\#\#\#\# KeyRock/SocIoTal IdM connection parameters: access url, IP/URL, access Port and Protocol (http/https)

PROTOCOL = https

KS\_URL = [*https://sociotal.idm.um.es/proxy*](https://sociotal.idm.um.es/proxy)

KS\_IP = sociotal.idm.um.es

KS\_Port = 443

\#\#\#\# KeyRock/SocIoTal IdM security parameters: Admin Password, Certificates (+ certificate password)

KS\_Admin\_Passwd = KeyRock\_Admin\_Pass

KeystoreFile=/server/certs/idm\_UMU\_cert

KeystorePasswd=changeit

CA\_CERT = /server/certs/ca.cer

\#\#\#\# ROLES IDs: Communities defined roles ID’s (can be obtained from KeyRock/SocIoTal IdM installation

\# owner role and member role are mandatory

admin = XXX

owner = fdde40607d7c4938bd66604f98be7f34

member = 1b7377775e354d65aa464a8cdd727d5e

writer = XXX

reader = XXX

The configuration file links the SocIoTal Communities Manager with the SocIoTal IdM OR the FIWARE KeyRock IdM required to store users and perform communities’ management. The first set of parameters specifies the protocol (http or https), the complete URL to access the selected IdM plus, separated, the IdM IP and port listening, in order to , later, check certificates (if https is selected). Second section of parameters refers to security. Here, the Admin Password of the IdM should be pointed (This will be also the same password used in the corresponding linked SocIoTal CM). You can refer to installation guides of SocIoTal IdM or KeyRock IdM to see how this password is set in their corresponding installation processes. For testing purposes or local deployments, idm\_UMU\_Cert and ca.cer could be used and downloaded from SocIoTal GitHub. Roles IDs point to the ID’s of the different roles required to check Communities actions. These ID’s are created in IdM installation. Owner and member roles are mandatory. Others are optional. You can refer to IdM installation guide to get these ID’s.

Once this is set, you can access Tomcat7 web and upload the SocIoTal CM WAR file and start it. This link shows you how to do it using Tomcat7 \[[*https://tomcat.apache.org/tomcat-7.0-doc/manager-howto.html\#Deploy\_A\_New\_Application\_Archive\_(WAR)\_Remotely*](https://tomcat.apache.org/tomcat-7.0-doc/manager-howto.html%23Deploy_A_New_Application_Archive_(WAR)_Remotely)\].

Now, you can refer to WiKi SocIoTal Communities Manager and start using the methods there described.
