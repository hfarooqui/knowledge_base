# Concepts

### Block storage
Traditional block storage device — like a hard drive — over the network

Cloud providers often have products (volumes) that can provision a block storage device of any size and attach it to your cloud instance.

You can run your own using OpenStack Cinder, Ceph, or the built-in iSCSI service available on many NAS devices

#### Advantages:

1. You can easily take live **snapshots** of the entire device for backup purposes
2. Block storage devices can be **resized** to accommodate growing needs
3. You can easily **detach and move** block storage devices between machines
4. Block storage devices provide **low latency IO**, so they are suitable for use by databases.
5. Block devices are **well supported**. Every programming language can easily read and write files
6. Well suited for storing data in **traditional databases**. Additionally, many legacy applications that require normal **filesystem storage** will need to use a block storage device.

#### Disadvantages:

1. Storage is **tied to one server** at a time
2. Blocks and filesystems have **limited metadata** about the blobs of information they're storing (creation date, owner, size). Any additional information about what you're storing will have to be handled at the application and database level, which is additional complexity for a developer to worry about
3. You need to **pay for all the block storage** space you've allocated, even if you're not using it
4. You can only access block storage through a **running server**
5. Block storage needs **more hands-on work and setup** vs object storage (filesystem choices, permissions, versioning, backups, etc.)


------------

### Object storage
Object storage is the storage and retrieval of unstructured blobs of data and metadata using an HTTP API.  Instead of breaking files down into blocks to store it on disk using a filesystem, we deal with whole objects stored over the network.

These objects could be an image file, logs, HTML files, or any self-contained blob of bytes. They are unstructured because there is no specific schema or format they need to follow.

Object storage is useful for hosting static assets, saving user-generated content such as images and movies, storing backup files, and storing logs

There are some self-hosted object storage solutions, though you will give up some of the benefits of a hosted solution (such as not having to worry about hard drives and scaling issues). You could try Minio, a popular object storage server written in the Go language, or Ceph, or OpenStack Swift.

#### Advantages:

1. A simple **HTTP API**, with clients available for all major operating systems and programming languages
2. Object storage services charge only for the storage space you use
3. Some object stores offer **built-in CDN integration**, which cache your assets around the globe to make downloads and page loads faster for your users
4. Optional **versioning** means you can retrieve old versions of objects to recover from accidental overwrites of data
5. Object storage services can easily **scale** from modest needs to really intense use-cases without the developer having to launch more resources or rearchitect to handle the load
6. Using an object storage service means you **don't have to maintain hard drives and RAID arrays**, as that's handled by the service provider
7. Being able to store chunks of metadata alongside your data blob can further **simplify your application architecture**


#### Disadvantages:

1. You can't use object storage services to back a traditional database, due to the **high latency** of such services
2. Object storage **doesn't allow you to alter just a piece of a data blob**, you must read and write an entire object at once. This has some performance implications. For instance, on a filesystem, you can easily append a single line to the end of a log file.
On an object storage system, you'd need to retrieve the object, add the new line, and write the entire object back. This makes object storage less ideal for data that changes very frequently
3. Operating systems **can't easily mount an object store like a normal disk**. There are some clients and adapters to help with this, but in general, using and browsing an object store is not as simple as flipping through directories in a file browser

### Links:
- https://acloud.guru/ for questions
- https://aws.amazon.com/free/
- https://aws.amazon.com/certification/certified-solutions-architect-associate/


## AWS

1300+ services

### AWS Global InfrastructureAWS Global Infrastructure

#### Region
Region is a **physical location** in the world which consists of 2 or more AZ (around 60)

#### AZ
- AZ is one or more discrete data center each with **redundant power, networking and connectivity** housed in seperate facility.
- One subnet One AZ

#### Edge locations
- Edge locations are simply **endpoints** for AWS which are used for caching content. Typically this consists of ** CDN, CloudFront**. There are more Edge locations than region (around 96)

# Services

#### Compute

- **Elastic Compute Cloud** (EC2)
Can have VM as well as physical dedicated machine

- **EC2 Container service**
Manage docker container at scale

- **Elastic BeanStalk**
For dev who don't understand AWS. They would upload their code and EBS would provision Autoscailing groups, load balancers, EC2 instances etc; Devs can focus on their code.

