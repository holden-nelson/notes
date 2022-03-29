# AWS Cert Notes

Benefits of cloud computing/how cloud saves you money

* Go global in minutes
* No maintaining and running data centers
* Benefit from economies of scale
* Increased speed and agility
* Stop guessing capacity
* Trade capital expense for variable expense

Basic Cloud Terminology

* High availability - resilience
* Elasticity - Grow and shrink resources on demand
* Agility - Tons of services available at the click of a button
* Durability - Long term data protection

Cloud computing models

1. Infrastructure as a service
   1. Fundamental building blocks that can be rented
   2. e.g. web hosting
2. Software as a service
   1. Complete application 
   2. e.g. email provider
3. Platform as a service
   1. Develop software using web-based tools without worrying about underlying infrastructure
   2. e.g. Shopify

Cloud Deployment Models

1. Private cloud
   1. "On prem"
   2. Exists in internal data center
   3. No sharing resources, but no advantages above
2. Public cloud
   1. e.g. AWS
   2. No responsibility for physical hardware
   3. All advantages above
3. Hybrid cloud
   1. Mix of the two
   2. Some private, more secure
   3. AWS provides some tools for this

Global Infrastructure

1. Regions
   1. Regions are physical locations by which infrastructure is grouped
   2. Fully independent and isolated
   3. Resource and Server specific
2. Availability Zones
   1. One or more physically separated data centers
   2. Physically separated, connected low latency, fault tolerant, highly available
3. Edge locations
   1. Basically CDNs - near population centers, explicitly there to cache content
   2. Reduce latency

AWS Account

1. Management Console - browser app for managing apps running in your account
2. Root user
   1. Associated with email, created on signup
   2. should only be used once
   3. should be MFA
3.  CLI
   1. Some new features are CLI only,  or available via the CLI first.
   2. Other programmatic access - through application code through SDK

Technology

1. Elastic Compute Cloud (EC2)
   1. Manage and rent virtual servers in the cloud
   2. An EC2 instance is a virtual server running on a physical server in a data center in an availability zone in a region
   3. EC2 instances are not "serverless"
   4. Access with management console, SSH, EC2 instance connect, AWS systems manager
   5. Pricing
      1. On-demand 
         1. fixed price, billed down to the second.
         2.  No contract, pay for what you use. 
         3. Apps with unpredictable workloads that can't be interrupted
         4. Apps under development
         5. Workload will not run longer than a year
         6. You can reserve capacity
      2. Spot
         1. Cheapest option
         2. Take advantage of unused EC2 capacity
         3. Request only fulfilled if capacity is available.
         4. No concern for start or stop time
         5. Workloads can be interrupted
         6. Only feasible at low compute prices
         7. Pay the price in effect at beginning of hour
      3. Reserved Instances
         1. Allows you to commit to a specific instance type in a particular region for 1 or 3 years
         2. Steady state usage, can commit to 1 or 3 years
         3. Pay upfront to receive discount on on-demand pricing
         4. App requires capacity reservation
         5. Provides convertible types? At 54% discount
      4. Dedicated Hosts
         1. Physical server fully dedicated to running instances
         2. Use when you want to bring your own server-bound software license
         3. Regulatory or compliance requirements around tenancy model
      5. Savings Plans
         1. Allows commitment to compute usage (measured per hour) for 1 to 3 years
         2. Lowers bill, gives flexibility to change compute services, instancy types, operating systems, or regions.
   6. Features
      1. Elastic load balancing
         1. automatically distributes incoming traffic across multiple ec2 instances
      2. Autoscaling
         1. Adds or replaces instances on demand
         2. Upgrades horizontally (more servers), not vertically (better equipment)
2. Lambda
   1. Serverless compute service that lets you write code without managing servers
   2. Code is called "function", and just runs in response to an event
   3. Allows focusing on core business logic instead of managing servers
   4. Some use cases
      1. File processing --> User uploads file which goes to S3 bucket, which triggers the function, which processes and stores the data from the file into a database
      2. Email --> Some event triggers the lambda function which triggers SNS which sends an email
      3. Backend business logic --> An alexa skill that triggers a lambda that queries a database that sends the info back to Alexa
   5. Features
      1. Supports popular programming languages
      2. Author your code your way
      3. Lambda executes code in response to events
      4. Lambda functions have a 15-minute timeout
   6. Pricing
      1. Charged on duration and number of requests
      2. Duration is compute time
      3. Request is counted each time it starts execution. Includes tests.
      4. Always free - The free usage tier includes 1 million requests per month - **and duration?**
