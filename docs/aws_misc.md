### Organizations
![Organizations](https://s3.amazonaws.com/hfcontents/kbimages/Organization.png "Organizations")
- Account Management service that enables you to consolidate multiple AWS accounts into an Organization that you create and centrally manage
- Available in two feature sets:
    - Consolidated billing
	- All features

#### Consolidated billing
![Consolidated_Billing](https://s3.amazonaws.com/hfcontents/kbimages/Consolidated_Billing.png "Consolidated_Billing")
![Consolidated_Billing1](https://s3.amazonaws.com/hfcontents/kbimages/Consolidated+Billing1.png "Consolidated_Billing1")

![Consolidated_Billing2](https://s3.amazonaws.com/hfcontents/kbimages/Consolidated_Billing2.png "Consolidated_Billing2")
- One bill per AWS account
- Easy to track charges and allocate costs
- Volume pricing discount
- 20 linked account only
- As a best practice:
	- Turn-on CloudTrail in the paying account
	- Create a bucket policy that allows cross account access
	- Turn-on CloudTrail in the other accounts and use the bucket in the paying account

### Cross Account Access
- Identify Account numbers
- Create a Group in IAM (Developer) -Dev
- Create User in IAM (John) - Dev
- Log-in to Production
- Create "read-write-app-bucket" policy
- Create "UpdateApp" Cross account role
- Apply newly create Policy to the Role
- Login to Developer Account as root user
- Create new inline policy
- Apply inline policy to Developer group
- Login as John
- Switch Account

### Tags
- Key Pair value attached to AWS resources
- Metadata
- Tags can be inherited (Autoscaling, CloudFormation, Elastic Beanstalk)

### Resource Groups
- Makes it easy to group resources based on Tags
- Resource Group contains information as Region, Name, Health Checks
- Specific information:
    - EC2: Public and Private IP address
	- Port configurations
	- Database Engine
- Types:
	- **Classic Resource Groups**: Global, spans across regions
	- **AWS System Manager**: Per region, Can execute commands against these RG (E.g. Create image of EC2 instance within this group)

### Security Token Service (STS)
- Grants users limited or temporary access to AWS resources.
- Users can come from 3 sources:
	- Federation (typically Active Directory)
		- Uses Security Assertion Markup Language (SAML)
		- Grants temporary access based off the users active directory credentials. Does not need to an user in IAM
		Single sign-onallows users to log into AWS console without assigning AWS credentials
	- Federation with mobile apps
		- Use Facebook/Amazon/Google or other OpenID providers to login
	- Cross Account Access
		- Lets User from one AWS account access resources in another

#### STS Key terms
- **Federation**
	- Combining list of users from one domain (IAM) with the list of users in another domain (AD, Facebook)
- **Identity Broker**
	- A service that allows you to take identity from Point A and join/federate it with Point B
- **Identity Store**
	- Services like AD, Facebook, Google
- **Identites**
	- User of a service like Facebook

### Active Directory (AD) Authentication
![AD_Auth](https://s3.amazonaws.com/hfcontents/kbimages/AD_Auth.png "AD_Auth")

### Workspaces
- Cloud based replacement for traditional desktop
- Available as a bundle of compute resource, storage space and software application access that allows users to perfom day-to-day task just like traditional desktop
- You do not need AWS account to access workspace
- WIndows 7 experience provided by Windows 2008 R2
- By default you are given local administrator access
- Workspaces are persistant
- All data is D:\ and is backed up every 12 hours

### Docker
- Software platform that allows you to build, test and deploy application quickly
- Highly **Reliable**: you can quickly deploy and scale applications into any environment and know your code will run
- Infinitely **Scalable**: Running docker in AWS is a great way to run distributed applications at any scale
- Docker packages applications into standardize units called Containers

#### Containers
- Containers are method of operating system virtualization that allow you to run an application and its dependencies in resource-isolated process
- Contains everything that a software needs to run - libraries, system tools, runtime and code
- Created from read-only template called an image
- Allow you to easily package applications code, configuration, dependencies into easy to use building blocks that deliver environmental consistancy, operational efficiency, developer productivity and version control of softwares/dependencies packages in container
- Escape from dependency hell
- Consistant progression from Dev > Test > QA > Prod
- App isolation: Perdormance or stability issues with App A in Container A will not impact App B in Container B
- Better resource management (You dont have to worry about Guest OS consuming 80% of the resources)
- Extreme code portability ("shipping containers")
- Micro services

#### Components
 - Docker Image (Similar to iso/ami, but only contains required components making it lightweight)
 	- Read-only template with instructions for creating Docker Container. It contains an ordered collection of root filesystem changes and corresponding execution parameters for use within a container runtime
	- An Image is created from DockerFile - aplain text file with instructions as to what all components that needs to be included in the container
	- Images are stores in registery - Docker Hub or AWS ECR

#### EC2 Container Registery (ECR):  
	- Managed AWS Docker registery service that is secure, scalable and reliable
	- Supports private docker repositories with resource based permissions using AWS IAM so that specific users or Amazon EC2 instances and access repositories and images
	- Developers can use Docker CLI to push, pull and manage images

 - Docker Container (holds everything that is needed to run the application, isolated and secure application platform)
 - Layers / Union File system
 	- Docker images are read-only templates from which containers are launched
	- Each image consists of series of layers
	- Docker makes used of Union File system to combine the layers into a single image
	- Union File System allows Files and Directories of seperate file system known as branches to be transparently overlaid forming a single coherent file system
	- Layers make docker lightweight
	- For example if you were to update an application to a new version a new layer is going to be built and thus rather than replacing the whole image or entirely rebuilding as you may do with a virtual machine only that layer is added or updated. So now you don't need to distribute a whole new image to do the update.
 - DockerFile
 	- Images are built from base image using simple and descriptive set of steps called instructions. These instructions create new layer in the image
	- Instructions include actions like RUN as command, ADD a file or directory or CREATE an environment variable
	- These instructions are stored in DockerFile and docker reads this file when you request to built an image, executes these instructions and returns a final image
	
	- Each instruction creates new layer 
 - Docker Daemon/Engine
 	- Runs on linux
	- Host daemon communicates with the Docker client to execute commands to build, ship and run containers
 - Docker Client
 	- Interface between you and the Docker Engine and allows creation, manipulation and deletion of Docker containers and control of Docker daemon
 - Docker repository/ Docker hub
 	- Hold Docker Images
	- There are public or private stores from which you upload and download images
	- Public Docker registery is provided with Docker and it servers as huge collection of images created by you or others

### Virtualisation vs Containerisation
![Virtualization_vs_Containerization](https://s3.amazonaws.com/hfcontents/kbimages/Virtualization_vs_Containers.png "Virtualization_vs_Containerization")

- Traditional Virtual Machine
	- Guest OS uses 80% of resources
	- Density compromises
- Containers
	- Light weight, only contains application and dependecies (bare minimum required to run the application
	- Doesn't have to package copy of OS)
	- Higher density and improved portability by removing per container Guest OS
	- Almost 100% of it is used by the application

### Elastic Container Service (ECS)
- Amazons managed service for Docker
- Highly scalable, fast, container management service that makes it easy to Run, Stop and Manage Docker Containers on a cluster of EC2 instances
- ECS enables you to launch and stop container based applications with simple API calls, allows you to get state of your cluster from centralized service, and gives you access to many familiar EC2 features
- Regional service that you can use in one or more AZs across a new or existinc VPC, to schedule placement of containers across your clusters based on your resource needs, isolation policy and availability requirement
- ECS eliminates the need for you to operate your own cluster management and configuration management systems or to worry about scaling your management infrastructure
- Can also be used to create consistant build and deployment experience, manage and scale ETL and batch workloads and build sophisticated application architecture on a microservice model

#### ECS Task Definition
- A Task Definition is required to run Docker Container in EC2
- Task Definition is a JSON file that describes one or more containers that form your application
- Some of the paramters that you can specify in Task Definition include:
	- Which Docker images to use with the containers in your task
	 - How much CPU and Memory to use with Docker Container
	- Wheather Containers are linked togather in a task
	- Docker networking mode to use in your task
	- What (if any) ports are mapped from container to host instance
	- Whether the Task should continue to run if container finishes or fails
	- Command the Container should run when it is started
	- Whether nay environment variables should be passed to the container when it starts
	- Any data volumes that should be used with the containers in the Task
	- If any, IAM role your tasks should use for permissions

#### ECS Service
- ECS service allows you to run and maintain specified number of instances of task definition simultaneously in ECS cluster
- Think os Services like Auto-scaling group for ECS
- If a Task should fail or stop, ECS scheduler launches another instance of your Task Definition to replace it and maintain the desired count of Task instances in the service

#### ECS Clusters
- Logical grouping of Container instances that you can place Tasks on
- When you first use Amazon ECS service a default cluster is created for you. However you can create multiple Clusters to keep your resources seperate
- Clusters can contain multiple different Container instance
- Clusters are region specific
- Container instance can belong to only one Cluster at a time
- You can create IAM policies for your Clusters to restrict or allow users access to specific Clusters

#### ECS Scheduling
- **Service Scheduler**
	- Ensures that specified number of Tasks are always running and reschedules Tasks when task fails
	- Can ensure tasks are registered against ELB
- **Custom Scheduler**
	- You can create your own scheduler to meet your business needs
	- Leverage third-party scheduler as Blox
- ECS Scheduler leverage same cluster state information provided by Amazon ECS API to make appropriate placement decision

#### ECS Container Agent
- Allows Container instances to connect to your Cluster
- Included in Amazon ECS optimized AMI but can be installed on any EC2 instance that supports ECS specification
- Supported only on EC2 instances
- Linux based. Will not work on Windows

#### ECS Security
- IAM Roles
	- EC2 instances can use an IAM role to access ECS
	- ECS Tasks use IAM role to access services and resources
- Security Groups attached at instance level
- You can access and configure OS of EC2 instances in your ECS Cluster

#### ECS Limits
- Soft limits
	- Cluster per region (Default 1000)
	- Instances per Cluster (Default 1000)
	- Services per Cluster (Default 500)
- Hard Limits
	- One Load Balancer per service
	- 1000 Tasks per Service (Desired Count)
	- Max 10 Containers per Task Definition
	- Max 10 Tasks per instances (host)
- Hard limits