- **Lambda **
 - "A Clould guru" runs on Lambda.
 - Code that you upload to the cloud. You dont have to worry about underlying physical or Virtual machines. Literally there's nothing to manage there's no operating systems or anything. All you worry about is your code.
 - So give you an example you might have a you know main web site where people upload images and then want to overlay text on top of it.
What you do is you could basically create a lambda function that puts the text over the top and it's triggered as soon as somebody uploads an image to your Web site.
It then senses that this image has been uploaded and then based on the inputs that they give you would try to you know takes over that image and outputted that's a good example of lamda we will use Lamda

- **Lightsail**
 - Amazon's VPService or virtual private service service this is basically designed for people who just don't really want to understand anything about AWOS and the underlying infrastructure.They don't want to know about the PCs or security groups or anything like that. So essentially this just provision you with the server it'll give you a fixed IP address that you can log into the server from and will give you either ODP access for Windows or S-sh access for Linux. And then it comes with the really cool management console you can go in and manage that server using management console.
 - Watered down version of EC2

- **Batch**
 - Used for batch computing in cloud


#### Storage

- **S3**
  - Simple Storage Service (S3)
  - Object based storage
  - It has buckets where you upload your files
  
- **EFS**
 - Elastic Flie Storage
 - Attached to network

- **Glacier**
  - Data Archival Service
  
- **Snowball**
 - Snowball is a way to bring in large amount data (terabytes) into AWS data center
 - Rather than transmitting it over the broadband line or Wi-Fi or whatever if you're bringing in terabytes, sometimes it is easier just to write it physically to a disc they then will send that into the AWS data center and then import it manually.
 
- ** Storage Gateways**
 - Storage Gateway are essentially virtual appliances that you install in your datacenter or in your head office and then replicate information back to a S3.
 
#### Database

 - **RDS**
Relational Database Service
Supports most of the major databases including MySQL, Postgres, Oracle.
Basically any relational database will sit inside RDS.

 - **DynamoDB**
 Non-Relational Database Service provided by Amazon

 - **Elasticache**
 Way of caching commonly queried things rather than DB service pulling it from Database

 - **RedShift**
 Redshift is for data warehousing or business intelligence.
 
#### Migration

 - **AWS Migration Hub**
 Tracking service that allows you to track your applications as you migrate them
to AWS and integrates with other services within the migration framework.

 - **Application Discovery Service**
 Way of tracking dependencies for your applications

 - ** Database Migration Service**
Way to migrate your databases from on premise into AWS.

- **Server MIgration Service**
Enables you to migrate physical or virtual servers into AWS

#### Network & Content Delivery

 - **VPC**
 Amazon Virtual Private Cloud can be thought of as Virtual Data Center
 
 - **CloudFront**
 Amazons Content Delivery Network (CDN)
 But essentially what cloud front does is if you think about your media assets like your video files or your image files and if you've got users in Australia and your files are stored in say London what cloud front can do is actually store it closer to your users
in Australia so that they don't have to go and access the information directly from London they can actually access it from an edge location nearest use instead.

- **Route53**
Amazon's DNService

- **API Gateway**
API Gateway is essentially a way of creating your own API for your other services to talk to.

- **Direct Connect**
Direct Connect as a way of running a dedicated line from your corporate head office or datacenter directly into Amazon and it will directly connect into your PC.

#### Developer Tools

 - **CodeStart** is a way of getting a group of developers working together. It's a way of project managing your code. You basically set up your code and you have a continuous delivery toolchain and you can release your code within minutes so it's a way of collaborating with other developers who are working on a particular project.
 - **CodeCommit** is a source control service. So you basically store your own private get repositories within code commit search. Just think of it as a place to store your code.
 - **CodeBuild** Basically once you've got your code ready build it compile that code for you or run tests against it and then it will basically produce software packages that are ready to deploy.
 - **CodeDeploy** automates application deployments to your instances but it can also do it to on premise instances as well as to your lambda functions.
 - **CodePipeline** This is basically a continuous delivery service and use that sort of model and visualize and automate steps required to release your software.
 - **X-Ray** is used to debug and to analyze your service applications. It has request tracings you can actually go in and find the root causes of issues and performance bottlenecks.
 - **Cloud9** IDE environment. So this basically is a place where you can develop your code inside the AWS console. You don't even need to do it on your desktop any more you can do it inside a web browser.