3. Additional Compute Services
   1. Fargate
      1. Serverless compute engine for containers
      2. Manage containers, like Docker
      3. Scales automatically
   2. Lightsail
      1. Quickly launch resources for small projects
      2. Deploy preconfigured apps, like Wordpress, with the click of a button
      3. Simple screens
      4. Incudes virtual machines, DNS management, SSD storage, Data transfer, DNS management, static IP
      5. Provides low predictable monthly fee, as low as $3.50 USD
   3. Outposts
      1. Run cloud services on prem
      2. Supports workloads that need to remain on prem due to latency or legal reasons
   4. Batch
      1. Process large workloads in smaller chunks
      2. Run hundreds of thousands of smaller batch processing jobs
      3. Dynamically provisions instances based on volume
4. S3 - Simple Storage Service
   1. Highly available and durable object storage service
   2. Basics
      1. Objects (or files) are stored in (buckets)
      2. Ostensibly unlimited storage
      3. Objects can be public or private
      4. Upload via the console, CLI, or programatically 
      5. Security at bucket or individual level using ACLs, bucket policies, access point policies
      6. Optional versioning created multiple versions of file in order to protect against accidental deletion and to use a previous version
      7. Use S3 access logs to track access to buckets and objects
      8. S3 is regional, but bucket name is globally unique
   3. Storage Classes
      1. Standard - general purpose storage for any data. Good for frequently accessed data.
      2. Intelligent Tiering - Automatically moves data to the most cost effective storage class. Good for data with unknown or changing access pattern.
      3. Standard Infrequent Access - Not accessed frequently, but rapid access required when accessed. Recommended for long-lived data, infrequently accessed data
      4. One Zone Infrequent Access - similar to IA but only one zone. DATA MAY BE LOST if something happens to that zone. Cheaper. Recommended for data that could be recreated, infrequently accessed, availability/durability not essential
      5. S3 Glacier - long term data storage and archival. Very low costs. Data retrieval takes forever.  Three retrieval options. Recommended for long term backups.
      6. S3 Glacier Deep Archive - Absolute cheapest. Longer access times. Recommended for long term data archival, regulatory compliance data
      7. S3 Outpost - on prem storage. Recommended for demanding performance needs.
   4. Use Cases
      1. Static Websites
      2. Data archives
      3. Analytics raw data
      4. Mobile applications
5. Additional Storage Options
   1. Elastic Block Store - like a flash drive that can be attached and detached to/from an instance. Data persists when instance not running. Tied to one AZ. Can only be tied to one instance. Recommended for quickly accessible data, database running on instance, long term data storage
   2. EC2 Instance Store - local storage that is physically attached to host and cannot be removed. Storage is temporary, data is lost on instance stoppage. Fast IO. Recommendded for temp storage needs or data that's replicated.
   3. Elastic File System - only supports Linux instances. More expensive than EBS. Accessible across different AZs in same region. Recommended for main directories of business-critical apps, lift-and-shift existing enterprise apps.
   4. Storage Gateway - hybrid storage service. Connects on-prem to cloud data. Recommended for moving backups to cloud, reducing costs for hybrids cloud storage, low latency access to data.
   5. AWS Backup - helps manage data backups across multiple AWS services. Integrates with other storage solution, allows backup plan creation.
6. Content Delivery Networks
   1. Ensures fast download times by positioning resources close to users
   2. Amazon Cloudfront
      1. Makes content available globally or restricts it based on location
      2. Speeds up delivery of static and dynamic web content
      3. Uses edge locations to cache content
      4. Prevents DDOS
      5. IP address blocking - geo-restriction prevents users in certain countries from accessing content
   3. Amazon Global Accelerator
      1. Improves latency and availability of single region apps
      2. Sends traffic through the AWS global network infrastructure
      3. 60% performance boost
      4. Automatically reroutes traffic to healthy endpoints
   4. Amazon S3 Transfer Acceleration
      1. Improves content uploads and downloads to S3 buckets
      2. Fast transfer of files over long distances
      3. Uses cloudfront's edge locations
      4. Customers around the world can quickly upload to one bucket
