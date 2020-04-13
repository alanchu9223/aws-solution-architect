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

### 8. DDB

1. Features

- Automatic scaling
- Query on attribte - secondary indices
- Cross region replication
- Schema-less
- Downloadable local version

NOTE: SQL vs No SQL

- SQL optimized for storage - NoSQL optimized for speed
- Normalized - Not normalized (heirachical)
- Rich Language - Simple Language
- Has Joins and Rules - N/A
- Vertical Scaling - Horizontal Scaling
- Online Analytical Processing (OLAP) -- SLOW/COMPLEX - Online transaction Processing (OLTP) -- FAST/SIMPLE

2. Tables, Attributes, Items, Primary Keys

- Primary Key: ISBN: Partition or Partition and Sort
- Secondary Indices: Title, Authors (Alternative Key)
- Local Secondary Index: Primary + Secondary: Same partition key as the Table (ISBN) E.g. FInd me a book given ISBN and Author name (Sort Key).
- Global Secondary Index: No Primary Index: Not same partition Key as the Table (ISBN) E.g. Find me a book given Author (Sort Key) and Title (Partition Key).
- Query vs Scan - Query searches the keys. Scan searches entire dataset

3. Indexes, Queries, Scans
4. DDB Performance
5. DDB Streams and Triggers
6. DDB Accellerator

## 50. VPC

- AWS Cloud = multiple regions.
- Region = Multiple AZs
- AZ - when one AZ goes down,the other AZ continues to service the application
  The VPC

1. Should span more than one AZ for maximum availability - Different subnets for each AZ
2. Internet Gateway - Access to public via Internet
3. Virtual Private Gateway - Access via VPN - There is a Customer Gateway on the Customer Premises
4. AWS Customer connect - A physical lease line connecting the customer to AWS.
5. INTERNET CONNECTIVITY:

- VPC Must have gateway
- EC2 Instance must have public IP
- The route table defines a route from IGW to EC2 instance
- Security Group - Security per instance (STATEFUL)
- NACL - Security per subnet (STATELESS)

### 1. Connecting into VPC

### 2. Make Web Server Accessible

### 3. Security Features of the VPC

- Security Group - Instance level, Allow only, Stateful, Most Permissive Rule Wins
- ACL - Subnet level, Allow an Deny, Stateless, Most Restrictive Rule Wins
- Flow Logs

## 53. AWS WAF, AWS SHIELD, AWS FIREWALL MANAGER

1. DDOS ATTACK - multiple sources, flood the server with requests. Infrastructure Layer. Application Layer.

- Reduce Surface Area
- Plan for Scale
- Know what is normal and what is abnormal
- Deploy WAF

2. AWS SHIELD - Cloudfront and Route53 - Active monitoring, AWS DDOS Response Team

- AWS Shield Advanced - EC2, Elastic Load Balancers

3. WAF - Web application firewall

- Rules for HTTP Headers, HTTP Body, IP Address
- AWS Managed Rules for common security threats
- API Gateway, Load Balancer, EC2
- AWS Security Automation - Lambda integrate with WAF with custom automation

4. FIREWALL MANAGER

- Many AWS rules - centrally managed

5. PENETRATION TESTING

- Inform AWS before penetration testing (2 business days)
- Apply as root user from the console
- Only allowed for selected services: EC2 RDS, Load Balancer, Cloudfront, Lambda, Lightsail, API Gateway.

## 62. Cloudwatch

1. 2. 3. 4. 5. 6.

## 70. SQS, SNS

### 70. SQS

1. What is SQS

- Buffer for processing server (smooth out the peak demand)
- 10 attributes. Plus the message body.
- Up to 256kB
- Link to cloudwatch and autoscaling the EC2 instances

2. Queue Types

- Standard(Faster, almost all are in correct order) and FIFO (Slower)

3. Message Lifecycle

- Message received by the Queue
- Message received by processing server (Visibility Timer Begins)
- Message processed by the processing server (Visibility Timer Ends)