####Management Tools
 - **CloudWatch**  Cloud watch cloud watch is a monitoring service.
 - **CloudFormation** Way of scripting infratructure. Turns infrastructure into code
 - **CloudTrail** Used to log changes to your AWS environment (Instance/S3/User/.. creation calls the API and CloudTrail logs that so called trail). Stores record for a week.
 - **AWS Config** Basically monitors the configuration of your entire AWS environment and it has like point in time snapshot. You can go back in time and visualize your AWS environment.
 - **OpsWork** Similar to BeanStalk but more roboust. Uses chef, puppet. It's a way of automating your environments.
 - **ServiceCatalog** Way of managing catalogs of I.T. services that are approved for use. These can be anything from Virtual Machine images individual operating systems software databases all the way through to like complete multi-tier architecture. So it's just basically a catalog service. This is typically used by big organizations for basically governance and compliance requirements service
 - **Systems Manager** Interface for managing your AWS resources. Typically used for EC2, Patch maintainance. For example, if you want to roll out a whole bunch of security patches across thousands of easy to instances it's easier to use systems manager.
 - **Trusted Advisor**Trusted advisor will give you advice across multiple different disciplines will give you advice around security.
 Tells you if you've left your ports open that could be a risk.
 It also tells you if you're not using your AWS services as much as you can or as much as you think.
 Can basically tell you how to save money.
 - **Managed Service** If you don't want to have to worry about your EC2 instances or auto scaling (ElasticCache, RDS, AmazonMQ)

**CloudFormation, CloudTrail, TrustedServices are important for SAA exam**

####Media Services (NEW)
  - **Elastic Transcoder** Takes a Video recorded on Mac and resizes it to make sure it looks good on Android
  - **MediaConvert**
  - **MediaLive**
  - **MediaPackage**
  - **MediaStore**
  - **MediaTailor**
  
####MachineLearning
 - **SageMaker** Makes it really easy for developers to use deep learning when  coding for the environments
 - **AmazonComprehend** Does sentiment analysis around data so tell you whether or not people saying good things or bad things about your products
 - **DeepLens** It can actually the camera that you can buy. It itself can figure out what it is it's looking at and it's not connecting back to an AWS backend. It's actually doing this on the camera itself. So, you can create an app that would detect somebody coming to your front door. And whether or not you recognize that person or not and that the door should open for them or not.
 - **Lex** This is what powers Alexa and it is a way of communicating with customers
 - **MachineLearning**Difference between machine learning and deep learning is that deep learning is around neural networks whereas machine learning is sort of entry level. Deep learning is more intelligent the machine learning. 
 With machine learning you throw a data set up into the AWS cloud. It will then analyze that data set and you basically give it some results and then it would determine whether or not based off that data set any new data, whether or not it will basically predict an outcome. Amazon themselves use it to recommend products from Amazon.com page uses MachineLearning
- **Poly** basically takes text and converts it into speech. And you can choose different languages, regions, accent
- **Rekognition** Essentially you upload a file and it will tell you what's in that file so it might be a picture and there is a dog on the beach playing you know with a bowl recognition will tell you dog Bache bowl and give you percentages as to you know accuracy. Likewise with video if you've got a guy walking up to a car and he opens a door and the dog gets in the back and they're at the beach. You can put that video up into recognition recognition tell you exactly what it recognizes within that video.
- **Amazon translate** Translates english into other languages
- **Amazon transcribe** Automatic speech recoginition. Converts speech to text

####Analytics
- **Athena** Allows you to run SQL queries for things in your S3 bucket. Lets say if you have different Excel or CSV spreadsheets in your S-3 bucket and you want to find out names of all your employees. You can actually design a SQL query which would go through and look at those objects in your bucket and then return results. It's completely serverless. There's no infrastructure to manage.
- **EMR** Elastic Map Reduce used for processing large amount of data (for big data solutions)
- **CloudSearch** Search service for AWS
- **ElasticSearch** Search service for AWS
- **Kinesis** Way to ingest large amount of data into AWS (Social media feed, tweets...)
- **Kinesis Video Stream** 
- **QuickSight** Amazon's business intellegence (BI) service
- **Data Pipleline** Used to move your data between AWS services
- **Glue** Used for ETL (Extract Transform Load). Migrate large amount of unstructured data - Extract>Transform>Load

