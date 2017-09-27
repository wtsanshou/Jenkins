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