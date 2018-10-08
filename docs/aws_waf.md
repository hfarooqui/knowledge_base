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
