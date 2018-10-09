# Well Architected AWS Framework

## Basics

#### Benefits of Cloud
- Almost zero upfront infrastructure investment
- Just-in-time infrastructure
- Efficient resource utilization
- Usage-based costing
- Reduced time to market
- Automation - "Infrastructure as Code"
- Auto scailing
- Proactive scaling
- Efficient development cycle
- Improved testability
- DR and business continuity
- "Overflow" the traffic to the cloud
![Cost_vs_Automation](https://s3.amazonaws.com/hfcontents/kbimages/Cost_vs_Automation.png "Cost_vs_Automation")


#### Design for failure
- Be a pessimist. Assume things will fail. In other words always design, implement and deploy for automated recovery from failure
- In perticular assume
   - Hardware failure
   - Application failure
   - Outages
   - More than expected number of requests per second some day
 - Netflix has Choes Monkey, choes snail

#### Decouple your application
- Loose coupling isolates verious layers and components of your application so that each component intrects asynchronously with others and treates them as blackbox such that if one component fails other whould continue to work (SQS)
- For example you would isolate Web server, App server and DB server

#### Elasticity
Can be implemented in following ways
- Proactive Cyclic scaling: Periodic scaling that occurs at fixed interval
- Proactive Event-based scaling: Scaling only when you are expecting big surge of traffic (new product launch, marketing campaign, black friday sale)
- Auto scaling: By using monitoring service, you system can send triggers to take appropriate action so that it scales up or down based on the matrics

#### Secure your application
![Securing_Applications](https://s3.amazonaws.com/hfcontents/kbimages/Securing_Applications.png "Securing_Applications")

## Five pillers of well Architected framework
- Security
- Reliability
- Cost Optimization
- Performance Efficiency
- Operational Excellance

#### Design Principles
- Stop guessing your capicity needs (Don't predict, scale automatically)
- Test systems at Production scale (CloudFormation: Create > Test> Destroy)
- Automate to make your architectural experimentation easier
- Allow for evolutionary architectures (business decisions change frequently)
- Data-driven architecture (Logging helps you make fact based decision)
- Improve through game days (Schedule game days to simulate production environment. E.g. schedule game day for black friday. Here you pay only for used resources for duration they are used)

## Security
### Design Principles
 - Apply security at all layers within your infrastructure (Subnets, NACL, SG's, Anti-Virus on Windows OS)
 - Enable tracibility (If someone hacks your environment, you should know how he did that)
 - Automate responses to security events (If someone is trying to hack into the system, multiple SSH attempts, you should send SNS)
- Automate security best practice (E.g. Using hardened image/ami)
- Focus on securing the system (Share responsibility mode. You as a customer what are your responsibilities vs AWS)
![Infra_Resp](https://s3.amazonaws.com/hfcontents/kbimages/Infrastructure_Responsibility.png "Infra_Resp")

### Definition
Security in the cloud consists of 4 areas:
**Data protection**
   - Organise and classify your data into segments: Publicly available, Available only to members of your organisation, available to certain members of your organisation, available only to board members
   - You should implement a least privilage access system so that people are only able to access what they need
   - Encrypt everything where possible, be it at rest or in-transit
   - AWS customer maintain full control over their data
   - AWS makes it easier to encrypt your data and manage keys including regular key rotation which can be easily automated by AWS or maintained by customer
   - Detailed logging is available (Audit Trail) that contains important contents such as file access and changes
   - AWS has desiged storage system for exceptional resiliency
   - Versioning: Protects against accidental overwrites, deletes and similar harm
   - AWS never initiates movement of date between regions (Patriot Act cannot leave US)
	How are you protecting data at rest and in-transit

**Privilege Protection**
   - Ensures that only Authenticated and Authorized personals are able to access your resources and only in a manner that is intended. It includes:
       - ACL
	   - Role based access control (S3, DynamoDB roles)
	   Password Management (password rotation policy)
 - How are you protecting access to and use of the AWS root credentials
 - How are you defining roles and responsibilities of system users to control human access to the AWS Management Console and API's
 - How are you limiting automated access (scripts, or third party tools or services) to AWS resources

 **Infrastructure Protection**
  - Protection outside of cloud typically involves RFID controls, security, lockable cabinets, CCTV. Within AWS they handle this, so really infrastructure protection really exists at a VPC level
  - How are you enforcing network and host-level boundry protection? (NACL, SG, Public/Private Subnets, Bastion hosts)
  - How are you enforcing AWS service level protection (How are users accessing AWS console, Do you have groups setup, Multi-factor authentication, Strong password preotection, password rotation)
  - How are you protecting integrity of the operating system on your amazon EC2 instances (Is your OS hardened do you have AV installed on Windows OS instances)

 **Detecive Controls**
- Detect or identify security breach. AWS services to achieve this includes:
      - CloudTrail (Logs every change to your AWS environment)
      - CloudWatch (based on sudden spike in CPU - bitcoin mining)
      - AWS config
      - S3 (backup/restore)
      - Glacier
 - How are you capturing and analyzing AWS logs? Are you using any log management service

### Key AWS Services
- Data Protection: You can encrypt your data both in-transit and at rest using ELB, EBS, S3 and RDS
- Privilege Management: IAM, Multi-factor Authentication (MFA)
- Infrastructue Protection: VPC
- Detective Controls: CloudTrail, CloudWatch, AWS Config, Glacier

## Reliability
- Ability to acquire computing resource
- Ability to recover from service or infrastructure outage

### Design Principles
- Test recovery procedures (Choes Monkey, ChoesSnail)
- Automatically recover from failure
- Scale horizontally
- Stop guessing capacity as you might end up sitting on expensive resources

### Definition
**Foundations**
- How are you managing AWS service limits for your account?
- How are you planning your network topology on AWS?
- Do you have an escalation path to deal with technical issues?

**Change Management** (Auto scaling)
- How does your system adapt to changes in demand?
- How are you monitoring system resources?
- How are you executing change management?

**Failure Management** (CloudWatch, CloudTrail)
- How are you backingup your data?
- How does your system withstand component failure?
- How are you planning for recovery?
- Ability to identify cause and respond to failure

### Key AWS Services
- Foundations: VPC, IAM
- Change Management: CloudTrail
- Failure Management: CloudFormation, RDS failover

## Performance Efficiency
- Focuses on how to use computing resources effectively to meet your requirement and how to maintain that efficiency as demand changes and technology evolves

### Design Principles
- Democratize advanced technologies (use technology as servic - NoSQLDB, ML, Transcoding)
- Go global in minutes
- Use server-less architectures
- Experiment more often

### Definition
Performance efficiency in the cloud consists of 4 areas
- Compute
- Storage
- Database
- Space-time trade-off

#### Compute
- How do you select appropriate instance type for your system?
- How do you ensure that you continue to have the most appropriate instance type as new instance type features are introduced?
- How do you monitor your instances post launch to ensure they are performing as expected?
- How do you ensure quantity of your instances matches demand?

#### Storage
- Factors affecting storage
    - Access methods - Block, File or Object
    - Patterns of access - Random or Sequential
    - Throughput required
    - Frequency of access - Online, Offline or Archival
    - Frequency of update - Worm, Dynamic
    - Availability Constraints
    - Durability Constraints

- How to select the appropriate storage solution for your system?
- How do you ensure that you continue to have the most appropriate storage solution as new storage solutions and features are launched?
- How do you monitor your storage solutions to ensure it is performing as expected?
- And how do you ensure that the capacity and throughput of your storage solutions matches demand?

#### Database
- DB solution depends on number of factors
    -  Do you need database consistancy
	- Do you need HA
	- Do you need No-SQL
	- Do you need DR
- How do you select appropriate DB solution for your system
- How do you ensure that you continue to have the most appropriate DB solution as new DB solutions and features are launched?
- How do you monitor your DB to ensure it is performing as expected?
- And how do you ensure that the capacity and throughput of your DB matches demand?

#### Space-time trade-off
- How do you select the appropriate proximity and caching solutions for your system?
- How do you ensure that you continue to have the most appropriate proximity and caching solutions as new solutions are launched?
- How do you monitor your proximity and caching solutions to ensure performance is expected?
- How do you ensure that proximity and caching solutions you have matches demand?

### Key AWS Services
- Compute: Autoscaling
- Storage: S3, EBS, Glacier
- Database: RDS, DynamoDB, Redshift
- Space-time tread-off: CloudFront, ElasticCache, RDS Read replica, Direct Connect

## Cost Optimization
- Reduce cost, Serverless technology

### Design Principle
- Transparently attribute expenditure (encourage BU to lower cost)
- Use managed services to reduce cost of ownership
- Trade capital expense for operational expense
- Benefit from economies of scale (Amazon buying in bulk and producing thir own as well)
- Stop spending money on data center operations

### Definition
- Cost optimization in the cloud consists of 4 areas:
    - Matched Supply and Demand
	- Cost-effective resources
	- Expenditure awareness
	- Optimizing over time

#### Matched Supply and Demand
- Autoscaling: Scale with demand
- Lambda: Execute only when request comes in
- CloudWatch: Keep track of what your demand is
- How do you make sure your capacity matches but does not exceeds what you need?
- How are you optimizing your usage of AWS services?

#### Cost-effective resources
- Use correct instance type
- Have you selected the appropriate resource types to meet your cost targets?
- Have you selected the appropriate pricing model to meet your cost targets?
- Are there any managed service which would improve your ROI?

#### Expenditure awareness
- You no longer have to go out and get the quotes on physical hardware, choose a supplier, have those resources delivered and installed
- You can provision resources in seconds.
- You can track spending of each account (Dev, QE, SRE), billing alerts
- What access controls and procedures you have in place to govern AWS costs?
- How are you monitoring usage and spendings?
- How do you decomission resources that you no longer need or stop resources that are temporarily not needed
- How do you consider data transfer charges when designing your architecture?

#### Optimizing over time
- You should keep track of changes made to AWS and constantly re-evaluate your existing architecture (AWS blog, Trusted advisor)
- How do you manage and consider adoption of new services?

### Key AWS Services
- Matched Supply and Demand: Autoscaling
- Cost-effective resources: EC2 (reserved instances), AWS trusted advisor
- Expenditure awareness: CloudWatch, SNS
- Optimizing over time: AWS blog, AWS trusted advisor
