# Section 3 - Getting started
  
    * AWS Global Infrastructure
        * AWS Regions
              * has multiple Availability Zones
            * named like (US-East 1)
            * most services are region scoped
            * how to choose an AWS region?
                * compliance reasons (keep data out of China)
                * proximity (reduce latency)
                * available services (not all regions have all services)
                * Pricing - pricing varies on region
        * Availability Zones
            * each region has multiple availablility zones
            * one or more data centers
            * seperated from other availability zones for redundancy
            * connected with high bandwith, ultra low latency network
        * Data Center
            * has redudant power, networking, and connectivity
        * Points of Presence (Edge Location)
            * 216 points of presence
            * gets content to users

# Section 4 - IAM & AWS CLI
    * IAM - Identity and Access Management
        * Root account is default, SHOULD NOT BE USED 
        * Users are people within an org, can be grouped
        * groups can only contain users, no groups within groups
        * users don't have to belong to a group, but can be part of multiple groups
    * IAM Permissions
        * users or groups can be assigned JSON documents called  policies 
        * polices define the permissions of the users
        * utilize the least privelage principle
        * tags  can be added to help identify users or groups
    * IAM Policies
        * policies attached at group level will cause all group members to inherit policy
        * inline policy - policies attached to individual usuer
    * IAM Policy structure
        * version number -  (ex 2012-10-17)
        * ID (optional)
        * Statement
            *  SID (statement ID)   
            * effect: allow, deny
            * Prinicipal: which account/user/role this policy
            * Action: list of actions policy allows or denies   
            * Resource: which resource from which action applies (which service action or API) 
            * Condition: special conditions for when policy applies
                * "*" wildcard allows access to everything
    * IAM Password Policy
        * set min length
        * Require specific characters
        * Allow IAM users to change their own passwords
        * Require user to change password after some time
        * Prevent password re-use
        * MFA - Multifactor authentication
            * protect root accounts and IAM users with MFA
            * password you know _ security device you own
            * MFA options
                * Virtual MFA devices
                    * Google Authenticator (phone only)
                    * Authy (multi-device)
                * Universal 2nd Factor (U2F) Security Key
                    * Supports multiple root and IAM users
                * Hardware Key Fob MFA Device
                * AWS GovCloud device
                    * only for gov accounts
    * AWS Access keys, CLI, and SDK
        * Ways to access AWS
            * AWS Management Console: protected by password + MFA
            * Command Line Interface (CLI): protected by access keys
            * Software Development Kit (SDK): protected by access keys
        * Access keys are generated through AWS console
        * Users manage their own access keys
            * treat access keys like passwords
        * AWS CLI
            * "aws" root user command
            * direct access to public APIs
        * AWS SDK
            * langauge specifiic APIs
            * embedded within your application
            * most major languages supported
        * Creating access keys
            * user -> security credentials -> access keys
            * CLI command "aws configure"
                * add ID, Key, and default region
        * AWS Cloudshell
            * virtual terminal within management console
    * IAM Roles
        * Permissions for AWS services
        * Some AWS services need to perform actions on our behalf
    * IAM Security Tools
        * IAM Credentials Report (account level)
            * report that lists your account's users and the status of credentials
        * IAM Access Advisor (user level)
            * shows service permissions for a user and last access
            * can be used to see if permission can be removed if not used
    * IAM Best Practices
        * Don't use root account
        * one physical users = one AWS user
        * Assign users to groups and assign permissions
        * Create strong password policy
        * Use MFA
        * Create Roles for giving permission to AWS services
        * Use access keys for programattic access
        * Use IAM credentials to audit permissions
        * NEVER share IAM users
# Section 5 - EC2 Fundamentals
    AWS Budget Setup
        * billing and cost management dashboard - make changes to billing
        * bill details
            * service charges - costs of using services
   ## Amazon EC2 - Elastic Compute Cloud (IAAS)
        * mainly cosists of
            * Renting virtual machines (EC2 instance)
            * storing data on virtual drives (EBS)
            * Distributing load across machines (Elastic Load Balancer)
            * Scale Services (Auto Scaling Group)
    EC2 sizing & config options
        * Operating Systems Linux, Windows, Mac
        * CPU
        * Ram
        * storage space
            * network attached (EBS & EFS)
            * hardware - EC2 Instance Store
        * network card - public IP address
        * Firewall rules - security group
        * Bootstrap script
            * launching commands when instance first starts
                - installing updates
                - installing software
                - download common files from internet
            * runs with root user
        * Amazon Machine Image
            * has OS and some simple startup
        * key pair (use to login to EC2 with ssh)
    EC2 Instance Types
        * naming convention
            i.e. m5.2xlarge
            m: instance class
            5: generation
            2xlarge: size within instance class
        * General Purpose (T class)
            - great for diverse workloads such as web servers or code repos
            - balance between compute, memory, networking
        * Compute Optimized (C class)
            * things that require high performance processors
                - batch processing
                - media transcoding
                - high performance web servers
                - High performance computing
                - machine learning
                - gaming servers
        * Memory Optimized (R, X1, Z)
            * used for databases
            * distriubted web scale cache stores
            * process lots of unstructured data
        * Storage optimized (I, D, H1)
            * Online transaction processing systems
            * Relational & NoSQL databases
            * Data warehousing
    ## Security Groups
    * Controls how traffic is allowed in or out of EC2 instances
    * Security Groups only contain allow rules
    * act as "firewall" for EC2
    * regulate
        * access to ports
        * IP ranges
        * inbound network
        * outbond network
    * SG has following in rule
        * type (SSH, HTTPS, etc.)
        * protocol (TCP/ IP)
        * Port Range
        * Source (IP address)
    * can be attedhed


            

        
        






    


    

        
        








    

    
        