4. Dead Letter Queues

- Messages that cannot be processed end up the Dead Letter Queue
- These messages can be analysed and disposed later

5. Delay Queues, Message Timers

- Queue that cause messages to be delayed
- Messages themselves can have an invisibility timer

6. Long Polling

- Short Poll - instant response indicate if message is present
- Long Poll - response only happens when a message is found

### 71. SNS

- Allows sending of messages to clients (up to 256kB)
- Publish-Subscribe model. Topic name is Unique.
- Subscriber access controlled by policies.

1. Transport Protocols

- HTTP, HTTPS, Email, SQS, Lambda, SMS, MObile Push Service

2. Message format

- Email Message = message body
- HTTP/HTTPS/SQS: Message ID, Timestamp, Topic, Type Unsubscribe URL, Message, Subject, Signature, Signature version

3. Mobile push notification

- Request Credential from Mobile Platform
- Request Token from Mobile Platform
- Create SNS Platform Application Object
- Create SNS Platform Endpoint Object
- Publish SNS Message to Endpoint Object

4. ADM Setup (Amazon Device Messaging)

- Create Kindle Fire App - ADM Service Enabled
- Get CLient ID and CLient Secret
- Get API Key
- Get Registration ID (Use this with the Push Notification Service to send the message)

5. APNS Setup (Apple Push Notification Service)

- Create IOS App
- APNS SSL Certificate
- App Provate Key
- Verify the certificate and App Private Key
- Obtain Device Token

6. Mobile Push Notification (Google Cloud Messaging)

- Create Google API Project Enable the GCM
- Get Server Key
- Get Registration ID and GCM

### 72. SWF - Simple workflow service

- What is? Used for implementing distributed complex business processes. Long running execution.
- Features: Tasks, Routing and Queueing of Tasks, TImeout and Execution, Child workflows, User inputs, Execution Outputs
- Components: Workflow, Domain, Tasks, Actors
- Actors (Starter, Decider, Worker)
- Tasks (Parallel/Serial, CUstom code, A service call, User input)
- Implementations

## 73. Big Data

- DATABASE: Redshift, DDB, S3, RDS.
- ANALYSIS: EMR (Hadoop as a service), Elastic Search, Quick Sight BI, MAchine Learning, Lambda
- REAL TIME DATA: Kinesis Streams
- THIRD PARTY

1. Redshift

- Petabyte scale Warehouse - Base on PostgreSQL
- Standardd BI Tools
- Data replication between nodes in the cluster
- Continuously backed up to S3 with snapshots
- Quick snpshot recovery
- NOTE: BIG DATA MIGRATION
- 100TB of data can take 10 days to a year to transfer depending on network connection
- Use AWS Snowball if more than 5TB and 100 Mbps connection
- Examples: Create snapshot of RDS database and send
- Example: AWS Database Migration Service (database to database)
- Example: AWS Data Pipeline (data source to database)
- Example: Snowball to S3, use AWS datapipeline to import to the database from S3

2. EMR

- Hadoop/Apache Spark - EC2 Clusters automatically deleted upon task completion
- File system: Hadoop Distributed FS, EMR FS (to access S3), Local File System (EC2)

3. Elastic Search

- Real time search and analytics engine. (Enterprise Search Engine) Used by FB, Github, Stack Exchange, Quora.
- Analyze data from: S3, Kinesis, DDB, Cloudwatch logs, Cloudtrail
- NOT SUITABLE FOR: OLTP - online transaction processing, Petabyte storage

4. Quick Sight

- BI Reporting Service
- 10% the cost of traditional BI software (Tableau)
- Super fast parallel in memory calculation engine (SPICE)

5. Machine Learning

- Predictive Analytics with visualization and wizards
- Data Sources: S3, Redshift, RDS (MySQL)
- Flag suspicious transactions, forecast demand, personalize application content, predic user activity, analyze social media
- NOT SUITABLE FOR: Very large data sets, Unsupported learning tasks
- For large datasets, use EMR to run SPARK and MLib