7. Networking Services: VPC and subcomponents
   1. Amazon Virtual Private Cloud
      1. Allows creation of secure private network in AWS cloud where we launch resources
      2. Launch resources inside VPC, isolates and protects them
      3. Spans AZs
      4. Subnets allow splitting the network inside a VPC
      5. Can peer VPCs to connect them
   2. Additional Network Services
      1. Amazon Route 53
         1. Highly available DNS service
         2. Domain name registration
         3. Peforms health checks on resources
         4. supports hybrid
      2. Amazon Direct Connect
         1. Dedicated private physical network connection from on prem to AWS
         2. Supports hybrid
         3. For large datasets, business critical data, hybrid model
      3. AWS VPN
         1. Site to site VPN creates secure connection between internal network and AWS VPCs
         2. Like direct connect but virtual
         3. Virtual Private Gateway - VPN connector on amazon side
      4. Amazon API Gateway
         1. Build and manage APIs
         2. Share data between systems
         3. Integrates with services like Lambda
8. Utilizing Databases
   1. Amazon RDS (Relational Database Service)
      1. Launch and manage relational databases
      2. Supports popular DB engines
      3. Offers high availability and fault tolerance
      4. Automatic patching, backups, maintencance
      5. Launch read-only replicas for fast querying
   2. Amazon Aurora
      1. Relational DB compatible with MySQL and Postgres
      2. Faster than both
      3. Scales automatically, durable, high available
      4. Managed by RDS
   3. Amazon DynamoDB
      1. NoSQL key-value database
      2. Fully managed and serverless
      3. Scales automatically to massive workloads with fast performance
   4. Amazon DocumentDB
      1. Fully managed serverless MongoDB
   5. Amazon Elasticache
      1. Fully managed in-memory datastore
      2. Compatible with Redis and Memcache
      3. Data can be lost
      4. High performance, low latency
   6. Amazon Neptune
      1. Fully managed graph database
      2. Good for highly connected datasets like social networks
      3. Fully managed and serverless
9. Migration and Transfer Services
   1. Companies need inexpensive, fast, secure ways of migrating to cloud
   2. Database Migration Service (DMS)
      1. Helps migrate databases to or within AWS
      2. Continuous data replication
      3. Supports heterogeneous or homogenous migrations
      4. Virtually no downtime
   3. Server Migration Service
      1. Migrates on-prem servers to AWS
      2. Server saved as new Amazon Machine Image
      3. Use AMI to launch servers as EC2 instances
   4. The Snow Family
      1. Physical devices for data transfer
      2. Snowcone - 8 terabytes of usual storage. Offline shipping or online with AWS Datasync
      3. Snowball - Petabyte scale solution. Transfer data in and out. Cheaper than internet transfer. Supports EC2 and Lambda as well.
      4. Snowmobile - Multi-petabyte or exabyte scale. For complete moves to AWS usually. Loaded to S3. Literally a truck drives your data to Amazon
   5. Amazon Datasync
      1. Allows online data transfer from onprem to AWS storage services
      2. Replicate data across region or across account
10. Leverage Analytics Services
    1. A data warehouse is a data storage solution that aggregates massive amounts of historical data from disparate sources
    2. Primarily used for reporting and analytics
    3. Redshift
       1. Improves speed and efficiency when querying
       2. Handes exabytes of data
       3. Good for consolodating data or data that doesn't require real time transaction processing
    4. Athena
       1. Query service for Amazon S3 using SQL
       2. Pay per query
       3. Considered serverless
    5. Glue
       1. Prepares your data for analytics
       2. ETL service
    6. Kinesis
       1. Allows analyze data and video streams in real time
       2. Supports video, audio, app logs, clickstreams, IOT data
    7. Elastic MapReduce (EMR)
       1. Process big data
       2. Analyze data using Hadoop and other big data frameworks
    8. Data Pipeline
       1. Helps move data between compute and storage services running either on AWS or on-prem
       2. Moves data at specified intervals, or based on conditions
       3. Sends notifications on success or failure
    9. Quicksight
       1. Helps visualize data - dashboards etc
11. Leveraging Machine Learning Services
    1. Rekognition
       1. Allows automation of image and video analysis
       2. Identify custom labels, face and text detection
    2. Comprehend
       1. Natural language processing service 
       2. Finds relationships in text
    3. Polly
       1. Turns text into speech
       2. Sounds natural, many languages, custom voice
    4. SageMaker
       1. Build, train, deploy machine learning models quickly
    5. Translate
    6. Lex
       1. Build conversational interfaces, like chatbots
