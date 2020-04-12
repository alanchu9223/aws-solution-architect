# AMAZON SERVICES BASIC

- IAM
- ECS
- RDS
- VPC
- Cloudwatch
- SQS/SNS

### 26. IAM, Organisations, Cloud Trail

IAM - allows the organisation to manage many users.

- E.g. 1000 employees - different access levels
- E.g. 100,000 Users from outside the enterprise
- E.g. Secure access for EC2 instances to other services - Using Roles
- E.g. Grant users from third party authentication access to enterprise resoures (Google, Facebook)
- Note: PCI and DSS compliant - Cloudtrail activity log, that allows process auditing.

### 26.1 Shared Responsibility Model

- AWS is responsible for their cloud - You are responsible for what you do with the Cloud
- Infra services - EC2, EBS, VPC
- Container Service - RDS, EMR (Elastic Map Reduce), Elastic Beanstalk
- High LEvel Abstract Services - S3, Glacier, DynamoDB, Lambda, SQS/SES

### 26.2 User Groups and Roles

- We can create users and manage their access permissions to the accounts
- A role can be assumed by an AWS resource. Not just by a User.
- The User can be included into a group.
- Note: Users can be identified by the name, ARN, Unique ID
- Note: Users have credentials: password, Access Key ID, Secret Access Key
- Note: Never use access key to access resources.
- Example groups: Admins, Developers, Testers

ROLES

- Permissions that can be assumed by users of resources.
- The user can be an EC2 instance, not just a human user.
- The user can grant access to users in another Amazon Account
- Provide access to users identified using external third party (Identity Federation)
- Can also use identity federation with Microsoft Actice Directory (LDAP) or AWS Cognito

### 26.3 AWS Organisations

- An organisation can have multiple accounts.
  EXAMPLE: Ford Motor Corporation

1. Organisational Unit (OU) = Ford Motor Company
2. Service Control Policy (SCP) = whitelist/blacklist services within the OU.
3. Note: The SCP will override access the Service = Even if the User or Group Policy Allows it.
4. Features: Central mgt, control, automate and billing

### 26.4 IAM Policies

- A Policy defies what access a user, group or role has on the account
- By default users cannot access anything in the account
- Policy contains: Effect, Action, Resources
- Example: Effect="Allow", Action="dynamodb:\*", Resource="arn:aws:dynamodb:us-west-2:12345678:table/Books"
- Allow all actions on dynamodb for this resource.
- Not the only resource to restrict IAM users
- User/Group/Role based policy.
- Resource based policy - impose restriction on the resource.

### 26.5 Identity Federation

- To verify the identity of the user through third party verification
- IAM Role can be specified to allow access to federated (externally identified) users
- There is limit on IAM users (5000) There is no limit on federated users
-

### 26.6 AWS Cloud Trail

- Security feature - tracks all activities to/from AWS services

### IAM BEST PRACTICES

1. Lock away the Root User
2. Create individual IAM Users
3. Use Groups to Assign Permissions
4. Use AWS Defined Policies
5. Grant Least Privilege
6. Use Access Levels I.e. Read/Write/List Permissions
7. Configure a strong password policy
8. Use MFA for privileged users
9. Delegate using Roles
10. Rotate Credentials regularly
11. Remove unnecessary credentials?
12. Use Policy conditions?
13. Monitor activity on the account - with cloudtrail

## 36. ECS

### 1. ECR

- Secure Repository. Encrypted at rest and on transit. IAM Permissions/Role. Highly Available, Fault Tolerant. Designed for integration with ECS service.

### 2. ECS Tasks - EC2 or Serverless.

- How many containers. What Security for a container. What ports, what interconnects. What IAM roles. Application can span multiple tasks.

- "family" = the name of the task
- "containerDefinitions" = Name, image, memory, cpu
- "requiresCompatibilities" = FARGATE // Choose: EC2 or FARGATE
- "network mode" = awsvpc // none, bridge (docker's virtual network), awsvpc, host (bypass dockers virtual netowrk, map directly to EC2 instance)

