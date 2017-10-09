# Jenkins Instruction

This document is an instrcution of how to use Jenkins.

## Prerequisites

* Java 8

## Objectives



### Install Jenkins

Download `Jenkins.war` from https://jenkins.io/download/

```bash
java -jar jenkins.war --httpPort=9090
```

Verify Jenkins, go to browser `localhost:9090`

## Using Jenkins with Tomcat

Download `apache-tomcat-8.5.20.tar.gz` from https://tomcat.apache.org/download-80.cgi

```bash
tar -zxvf apache-tomcat-8.5.20.tar.gz
```

Copy the `Jenkins.war` to folder `apache-tomcat-8.5.20/webapps/`

Run Tomcat

```bash
./apache-tomcat-8.5.20/bin/start.sh
```

Verify Tomcat, go to browser `localhost:8080`

Verify Jenkins, go to browser `localhost:8080/jenkins`

## Jenkins CLI

Open `localhost:9090/cli`, download `jenkins-cli.jar`

```bash
java -jar jenkins-cli.jar -s http://localhost:9090/ --auth sanshou:<my_password>

java -jar jenkins-cli.jar -s http://localhost:9090/ help
```

## Create Users, Manage, and Assign Roles

Open http://localhost:9090/

### Create users

Manage Jenkins --> Manage Users --> Create User

### Active Role-Based Strategy 

Manage Jenkins --> Configure Global Security -->  Role-Based Strategy 

### Create Role

Manage Jenkins --> Manage and Assign Roles --> Manage Roles

e.g: `developer -- Dev.*`

### Asign Role

Manage Jenkins --> Manage and Assign Roles --> Assign Roles

e.g: Add Global roles and Item roles for created users

### Create a new Item

New Item --> 

e.g: named `DevHelloWord`

### Verify the created users

Log out form the admin, log into the created user.

You should see the above created Item

## Getting started with Jobs

### Configure a freestyle project Item

* Source Code Management
* Build Triggers
- Build periodically (H/15 * * * *)
- Trigger builds remotely (JENKINS_URL/job/test4/build?token=TOKEN_NAME) (trigger the job remotely)
* Build --> Execute shell

### Chain job executions

1. Create item1, item2, and item3
2. In item2 --> configure --> Build Triggers --> Build after other projects are built (item1) --> Post-build Actions --> Build other projects (item3)
3. Run item1

## Integration with GIT

1. Create a project in local
2. Test it in Jenkins

* Create an Freestyle item
* Configure execute shell (Linux)

```
cd /home/sanshou/Workspace/JavaProject1
javac hello.java
java hello
```

* run it and check Console Output

3 Git

```
git init

git status

git add .

git commit -m "added hello project"
```

4 Create a project in Github

`https://github.com/wtsanshou/HelloJenkins.git`

5 Push the project to github

```
git remote add origin https://github.com/wtsanshou/HelloJenkins.git

git push -u origin master
```

6 Configure Jenkins

require git plugin

Source Code Management --> git

Repository URL: https://github.com/wtsanshou/HelloJenkins.git

Build Triggers --> Poll SCM --> `* * * * *` (every minute for test purpose)

7 Change the project and push to github

8 You will see a new build history.

## Catlight

https://catlight.io

CatLight is a notification app for developers.

It shows the current status of continuous delivery, tasks and bugs in the project and informs when attention is needed 

1. Download Catlight (No version for Ubuntu 16.04)
2. Install 
3. Congigure Jenkins URL
4. Run jobs and receive notification

## Automated Depolyment

Build ---> Deploy ---> Test ---> Release

1. install plugin `Deploy to container Plugin`
2. Download a war sample file at `https://tomcat.apache.org/tomcat-6.0-doc/appdev/sample/` (you can use your own war file)
3. Configure username and password for Tomcat

/home/sanshou/Jenkins/apache-tomcat-8.5.20/conf/tomcat-users.xml
```
 <user username="deployer" password="deployer" roles="manager-script"/>
```

4 Configure item

Post-build Actions --> Add post-build action --> Build war/ear to a container
```
WAR/EAR files:       **/*.war
        
Context path:       sample
        
        Credentials: Add the username and passward of tomcat
        Tomcat URL : http://localhost:8080/     
```

5 Verify it in tomcat

`http://localhost:8080/sample/`

You should see `Sample "Hello, World" Application` page

## Send Notification

* Manage Jenkins --> Configure System --> E-mail Notification

```
SMTP server: smtp.gmail.com     ## https://www.arclab.com/en/kb/email/list-of-smtp-and-pop3-servers-mailserver-list.html

Use SMTP Authentication 
        
    User Name : weitaoie@gmail.com      
        
    Password  : ************      
        
    Use SSL     
                
    SMTP Port    465  ## Connect to smtp.gmail.com on port 465, if you're using SSL. (Connect on port 587 if you're using TLS.   
        
Test configuration by sending test e-mail :    weitaoie@gmail.com  
                
Email was successfully sent
```

**Note:** enable Gmail `Allow less secure apps: ON`

* set a notification for an item

A project --> configure --> Post-build Actions --> E-mail notification

```
    Recipients  : weitaoie@gmail.com    
        
    ##Whitespace-separated list of recipient addresses. May reference build parameters like $PARAM. E-mail will be sent when a build fails, becomes unstable or returns to stable.    
        
        Send e-mail for every unstable build

```

run a unstable build and check the email


### Another plugins

* Notification Plugin: sending job status notifications in JSON and XML formats
* Extreme Notification Plugin: send web hook
* Email-ext plugin:  

