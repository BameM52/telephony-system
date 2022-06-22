# telephony-system
In our environment we are using terraform as IAAC and since we have a team of engineers working together, our state files are backed up in s3 backbends 
We are equally using KMS (key management service) service to encrypt it at rest.
We are equally using Dynamo DB tables to luck the state files such that no two infrastructure engineers can access the state file at the same time will bcause can cause infrastructure inconsistency
if this happens it will cause a serous problem since we are managing critical applications and security is inherent in our environment
In Dominion System, we are managing JAVA based applications enterprise and web applications for fintech and e-commerce client. These applications have a multi-tier infrastructure where we have the loadbalancer-tier, application tier and database-tier. In the front end, we have configure DNS record so that our endusers will not access our application using the loadbalancers DNS name. Therefore we configure a hostname in route53 so that for instance when endusers type app.com. this domanin name will be pointing to our load balancer, the loadbalancer will talk to our application and application will talk to our databased. 
It is also important to note, our applications are using in kubernetes clusters, we have a self managed and a managed clusters that we have configured in our environment, particularly we have self manage cluster using kubeadm and aws elastic kubernetes services.
In configuring our entire environment, security is inherent, now let me take you a little bit on how our pipeline is configure because everything is automated using a jenkins job. In our environment we have different jobs




