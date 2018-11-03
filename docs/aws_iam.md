# Identity Access Management (IAM)

- IAM allows you to manage users and their level of access to AWS console
- Centralized control of your AWS account
- Shared access to your AWOS account
- Granular permissions
- Identity Federation (including AD, FB, LinkedIn)
- Multi-factor authentication
- Provides temporary access for users or devices and services when necessary
- Allows you to set up and manage your own root password rotation policy
- Integrates with many different AWOS services
- Supports PCI DSS compliance

### Roles
- IAM Role had Policy (defining all the actions you can perform)
- Role > Policy > Actions
- If you planning to use roles CLI from aws instance you should use IAM role. If using from your local machine use credentials
  
### Pillers

- Users: End users
- Groups: Collection of users under one set of permissions
- Roles: You can create roles and assign them to AWS resources. For example you can create a role and assign it to EC2 instance to access S3 bucket without having to setup username/password
- Policy; Document that defines one or more permissions. Sits on top of above 3 pillars. You can create a policy and assign it to Users, Groups and/or Roles
