This project is a java based web application.  
We can use Maven to build this project.  
In this project, we have created a Jenkinsfile, which can be used in a pipeline or multibranch pipeline job

# Project Title: Building a CI/CD Pipeline with Jenkins

## Project objective: 

Use Jenkins to set up a CI/CD pipeline that will compile and test a Maven project and deploy it to a Tomcat Server.

- Background of the problem statement: 

You’re a DevOps engineer at Pied Piper, a software company that provides web and mobile app development services. Your team is tasked with building a web app for an online book shop named Bookzy. The developers have decided to use Springboot to generate the project, Maven for compilation, and Git as the Source Code Management system for the project. You’re required to set up a Jenkins pipeline that involves three different freestyle jobs for code compilation, testing, and deployment respectively. Your Pipeline has to poll the SCM nightly for commits, build the project, and trigger the test job if the build is stable. The test job has to run unit tests, publish the JUnit test report, and trigger the freestyle job for deployment. The deployment job is required to package the Maven project, deploy the war file to a Tomcat server, and notify you via email if deployment failed.

You must use the following: 

●	Java: To create the website

●	Git: As a version control system for the program

●	Jenkins: To create the build pipeline

●	Spring boot: To create the Maven app

●	Maven: To compile the program

●	Tomcat: To host the website

●	AWS EC2: To run Tomcat

●	JUnit: To run tests and publish results

#### The following requirements should be met: 

●	The app should be built with Maven.

●	The Tomcat server should allow remote deployment.

●	The pipeline should consist of three freestyle jobs for compilation, test, and deployment respectively.

Prerequisite:

●	To create the Maven project, go to start.spring.io/ and select Maven as the project type. Enter Group and Artifact values and click on Generate.


# Solution 


## Part 1: Set User Permissions for Jenkins and Tomcat


## Apache Tomcat
- [ ]  How to Install Tomcat
```
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.76/bin/apache-tomcat-9.0.76.tar.gz
```
- [ ]  Untar using the below command

```
tar -xvf apache-tomcat-9.0.76.tar.gz
```
- [ ] Move to the Apache unzipped folder

```
 cd apache-tomcat-9.0.76
 ```

- [ ]  Add the below lines
```
vi ./conf/tomcat-users.xml

 <role rolename="admin-gui,manager-gui,manager-script,manager-jmx,manager-status,admin-gui"/>
  <user username="admin" password="admin" roles="manager-gui,admin-gui,manager-script"/>
  <user username="robot" password="admin" roles="manager-script"/>
```

- [ ] Comment two lines in context.xml or remove 
```
vi  ./webapps/manager/META-INF/context.xml 

<!-- <Valve className="org.apache.catalina.valves.RemoteAddrValve"
  allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->
```
- [ ] Stop and start Tomcat
```
./bin/shutdown.sh
./bin/startup.sh
```
- [ ] Check your tomcat if accessible from UI

```
ip:port
```
![image](https://github.com/sdeshwal78/Java-sample-app/assets/137603560/e317bb53-9ca2-41ee-ba27-56a95af735b0)


----
Now you can also perform the deployment from there 


## Log in to this URL 

http://52.91.26.98:8090/manager/html

# Jenkins CI/CD Project for Java Spring Boot Tomcat

This README file contains the necessary steps for setting up a Jenkins CI/CD pipeline for a Java Spring Boot project using Tomcat as the application server. We will be using the sample project at (https://github.com/sdeshwal78/Java-sample-app.git)
![image](https://github.com/sdeshwal78/Java-sample-app/assets/137603560/f8b76d80-a807-4623-9e1c-0a05bc0bf2bf)



## Part 2: Create a Jenkins Job for Deployment with the Deploy to Container Plugin

1. Go to the Jenkins dashboard and click on 'New Item'.
2. Enter a name for your build job and select 'Freestyle project' as the build job type.
3. In the configuration page, scroll down to the 'Source Code Management' section and select 'Git'.


```
Repository URL : https://github.com/sdeshwal78/Java-sample-app.git 
```
4. Install and configure the 'Deploy to Container' plugin:
- Go to 'Manage Jenkins' > 'Manage Plugins'.
- Click on the 'Available' tab and search for 'Deploy to container'.
- Select the 'Deploy to container Plugin' checkbox and click on 'Install without restart'.

5. Configure the build triggers, build steps, and post-build actions for the deployment job:
- Build Triggers: Choose the desired build trigger, such as 'Build after other projects are built', 'Poll SCM', or 'Build periodically'.
- Build Steps: 
    - Click on 'Add build step' and select 'Invoke top-level Maven targets'.
    - Set the 'Maven Version' and enter 'clean package' in the 'Goals' field.
- Post-build Actions:
    - Click on 'Add post-build action' and select 'Deploy war/ear to a container'.
    - In the 'WAR/EAR files' field, enter '**/*.war'.
    - Click 'Add Container' and choose 'Tomcat 9.x'.
    - Enter the 'Tomcat URL' (e.g. http://52.91.26.98:8090/).
    - Add the credentials for the Tomcat manager (username and password).
    - Set the 'Context path' for the deployed application (e.g. Bookzy).

6. Save the configuration and click on 'Build Now' to start the deployment job.
![image](https://github.com/sdeshwal78/Java-sample-app/assets/137603560/37726793-ae05-41d5-9f67-751b4f041823)

![image](https://github.com/sdeshwal78/Java-sample-app/assets/137603560/44d0b936-f470-4911-a2f2-785f447e5008)

![image](https://github.com/sdeshwal78/Java-sample-app/assets/137603560/04ab33be-0b6c-4098-b7fb-9614e23ec0e6)



![image](https://github.com/sdeshwal78/Java-sample-app/assets/137603560/64edb906-a9d1-42f9-ac77-03fc4371f79f)


 - [ ] Check your Application at the context name given
```
ip:8080/app  e.g. http://52.91.26.98:8090/Bookzy
```

 - [ ]  Now use the given code to deploy from JenknsFile
 ![image](https://github.com/sdeshwal78/Java-sample-app/assets/137603560/ee7fa6f3-eb23-4a93-81f0-68f1fcec0f47)
      
![image](https://github.com/sdeshwal78/Java-sample-app/assets/137603560/9bde16a7-c7b5-4c15-b0cd-4831b635463a)
- [ ] Deployed on Remote Tomcat Server
      ![image](https://github.com/sdeshwal78/Java-sample-app/assets/137603560/2a15f310-7f76-4065-89db-f523a4f9ab5a)

