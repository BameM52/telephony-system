# telephony-system
In our environment we are using terraform as IAAC and since we have a team of engineers working together, our state files are backed up in s3 backbends 
We are equally using KMS (key management service) service to encrypt it at rest.
We are equally using Dynamo DB tables to luck the state files such that no two infrastructure engineers can access the state file at the same time as this will cause infrastructure inconsistency
if this happens it will cause a serous problem since we are managing critical applications and security is inherent in our environment
In Dominion System, we are managing JAVA based enterprise and web applications for fintech and e-commerce client. These applications have a multi-tier infrastructure where we have the loadbalancer-tier, application tier and database-tier. In the front end, we have configure DNS record so that our endusers will access our application using the loadbalancers DNS name. Therefore we have configure a hostname in route53 so that for instance when endusers type app.com. this domanin name will be pointing to our load balancer, the loadbalancer will talk to our application and application will talk to our databased. 
It is also important to note, our applications are using in kubernetes clusters, we have a self managed and a managed clusters that we have configured in our environment, particularly we have self manage cluster using kubeadm and aws elastic kubernetes services.
In configuring our entire environment, security is inherent, now let me take you a little bit on how our pipeline is configure because everything is automated using a jenkins job. In our environment we have different jobs
We have a job where Jenkins build and deploy directly to the kubernetes cluster and we have a job where jenkins builds and ship to Ansible and Ansible manages the deployment
In this Jenkins job, immediately developers commit a new version of the code to our version control system or project repository in GitHub 
Like I said before, security is inherent and before any code is commited to our master branch, it must go through a pull request and everything has been configured as such because of security.
once the code is commited to the master branch, we have configured build triggers using github webhook, pull SCM and build periodically depending on the nature of the job because jobs like database backup need build periodically.
So once there is a commit in github, a jenkins job is triggered, jenkins is going to pull the new version of the code because any commitID is a new version of the code and since our projects are Java-based, we are using Maven to do a build. There jenkins is integrated with Maven to execute the clean package goal where the code will be validated, compile and test cases run and if it passes,packages or build artifacts will be created

Because of how sensitive our applications are, software quality is our top priority, we have intruduce Sonarqube for code quality. Sonarqube is going to execute code quality report
In Dominion System we have the following bench marks or threshold or quality gate for code quality  
- There is no bug in the code 
- There is no security vulnerabilities because if there is security vulnerabilities hackers can hack our applications
- If there are any duplicate lines, it must not exxceed 5%
- code coverage on new code must be above 90%. In some other projects code coverage can be 80% and it is acceptable but since we are managing fintect clients, code coverage must not go below 90%

Once the sonarqube report is executed and everything is ok, our artifacts are uploaded to nexus to ensure that if there is any disaster, we can alway retrieve our artifacts so nexus is acting as our maven remote repository for our projects

once artifact are uploaded in to nexus, we run additional testings to know how the new version of the application is performing like functional testing, penetration testing, regression testing and load testing.

If all these test passes and we are satisfied, we are going to deploy the application to production.
In doing that there are two ways
In some of our projects, Jenkins with the help of a Dockerfile, dockerise the application and a docker image is created. 
At this level we configured the Dockerfile to pull some images which are base images(Jboss and or Tomcat). We pulled the images and scan to make sure that the base image that we want to copy our code into is free from Vulnerabilities and SQL injections etc.
At this point, we dockerise the application and push to the image registery in Dockerhub or Amazone ECS.
From this point the application will be deployed to a K8s cluster. 
From the image registry, the jenkins jon will deploy the application to a K8s cluster. NOTE. that the jenkins integration from end to end using plugins. With all of these we have deployed our application to production and since the application is three tier and endusers talk to our application using the Loadbalancer and loabalancer is pointing to a DNS name or hostname like app.com.

Since some of these services are micro services, we have configure and Ignix ingress because with the help of iginx increase we configure;
- ssL termination. This implies that end users will access this application from an Https protocol
- ssl redirect
In addition to that inginx ingress provide an additional layer of security and it is important to note like i said earlier on that we are very heavy on security on our entire infrastructure

At the level of the Jenkins pipeline we have securities in place like rbac, project based authorisation, we equally use AWS IAM role to ensure to sure that our environment is secured, 
we also use ldap integration to ensure that our environment is completely secured


At the level of our cluster security is very heavy and that is why we use namespace concept to restrick who can have access to the cluster with the help of cluster role and role binding to achieve this this.

Interms of our overall infrastructure, our entire infrastructure runs on private sub netnet, We use terraform as IAAC to provision our infrastrcture in the cloud
.
We have our frondend running in the public subnet so in our VPC setting we use nac for security at the sub net 

In some of the projects recently we have a senarior where when jenkins have done all the testing, the application is ready for deployment, jenkins with the help of plugins will ship the application to ansible. Jenkins will trigger a set of ansible playbooks

In our environment, there are seven key concept that we want 

 








