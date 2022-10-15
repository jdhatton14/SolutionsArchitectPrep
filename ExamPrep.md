* Section 2 Intro to AWS
    * Cloud Computing Defined
        Internet based solution
        operates on servers
        limited knowledge of implementation

    * Benefits of cloud computing
        * Reduced
            - hardware cost
            - operational cost
            - deployment cost
        
        * Increased
            - resiliency (health monitoring)
            - performance (auto scaling)
            - capacity (storage also scales if needed)

    * Cloud Computing Models
        Full Cloud Deployment
            All compponents are in the cloud (database, processing, storage)
            nothing on prem
        Hybrid Deployment
            some in the cloud, others are on prem (stuff that needs to be instant)
        IAAS (Infrastructure ias a service)
            entire infrastructure is in the cloud
                servers and stuff in the cloud, apps on prem
            must manage it all
        PAAS (Platform as a service)
            don't manage infrastructure
            applications are deployed on platforms
        SAAS (Software as a service)
            someone else develops the software
    * AWS History
        * 2004 AWS public launch
        * 2006 - S3, EC2, SQS launched
        * 2007 - Simple DB launched
        * 2008 - Elastic IP launched (allowed static IP for EC2 instances)
        * 2009 - Management console and VPC introduced
        * 2010 - Route 53 (DNS system), SNS (notification service), IAM (security)
        * 2011 - ElastiCache and CloudFormation
        * 2012 - SWF, DynamoDB (NoSQL DB)
        * 2014 - Redshift (SSD storage)
        * 2015 - CloudTrail, WAF, Data Pipeline
    * AWS Platform
        - built on top of compute capabilities
            processor, memory, storage
        Core Components
        * Storage
            *  S3 Storage (Simple Storage Solution)
                  block based or object storage
            * Amazon RDS (databae as a service functionality)
            * Amazon Vitrual Private Cloud (networking)
            * Identity and Access Management (IAM) - security
    * AWS Service Overview
        * Compute
            * EC2 - launch virtual machines and servers manually
            * Elastic beanstalk - automatically creates environment for launching webapp
                sets up instances and networking
            * Lambda
            * ECS
        * Storage
            * S3 (simple storage service)
            * EFS (Elastic File System)
                EC2 uses EFS
            * Glacier
                used for archival purposes (data that is not needed for a long time, if ever)
            * Storage Gateway
                access storage that is in the cloud locally or vice versa
        * Databases
            * RDS
            * DynamoDB
            * Elasticache
                keeps data in memory
            * Amazon Redshift
        * Migration
            * AWS migration Hub 
                take existing vms into AWS
            * App, Database, Server migration services
            * Snowball
                ship you a box that you can put all your data to be transferred to a VPC
        * Network & Content Delivery
            * VPC
            * Cloudfront
            * Route 53 DNS
            * API Gateway
            * Direct Connect - connect data center to cloud
        * Management
            * Cloudwatch - monitoring
            * AWS Auto Scaling
            * CloudFormation
            * CloudTrail
            * Trusted Advisor
        * Media Services
            * Transcoding, video streams
        * Machine Learning
        * Analytics
            * Kinesis
        * Security, Identity & Compliance
            * IAM
            * Cognito
            * Inspector
            * AWS Orginizations
            * WAF & Shield
        * AWS Cost Management
            * Cost Explorer
            * Budgets
        * Mobile Services
        * AR and VR
        * Application Integration
            * SNS (Simple Notification Service)
            * SQS (Simple Queue Service)
        * Customer Engagement
        * Business Productivity
        * IOT (Internet of Things)
    * AWS Security and Compliance
        * AWS responsibility
            if Amazon manages it, there responsibility
            * security of server locations
            * managed service security (DynamoDB)
        * dev responisbility
            * instance security
    * Regions and Availability Zones
        * Region
            * physical location or boundary with multiple AWS data centers
            * not a country or US state necissarily
        * Availability Zones
            * 1-6 data centers
            * redundant power and networking
                if one data center fails the others take over the load
            * multiple availability zones within regions
        * edge areas 
            * gets data closer to users (used in cloud front)
            

