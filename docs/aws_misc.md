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
