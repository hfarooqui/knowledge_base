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
