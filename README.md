# onedot-remote-task
Proposed solution for the remote task required as part of my application for the Devops Engineer Intern position at Onedot.

Description of the task:

We are going to take a basic Node.js example application and deploy it on AWS using Cloudformation. The goal is to deploy the Node.js
application in a scalable way behind a load balancer. From now on, we will call the mock web application 'helloworld-node-app'

Instructions:

1) Local deployment of the 'helloworld-node-app' in your workstation (Only a Linux workstation with Docker and git installed is needed) 
[Neither AWS nor Swarm orchestration enabled]

- $ git clone https://github.com/dvalderac/onedot-remote-task.git
- $ cd onedot-remote-task/
- $ docker compose up -d (it will use the docker-compose.yml to build the images and create the services/containers)
- $ curl localhost:8080 --> Hello World! (from within your workstation)
- $ Use a web browser request the 'helloworld-node-app' --> <your-machine-public-ip>:8080 --> Hello World!
- $ docker-compose down (to stop and remove all containers)

2) Deployment of the 'helloworld-node-app' in a cloud-based, scalable and fault-tolerant way using Docker CE for AWS to create 
a Swarm cluster infrastructure (vía CLI)

- Create and configure your AWS free tier account 
- Configure aws CLI in your worstation
- Run this command (enter your desired values to configure the cluster)

(the template can be found in this repo - Docker4AWS-cloudformation-template/DockerStack-cloudformation.json 
or just in the official Docker for AWS site)

$ aws cloudformation create-stack --stack-name teststack 
--template-url <templateurl> --parameters ParameterKey=<keyname>,
ParameterValue=<keyvalue> ParameterKey=InstanceType,ParameterValue=t2.micro 
ParameterKey=ManagerInstanceType,ParameterValue=t2.micro ParameterKey=ClusterSize,ParameterValue=1 .... --capabilities CAPABILITY_IAM

- Secure copy the docker-compose-AWS-deployment.yml file to the swarm manager node:

$ scp -i <yourkeypair.pem> docker-compose-AWS-deployment.yml docker@<ManagerPublicIP>:

- Secure login to the swarm manager node:

$ ssh -i <yourkeypair.pem> docker@<managerPublicIP>

- Deploy the web node app as a docker stack

$ docker stack deploy --compose-file docker-compose-AWS-deployment.yml helloworld-node-app

- Explicitly expose the services ports by updating port mappings

$ docker service update –publish-add published=80,target=8080 helloworld-node-app
- Use docker, docker node, docker swarm and docker service commands to manage the cluster and scale the services.

To remove the whole stack and the docker-swarm infraestructure from your AWS account, just go to Cloudformation, Stacks 
and remove the stack we have just created. 

AWSome, AWSome, AWSome! 
