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
    Amazon EC2 - Elastic Compute Cloud (IAAS)
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
    Security Groups
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
        * SGs can be attached to multiple isntances
        * locked down to a region/VPC
        * lives outside of EC2
        * time out -> SG issue, connection refused -> app error
        * by default all inbound traffic blocked, all outbound allowed
        * Ports to know
            * 22 - SSH
            * 21 - FTP
            * 22 - SFTP: SSH file transfer
            * 80 - HTTP
            * 443 - HTTPS 
            * 3389 - RDP (Remote Desktop Protocol)
    EC2 Instance Purchasing Options
        * On Demand
            * highest cost but no upfront payment
            * no long term commitment
        * Reserved (1 & 3 years) - long workloads
            * cheaper than on demand
            * reserve specific instance attribute
            * different payment options
        * Convertible Resrved - long workloads with flexible instances

        * Savings Plans (1 & 3 years) - commitment to an amoount of usage
            * get discount based on long term usage
            * any usage beyond plan is billed at on demand price
            * locked to specific instance family & AWS region
        * Spot instances - short workloads, cheap, can lose instances
            * up to 90% discounts
            * Most cost-efficient instances in AWS
            * useful for batch jobs, data analysis, distributed workloads
            * not suitible for critical jobs or databases
        * Dedicated hosts - book an entire server
            * good for complience or server bound software licenses
            * most expensive option
            * BYO licence models go here
        * Dedicated instances - no other customers share hardware
            * mayshare hardware with other instances in same account
            * no control over instance placement
        * Capacity Reservation - reserve capacity in a specific Avail Zone
            * no time commitment, no billing discounts
    EC2 Spot Instances Deep dive
        * Define max spot price
            * as long as current spot price < max, keep instance
            * Spot block
                * keep instance for specified time frame without interruptions
            * Spot Requests can only be canceled when open, active, or disabled
                * need to cancel spot requests and then terminate spot instances
        * Spot Fleets
            * set of spot instances & (optional) on demand instances
            * will try to meet target capcity with price constraints
                * define multiple launch pools: instance type, OS, AZ
                * can have mult launch pools
                * stops launching instances when reaching capacity or max cost
            * strategies to allocate spot instances
                * lowestPrice
                * diversified - distributed across all pools 
                    * great for availability, long workloads
                * capcityOptimized: pool with optimal capacity
            * spot fleets allow us to automatically request Spot instances w/ lowest price
# Section 6 - EC2 solutions architect
    * Private vs Public IP
        * Public IP
            * can be found on the web (WWWW)
            * must be unique on the WWW
            * can be geo located easily
        * Private IP
            * can only be found on private network
            * IP must be unique to private network
            * two different private networks can have the same IP
            * machines connect to WWW using NAT + a proxy
            * only a specific range can be used for IPs
        * Elastic IPs
            * start and stopping EC2s can change public IP
            * if you need a fixed public IP, you need an Elastic IP
            * can attach to one instance at a time
            * can mask the failure of an instance or software by rapidly remapping the address
            * you can only have 5 elastic IPs
            * AVOID ELASTIC IPS
                * instead use random public IP and register DNS name
                * or use load balancer 
        * EC2s come with a private IP and public IP
    * Placement Groups
        * control EC2 instance placement strategy
        * 3 strategies
            * Cluster - clusters instances into low latency group in single AZ zone, high risk high reward
                * same rack, Same AZ
                * great network speed
                * if rack fails, all instances fails at the same time
                * use cases
                    * big data job that needs to complete fast
                    * low latency, high network throughput
            * Spread - have max 7 instances per group per AZ, good for critical applications
                * different hardware for each instance
                * reduced risk of simultaneous failure
                * use case
                * cons limit of 7 instances per AZ per group
                * use case
                    * high availability programs
            * Partition - different sets of racks within an AZ
                * each subgroup (patition) is a rack within AZ
                * can be distributed through multiple AZs
                * pros - 7 partitions per AZ
                    * up to 100s of EC2 instances
                * a partition failure doesn't affect other partitions
                * use cases
                    * distributed jobs
                    * big data (Hadoop, HBase, Kafka)
    * Elastic Network Interfaces
        * logical component that represents a virtual network card
        * has the following attributes
            * Primary private IPV4
            * secondary IPV4(s)
            * public IPV4
            * one or more security groups
            * MAC address
        * can create ENI independently and attach to EC2 instances
        * bound to one AZ
        * useful for network failover if an instance fails
    * EC2 Hibernate
        * RAM is preserved
            * instance boot is much faster
            * OS is not stopped
        * root EBS volume must be encryped
        * use cases
            * long running processes
            * services that take time to initialize
        * supports many instance families
        * instance rame size < 150GB
        * doesn't work for bare metal instances
        * available for On-demand, reserved, and spot instances
        * cannot be hibernated for more than 60 days
# Section 7 - EC2 Instance Storage



        
    







            









            

        
        






    


    

        
        








    

    
        