### 4. Deployment on Serverless

- ECS TASK SCHEDULER - Places the tasks
- ECS CLUSTER - Logical group of resources that run the tasks
- ECS CONTAINER AGENT - Runs on each resource within the cluster. Responds to requests to Start, Stop, Etc.

- NOTE: Options = EC2, Use Elastic Beanstalk, Fargate

### 5. ECS Services -

- Run and maintain a number of ECS tasks
- Unhealthy tasks will be replaced - based on predefined health criteria
- Run service behind the load balancer

### EFS

- Security
- IAM Permissions
- EC2 Security Groups - Set inbound rules
- NACL - network access control list (control traffic)
- Linux root only permissions (CHOWN, CHMOD)

### S3

0. Basic Concepts

- Bucket: volume is unlimited, must have unique name
- Objects: 5TB maximum, multipart upload above 100mb
- Webstore, not file system: Will become eventually consistent
- SECURE BY DEFAULT - To allow access: IAM Roles, Bucket Policy, NACL (Access control list)

1. Storage Classes

- Standard (99.99% Availability, 11 9's of durability, SSL encryption, Lifecycle Mgt)
- Standard Infrequent Access (99.9% Availability)
- Standard One Zone Infrequent Access (99.5% Availability)
- INTELLIGENT TIERING: move objects between two access tiers (E.g. frequent and infrequent access)
- S3 Performance: Use Partitions/Prefixes to improve performance

2. Glacier

- Three levels of access: Expedited(3mins), Standard(3Hrs), Bulk (6hrs)

3. Life Cycle Mgt

- Delete when expire
- Transfer to glacier when expire
- S3 Version: Objects can have version control. Deleted version can be restored.

4. Cross SIte Replication

- S3 buckets can be replicated across regions
- Replicate across a pair of regions

## 43. RDS

### 1. RDS Backup, Fall Over, Replication

- User initiated snapshots of instance
- Automated DB Backups to S3
- Encrypted database. Encrypted Snapshots
- Enable multiple AZs
- RDS Read Replicas

### 2. Amazon Aurora

- Enterprise solution for RDS
- RDS is one instance, Aurora is a cluster of instances (single database endpoint)
- Automatic load balancing
- Vertical scaling: size the instances according to load
- Horizontal scaling: Adding up to 15 read replicas
- Aurora serverless: Spin up and shut down as needed. Pay only for the usage.

### 3. DynamoDB

- No SQL Database
- Tables = the file E.g. Persons
- Attributes = the fields E.g. firt name, lsat name, age
- Items = the records E.g. Ali, Ahmad, Ah Kow
- Partition key, Sort Key - for faster access
- Pricing model: Provisioned capacity: Number of RW per second. Or Auto scaling.
- DDB Provisioned Capacity
- DDB On Demand: Autoscaling based on Cloudwatch Alarm

### 4. Neptune

- Graph databases : Nodes, Edges, Properties
- Graph query languages: Gremlin Sparq

### 5. Redshift

- Petabytes of Data
- Postgres - OLA and BI
- Data warehouse: Complex queries against large datasets
- Redshift Cluster: Leader Node and Compute Nodes

### 6. Elasticache

- Fully managed in-memory data store
- Redis/Memcached Engine
- E.g. Frequently used data from DDB is stored in Elasticache for blazing fast response time. A Lambda is used to synchronize the Elasticache and the DDB.

### 7. DocumentDB

- It is not easy to migrate from MongoDB to DDB
- It is very easy to migrate MongoDB to DocumentDB
- Advantages:

1. No need to launch EC2 instances. Load balancing of EC2 instances
2. One primary, up to 15 replicas
3. 99.99 availability
4. Replication acress AZs
5. Continuous backup to S3 - Up to 35 days of point in time recovery

## 50. VPC

1. 2. 3. 4. 5. 6.

## 62. Cloudwatch

1. 2. 3. 4. 5. 6.

## 70. SQS/SNS

1. 2. 3. 4. 5. 6.
