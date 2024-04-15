# fso-roadshow-lab

## Lab Guide

### [1] Login to Cloud Academy and start Lab
https://cloudacademy.com/login/
Login with your company ID
 
Cisco.sso.cloudacademy.com
Use Cisco ID to log in
Search for AppDynamics
Select Lab:
PQS Lab 2.1 - Java - Core Application Monitoring
>> Start Lab

### [2] Download PEM and Connect to AWS Instance
Download generic PEM
chmod 400 generic.pem (Permissions for SSh Keyfile)
ssh -i ~/.ssh/generic-vm.pem centos@ec2-34-212-33-135.us-west-2.compute.amazonaws.com

### [3] Download latest agent
cd /home/centos/artifacts/
./getAgent.sh javaagent
unzip /home/centos/artifacts/AppServerAgent.zip -d /home/centos/agents/java-agent

### [4] Configure Controller Connection
cd /home/centos/agents/java-agent/ver22.4.0.33722/conf/
vi controller-info.xml
Configs to change:
controller-host = ceer.saas.appdynamics.com
controller-port = 443
controller-ssl-enabled = true
application-name = Any name you like but recommend myFirstApp-CI-<YOUR-LASTNAME>
account-name = ceer
account-access-key = 3gnmajormowm

## [5] Attach Java Agent to JVMs
cd /home/centos/labs/myFirstApp-lab
vi start.sh
nohup java -javaagent:/home/centos/agents/java-agent/javaagent.jar -Dappdynamics.agent.tierName=Tier1 -Dappdynamics.agent.nodeName=Node1 -DconfigFile=./config/tier1.json -jar ./java-services.jar > $FILEOUT 2>&1 &
Change for all 5 Java Processes, give unique names for Tiers and Nodes

## [6] Start the JVMs and Login to Controller
./start.sh
https://ceer.saas.appdynamics.com/controller
Login with
Account: ceer
User: appd@s3cret-mail.de
PW: AppDL0g1n!
Applications --> Your Application Name


 


