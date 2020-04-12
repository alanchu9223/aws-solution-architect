# AMAZON SERVICES ADVANCED - ABSTRACT SERVICES and ARCHITECTURE CONCEPTS

### Terminology

1. [provisioning vs deployment](https://codefol.io/posts/deployment-versus-provisioning-versus-orchestration/)

### 13. Elastic Beanstalk

1. Deployment architecture

- High availability/Fault tolerant: capacity provisioning, load balancing, app health monitoring
- Deployment options: All at once, Rolling (batch at a time), Immutable (Temporary double up capacity during deploy), Blue/Green (One is Development Environment, One Production Environment)
- Managed using AWS CLI/Console

2. Workflow

- Create application
- Launch Environment
- Manage Environment

3. Concept: High Available Architecture

- Use more than one AZ in the VPC - when one AZ goes down, the other continues to serve
- Autoscaling group will adjust number of EC2 instances according to demand (Elastic Architecture)
- The "Elastic" Load Balancer distributes requests between EC2 instances
- "Health Checks" are performed by the Autoscaling group: to ensure the number of EC2 instances match the requirements. This means _Fault Tolerance_ in the architecture

4. Example:

- Create ELB project: backspace academy
- Configure presets: "High Availability" - this will enable load balancer, auto scaling, AZs, multiple EC2 instances

### 15. Amazon Free Tier

- https://aws.amazon.com/free

### 16. Present a Business Case for Migrating to AWS

1. Advantages of Cloud Computing

- Trade capital expense for variable expense
- Benefit from massive economy of scale
- Stop guessing capacity
- Increase speed and agility
- Stop spending money on running/maintaiing data centers
- Go global in minutes

2. AWS Simple Calculator

- Works out the monthly bill given the resources

3. AWS TCO Calculator: Specify the existing cost components

- Server Cost/Storage Cost
- Network Cost
- IT Labour Cost
- Hardware Cost
- Software Cost

4. Amazon Inspector

- A major cost is Security Auting and Assessment
- Amazon Inspector Automates the Auditing and Assessment \$0.30 per agent-assessment per month

5. Compliance

- AWS is precertified: ISO9001, PCI-DSS Standard.

6. AWS Support

- Free - no tech support
- Developer - business hrs tech support. 12 hr response time (crit failures)
- Business - 24/7 tech support. 1 hr response time (crit failures)
- Enterprise - 24/7 tech support. 15 min response (crit failures)

### 17. AWS Architecture and Compliance

1. https://aws.amazon.com/architecture

- Reference Architectures/Templates
- Compliance Services
- AWS Quick Starts: AWS CloudFormation templates that automate the deployment and a guide that discusses the architecture and provides step-by-step deployment instructions.
- Take a look at : Standardized Architecture for PCI DSS on the AWS Cloud

2. AWS Well Architected Framework

- Operational Excellence
- Security
- Reliability
- Performance Efficiency
- Cost Optimization

3. AWS Well Architected Tool

- Answer a bunch of questions - use answers to self assess that all architectural considerations have been satisfied
- Print out a report and present this to this to the client!!!

- LOOK AT WELL ARCHITECTED LENS FOR IOT - PRESENT A CCL BASED ON THIS
- Focus on IOT LENS WHITEPAPER/SERVERLESS
- https://aws.amazon.com/architecture/well-architected/?sc_ichannel=ha&sc_icampaign=acq_awsblogsb&sc_icontent=architecture-resources
- Do one hour training on Architecture
  https://www.aws.training/Details/Curriculum?id=42037

### 19. Build your first Production Grade AWS Website

1. Register AWS Account
2. Get a Website template from freehtml5.co
3. Download course notes in Lecture#20

### 20. Get started

www.gastronaut.com.au
C:\work3\infra-auto\resto.zip

### 29. Trusted Advisor Service

- Audit the architecture agains the five pillars

1. Cost
2. Performance
3. Security
4. Fault Tolerance
5. Service Limits