12. Understanding Developer Tools
    1. Cloud9
       1. Write code within an IDE in browser
    2. CodeCommit
       1. Source control system for private Git repos
    3. CodeBuild
       1. Build and test app source code
       2. Runs test, CI/CD
    4. CodeDeploy
       1. Manages deployment of code to compute services
       2. Maintains uptime
    5. CodePipeline
       1. Automates software release process
    6. X-ray
       1. Helps debug production app
       2. Map app components
       3. View requests end to end
    7. CodeStar
       1. Helps devs collaborate on dev projects
       2. Helps manage the whole pipeline
13. Exploring Deployment and Infra Management Services
    1. Infrastructure as code - allows automation of resource provisioning
    2. CloudFormation
       1. Allows provisioning AWS resources using IaC
    3. Elastic Beanstalk
       1. Technically a compute service used to deploy web apps
       2. Automatically handles deployment
       3. Provisions resources
       4. Monitors app health via dashboard
    4. OpsWorks
       1. Use Chef or Puppet to automate the configuration of servers and deploy code
14. Utilizing Messaging and Integration Services
    1. These types of services help us build loosely couple apps to avoid cascading failure
    2. Queues are used to implement loosely coupled systems. They can hold requests or messages for different services to process.
    3. Simple Queue Service
       1. Message queueing service allows build loosely coupled systems
       2. Allows component to component communication using messages
       3. Multiple components (or producers) can add messages to the queue
       4. Messages are processed async
    4. SNS
       1. Simple notification system
       2. Send emails and text messages from apps
       3. Publish message to a topic
       4. Users can subscribe to messages
    5. SES
       1. Simple email service
       2. Send richly formatted emails from apps
       3. Ideal for marketing and professional campaigns
15. Exploring Auditing, Monitoring, and Logging Services
    1. Gives insight into performance and errors
    2. Cloudwatch
       1. Collection of services that help monitor and observe resources
       2. Detect anomolies
       3. Set alarms
       4. Visualize logs
    3. CloudTrail
       1. Tracks user activity and API calls within accounts
16. Additional Services
    1. Amazon Workspaces
       1. Host virtual desktops in cloud
    2. Amazon Connect
       1. Cloud contact center service
       2. Provides customer service functionality

Security

1. The Shared Responsibility Model
   1. In the cloud there is shared responsibility between you and AWS
      1. AWS is responsible for securing their infrastructure
      2. You are responsible for securing your app
2. Well Architected Framework
   1. Design principles and best practices for running workloads in the cloud
   2. Five pillars
      1. Operational excellence
         1. Plan for and anticipate failure
         2. Deploy smaller, reversible changes
         3. Script operations as code
         4. Learn from failure and refine
      2. Security
         1. Automate security tasks
         2. Encrypt data in transit and at rest
         3. Assign only least privileges required
         4. Track who did what and when
         5. Ensure security in all application layers
      3. Reliability
         1. Recover from failure automatically
         2. Scale horizontally for resilience
         3. Reduce idle resources
         4. Manage change through automation
         5. Test recovery procedures
      4. Performance Efficiency
         1. Use serverless architectures first
         2. Use multi-region deployments
         3. Delegate tasks to a cloud vendor
         4. Experiment with virtual resources
      5. Cost Optimization
         1. Utilize consumption-based pricing
         2. Implement cloud financial management
         3. Measure overall efficiency
         4. Only use what you require
   3. Understanding IAM Users
      1. Identity and Access Management gives you fine grained control over who has access to what
      2. Identities vs access
         1. Identities is who can access your resources
         2. Access is what resources identities can access
      3. Authentication vs authorization
         1. Authentication - I am who I say I am
         2. Authorization - I can do what I request
      4. Users
         1. Entities created in IAM representing a person or app that can access your resources
      5. Groups
         1. Collection of users
         2. Can give access control on group level
         3. Security groups for EC2 are not the same thing - security groups are firewalls
   4. Understanding IAM permissions
      1. Roles
         1. Define access permissions and are temporarily assumed by an IAM user or service
      2. Policies
         1. Manage permissions for IAM users, groups, and roles by creating a polcy document in JSON
   5. Exploring App Security Services
      1. Web Application Firewall
         1. Protects apps against common attack patterns
         2. Protects against SQL injection
         3. Protects against XSS
      2. Shield
         1. Managed DDoS protection service
         2. Standard - free protection against common and frequently occurring attacks
         3. Advanced - enhanced protections + access to 24/7 expert
      3. Macie
         1. Helps discover and protect sensitive data
      4. Config
         1. Assess, audit, and evaluate configs of resources
         2. Tracks over time
         3. Delivers config history file to S3
      5. GuardDuty
         1. intelligent threat detection system that uncovers unauthorized behavior
         2. Uses machine learning
         3. Built in detection for EC2, S3, IAM
         4. Reviews logs
      6. Inspector
         1. Uncovers and reports vulns in EC2 instances
      7. Artifact
         1. On demand access to AWS security and compliance reports
      8. Cognito
         1. Helps control access to mobile and web apps
   6. Utilizing Data Encryption and Secrets Management 
      1. KMS - Key management service
         1. Key gen
         2. Stores and control keys
         3. AWS manages encrypted keys
         4. Automatically enabled for some services
      2. Cloud HSM
         1. Dedicated hardware for generating encryption keys
         2. Generate and manage your own encryption keys
         3. AWS does not have access to keys
      3. Secrets Manager
         1. Manage and retrieve secrets
         2. Encrypt secrets at rest
         3. Integrates with some services