####Security, Identity & Compliance
- **IAM** (Identity Access Management). Create Users, Groups, set Permissions
- **Cognito** Way of doing device authentication. Here you can authenticate through your app on mobile device using FB, Gmail and then using Cognito service which would give you temporary access to AWS resources (E.g. Dynamo DB) for mobile devices
- **GuardDuty** Monitors for melicious activity on your AWS account
- **Inspector** Agent that you install on your EC 2instance and youc an run test against it (E.g. Does your EC2 instance have security vulnaribilities). Youc an scheduke it to run weekly, monthly...
- **Macie** Scans your S3 bucket and looks for things that contains Personal Identifiable Information (PII) Example: Credit Card info, NAme, Addresses, SSN,
- **Certificate Manager** Way of managing SSL cert. You get SSL certs for free if you are registering your service using Route53
- **CloudHSM** (Hardware Security Module - Dedicated bits of hardware) Stores encrytption keys (public key, private key) also used to store other encryption keys
- **Directory Services** Way of integrating Microsoft Directory service with AWS services
- **WAF** (Web Application Firewall) Layer7 (application layer) firewall
- **Shield** DDoS mitigation. Included with CloudFront, LB, Route53. Amazon would refun in case of DDoS attack.
- **Artifact** This is great for audit and compliance. Essentially it's a portal for On-Demand access to download AWS compliant reports and you can also manage select agreements as well. So essentially it allows you to go in and you can download things like this Service Organisational Controls (SOC), Payment Card Industry (PCI) reports

####Mobile Services
- **Mobile Hub** is essentially a Management console. If you have mobile app, you can create mobile hub which would setup AWS services and then you can use AWS mobile SDK to connect your mobile app to AWS backend
- **PinPoint** Way of using targated push notifications to drive mobile engagement. So basically if you want to push out a notification to your mobile users that  maybe they are near a restaurant and that you run like a groupon style app you say hey there is 50 percent off this restaurant if you go there in the next two hours something like that you can use pinpoint to do that service.
- **App Sync** It basically automatically updates the data in web and mobile applications in real time and it also updates data for offline users as soon as they reconnect
- **Device Farm** Way of actually testing your apps on real life devices like Android devices iPhone devices etc
- **Mobile Analytics** Analytics service for mobile

####AR/VR
- **Sumerian** Sumerian lets you create AR, VR, 3D application quickly and easily. Essentially allows you to use common set of tools to create these environments

####Application Integration
- **Step Functions**
- **Amazon MQ**
- **SNS**
- **SQS**
- **SWF** (Simple Workflow Service)

####Customer Engagement
- **Connect**
- **Simple EMail Service** Great way of sending EMails, Highly scalable

####Business Productivity
- **Alexa for Business**
- **Chime** For video conferencing call.
- **Work Docs** Way to safely and securly store your work related documents (like DropBox)
- **WorkMail** Office365 version of Amazon

####Desktop and App Streaming
- **Workspaces**
- **AppStream 2.0**

####IoT
- ** iOT**
- **iOT Device Management**
- **Amazon FreeRTOS**
- **GreenGrass**

####Game Development
- **GameLift**

###OpsWorks vs CloudFormation
CF is like Terraform, its is fully coded automation of almost every service in AWS. So use if you have a very extensive service that requires full description of environments or templated stacks to cover low level services like auto scaling, spot instance, redshift, Cloudwatch alarmss and much more. The configuration time is much more extensive, and time consuming.

OW is basically AWS managed Chef. It has some great features for automation via the GUI, and you can configure most things. The great thing about Opsworks is that a good Devops can script the system, so someone that has little knowledge of infrastructure or cloud can still deploy apps super simply, and in very little time. However it is not as powerful as CF

So i think of it as CFT for large environments / many users and OpsWorks for smaller solutions or Application/Solution Stacks
