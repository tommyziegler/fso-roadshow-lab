# fso-roadshow-lab

## Lab Guide

### [1] Login to Cloud Academy and Start Lab
- **URL for Login**: [Cloud Academy Login](https://cloudacademy.com/login/)
- **Company ID**: Use your Cisco ID to log in at [Cisco SSO for Cloud Academy](https://Cisco.sso.cloudacademy.com)
- **Procedure**:
  - Search for "AppDynamics"
  - Select the Lab titled "PQS Lab 2.1 - Java - Core Application Monitoring"
  - Click on "Start Lab"

### [2] Download PEM and Connect to AWS Instance
- **Download the PEM file**:
  - File name: `generic.pem`
- **Set Permissions for SSH Keyfile**:
  - Run: `chmod 400 generic.pem`
- **SSH Connection Command**:
  - Connect using: `ssh -i ~/.ssh/generic-vm.pem centos@ec2-34-212-33-135.us-west-2.compute.amazonaws.com`

### [3] Download Latest Agent
- **Navigate to Artifacts Directory**:
  - Command: `cd /home/centos/artifacts/`
- **Download the Java Agent Script**:
  - Run: `./getAgent.sh javaagent`
- **Unzip the Java Agent**:
  - Command: `unzip /home/centos/artifacts/AppServerAgent.zip -d /home/centos/agents/java-agent`

### [4] Configure Controller Connection
- **Change Directory**:
  - Command: `cd /home/centos/agents/java-agent/ver22.4.0.33722/conf/`
- **Edit Configuration File**:
  - Command: `vi controller-info.xml`
- **Configurations to Update**:
- ```
  controller-host = ceer.saas.appdynamics.com
  controller-port = 443
  controller-ssl-enabled = true
  application-name = Recommend using "myFirstApp-CI-<YOUR-LASTNAME>"
  account-name = ceer
  account-access-key = Insert your key here
  ```
### [5] Attach Java Agent to JVMs
- **Navigate to Application Lab Directory**:
  - Command: `cd /home/centos/labs/myFirstApp-lab`
- **Edit Startup Script**:
  - Command: `vi start.sh`
- **Script Modification for Java Agent**:
  - Update the command to include:
    ```
    nohup java -javaagent:/home/centos/agents/java-agent/javaagent.jar -Dappdynamics.agent.tierName=Tier1 -Dappdynamics.agent.nodeName=Node1 -DconfigFile=./config/tier1.json -jar ./java-services.jar > $FILEOUT 2>&1 &
    ```
  - Apply unique names for Tiers and Nodes for all 5 Java Processes

### [6] Start the JVMs and Login to Controller
- **Start Java Processes**:
  - Run: `./start.sh`
- **Controller Login**:
  - **URL**: [AppDynamics Controller](https://ceer.saas.appdynamics.com/controller)
  - **Credentials**:
    - **Account**: ceer
    - **User**: Insert your email here
    - **Password**: Insert your password here
- **Navigation**:
  - Go to "Applications" and select "Your Application Name"