Pricing, Billing, and Governance

1. Understanding Pricing
   1. Three fundamental drivers of cost
      1. Compute - hourly from launch to termination
      2. Storage - data stored in cloud
      3. Outbound data transfer - data in flight moving between systems

   2. Free offer types
      1. 12 month free tier - 12 months following sign up
      2. Always free - Offers do not expire, available to all
      3. Trials - Short term free trials starting from activation date

   3. Services aree priced independently

2. EC2 Pricing
   1. On-Demand
   2. Savings Plan
   3. Reserved Instances
   4. Spot Instances
   5. Dedicated Hosts

3. Lambda Pricing
   1. Number of requests
   2. Code execution time
   3. 1 million requests per month free

4. S3 Pricing
   1. Storage class
   2. Storage
   3. Data Transfer
   4. Request and retrieval

5. RDS Pricing
   1. Running clock hours
   2. Type of database
   3. Storage
   4. Purchase Type
   5. Database count
   6. API requests
   7. Deployment Type
   8. Data Transfer

6. Total Cost of Ownership
   1. Financial estimate that estimates direct and indirect costs of AWS
   2. App Discovery Service
      1. Helps plan migration projects to cloud
      2. Used to estimate TCO

   3. Reduce TCO
      1. Minimize capital expenditures
      2. Utilize reserved instances
      3. Right size resources

7. Price List API
   1. Query price of AWS service
   2. Receive price alerts

8. Billing Services
   1. Budgets
      1. Set custom budgets that alert when costs exceed certain amount
      2. Improves planning and cost control
      3. Cost, usage, reservation budgets
      4. Budget alerts

   2. Budget Types
      1. Cost budgets - plan spend
      2. Usage budgets - plan usage
      3. Reservation Budgets - Set reserved instance or savings plan usage goals

   3. Cost and Usage Report
      1. Comprehensive cost and usage data

   4. Cost Explorer
      1. Vizualize/forecast cost over time

   5. Cost Allocation Tags
      1. Useful for tracking spend
      2. Label resources using key/value pair
      3. Tracks costs via cost allocation report

9. Governance Services
   1. Help maintain control over cost, compliance, security across accounts
   2. Organizations
      1. Manage multiple AWS accounts under one umbrella
      2. Consolidating payment
      3. Automate account creation
      4. Manage access policies 
      5. Cost savings - volume discounts

   3. Control tower
      1. Helps ensure accounts conform to company wide policies

   4. Systems Manager
      1. Visibility and control over AWS resources
      2. Automate operational tasks
      3. Logically group services
      4. Patch and run commands on multiple EC2 instances at once

   5. Trusted Advisor
      1. Real time guidance to help provision resources following AWS best practices
      2. Checks for unrestricted access on certain ports
      3. Checks for MFA, password policy
      4. Checks for S3 buckets access
      5. Checks for RDS public snapshots
      6. Checks for exposed access keys
      7. Checks for CCF optimization

   6. License Manager
      1. Manage on-prem and cloud software licenses

   7. Certificate Manager
      1. Manage HTTPS certs

10. Management Services
   1. Managed Services
      1. Ongoing management of AWS infra

   2. Professional Services
      1. Helps enterprise customers move to a cloud-based operating model

   3. AWS Partner Network
      1. AWS consulting services

   4. Marketplace
      1. Digital catalog of prebuilt solutions available for purchase or license

   5. Personal Health Dashboard
      1. Alerts to events that might impact AWS environment

11. Support Plans
    1. Basic - included for free
    2. Developer - $29 a month
    3. Business - $100 
    4. Enterprise - $15,00