6. Kinesis

- Realtime data - streaming (Data comes from HTTP PUT, SDK, C++/Java Libraries)
- Kinesis Client Library: process data already in a stream
- Kinesis Firehose: used to capture, transform, load streaming data into Amazon Kinesis Analytics, S3, Redshift, Elastic Search Service.
- SUITABLE FOR: Real time analytics, Log and data feedintake processing, Real time metrics.
- NOT SUTABLE FOR: small scale consistent throughput, long term data storage and analytics.

### 74. API Gateway

1. Example Scenario 1:

- Webserver - Static Website - S3, Cloudfront
- Web API - Dynamic COntent - API Gateway, Cloudfront

### 76. ElastiCache

- What is?: Ultrafast, key-value pairs, Redis store option, Reduces load o
- Memcache: One object/string up to 1MB, Max vloume 4.7 TB, Add more nodes to scale, No persistence.
- Redis: Advanced caching, strings/hash/list/sets/bitmaps/geospatial indexes/hyperlogs, PERSISTENCE, Large command set, Notifications with PUB/SUB channel. NOTE: Advantage over memcache: data tyes, auto sorted data, Pub/Sub capability, High availabililty, Persistence.
- Updating Cache: Databse trigger-> Lambda Function -> Elastic Cache
- Caching Strategies: Lazy loading(load on a cache miss), Write through (load on a write), Adding TTL
- NOTE: Data must have TTL so that they will expire when not used EX - seconds. PS - milliseconds

### 77. Kinesis (Not available on free tier)

- Live and continuous data processing and analysis
- Managed service - Elasticity/durability (3AZs)
- Process and Safety Alarms
- Multiple applications can consume the stream
- Data retention default = 1 day. Can extend to 7 days.

1. Data Streams -

- Producer, Consumer, Stream (contains multiple Shards), Record (has sequence number), partition key (one for each shard), data blob,
- Management COnsole: Create Stream Option
  WRITING KINESES STREAMS
- One Shard: Writing to stream: 1000 PUT Records/second. 1MB/sec
- Shard has: Sequence number, Shard Iterator (position to start reading the record)
  READING KINESES STREAMS
- 5 transactions per second. 2MB/sec data reads
- Shard Iterator Type: At Seq Number, After Seq NUmber, At Time Stamp, Trim Horizon, At timestamp
- Get Record: Return value contains: Millis behind latest, Next Shard Ierator, Array of Records.
  HOW TO READ FROM A SHARD
- Get Shard Iterator
- Get Records (milliseconds behind latest, next shard iterator, records)
  TOOLS:
- KCL - Kinesis Client Libraries
  SCALING KINESIS - Update Shard Count Command
- Resharding (change number of shards)
- Shard Split (increase)
- Shard Merge (decrease)
- SHARD STATE: OPEN (before reshard), CLOSE (after reshard), EXPIRED.
  HOW MANY SHARDS DO WE NEED?
- BWwrite = how much data per second WRITTEN
- Number of Consumers of the stream
- BWread = WRITE BW x Number of Consumers
- MAXshard = 1MB write or 2MB read
- NUMshards = MAX (BWwrite/1MB or BWread/2MB)

2. Video Streams -

- FRAGMENT - Sequence of frames
- PutMedia - writes a Fragment to a stream
- GetMedia - reads Fragment from a stream at StartSelector position
- TOOLS: Producer Client, Producer Library, Parser Library, Kinesis Video Media, C++ Producer Library

3. Data Firehose -

- Deliver data from Kinesis to S3, Redshift, ElasticSearch, Splunk
- Buffers before delivery
- Server side encryption

4. Data Analytics -

- SQL to analyse steaming data
- Supports Firehose, Lambda and Kinesis as DESTINATIONS

### 78. Serverless

1. Serverless
2. Compute
3. Storage
4. Data
5. API
6. Integration/Orchestration
7. Analytics
