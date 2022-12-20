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
    * EBS Volume (like a usb stick)
        * (Elastic Block Store)
        * network drive you attach to instances while they run
        * allows your instances to persist data
        * are bound to a specific availability zone
        * uses network to communicate with the instance
        * to move a volume you need to snapshot it
        * delete on termination attribute
            * defaulted for root volume
            * any other attched EBS volue in defaulted to delete
            * use case preserve root volume when instance terminated
    * EBS Snapshots
        * backups of EBS volumes
        * can copy snapshots across AZs or regions
        * EBS snapshot archive
            * move snapshot to archive tier to be chaepr
            * takes 1 - 3 days to restore archive
        * Recycle BIn
            * setup rules for accidental delitions
        * Fast Snapshot Restore (FSR)
            * force full initilization of snapshot to have no latency
            * VERY expensive
    * AMI (Amazon Machine Image)
        * customization of an EC2 instance
        * add OS, software, monitoring etc
        * faster boot/config time
        * AMIs are built for a specific region and can be copied across regions
        * AMI cresation Process
            * start EC2 and customize it
            * stop instance
            * build AMI (will also create EBS snapshots)
            * luanch instances from other AMIs
    * EC2 Instance Store
        * EBS are network drives with good but limited performance
        * EC2 Instance Store is high performance hardware disk
        * lose their storage if instance is stopped
        * good for buffer/ cache/ scratch data/ temporary content
        * risk of data loss if hardware fails
    * EBS Volume Types
        * 6 types
            * GP2/ GP3
                * general performance
                * can be used for boot volumes, dev and test environments
                * 1 GB - 16TB
                * GP 3
                    * 3000 IOPS and 125 MB/s throughput
                    * can increase IOPS independently
                * GP 2
                    * small gp2 volums can burst IOPS to 3,000
                    * 3 
            * io 1/ io 2
                * highest performance SSD for mision critical low latency
                * critical bus apps with sustained IOPS performance
                * Provisioned IOPS (PIOPS)
            * st 1
                * low cost HDD for frequently accessed, throughput intensive workloads
                * cannot be boot volume
            * sc 1
                * lowest cost HDD designed for less frequently accessed workloads
                * cold HDD, data that is infrequently accessed
        * characterized by size, throughput, IOPS (I/O Ops Per Sec)
        * only gp2/gp3 and io 1/io 2 can be used as boot volumes

    * EBS multi attach - IO 1/ IO 2 family
        * attach same EBS volume to multiple EC2s in the same AZ
        * each instance has full read/write permissions
        * Use case
            * higher app availability (Teradata)
            * apps must manage concurrent write operations
        * max 16 EC2 instances at one time
        * must use file system that's cluster aware
    * EBS Encryption
        * encryption done behind the scenes
        * uses keys from KMS (AES 256)
        * encrypt an unencrypted EBS volume
            * create EBS snapshot
            * encrypt EBS snapshot (using copy)
            * create new volume using snapshot
                * make sure encryption selected yes
            * attached encrypted volume to original instance
    * Amazon EFS (Electronic File System)
        * managed NFS (network file system) that can be mounted to many EC2s
        * EFS works in multiple AZs
        * highly available, expensive, pay per use
        * Use cases
            * content management, web serving, data sharing
        * uses security group to control access
        * only Linux based AMIs
        * performance & storage 
            * 1000s of concurrent NFS clients, 10 GB/s throughput
            * sclaes well
            * Performance
                * Performance mode
                    * general purpose
                    * max I/O (big data)
                * througput mode
                    * bursting - throuput scales with size
                    * Provisioned set throughput regardless of size
            * Storage
                * Standard - frequently accessed files
                * infrequent access (EFS IA) 
                    * cost to retrieve files lower cost stoarge.  
                    * lifecycle policy needed
                * availability and durability
                    * standard (Regional) - multi AZ
                    * One zone: One AZ, great for dev or backups
                        * over 90% in cost savings
    * EFS vs EBS
        * EBS can only be attached to instance at a time
        * EBS locked only to one AZ
        * EBS backups use IO and shouldn't be run while app is under heavy load
        * EFS to share website files
        * EFS only for Linux
        * EFS more expensive
# Section 8 ELB & ASG
    * Scalability & High Availability
        * Scalability
            * Vertical - increase size of instance
            * Horizontal - increase number of instances
        * high availability
            * app runs in at least 2 data centers/AZs
    * Elastic Load Balancer (ELB)
        * managed load balancer
        * AWS  takes care of upgrades, maintenence, high availability
        * health checks
            * enable load balancer to know if instance is available
            * done on port and a route (/health)
        * types of load balancer
            * classic load balancer (CLB)
                * don't use (deprecated)
            * Application Load Balancer (ALB)
                * http(s), websocket
            * Network Load Balancer (NLB)
                * TCP, TLS, UDP
            * Gatweay Load Balancer
                * IP Protocol 
        * can be used on public or private networks
        * only allow traffic from load balancer from instances security group
    * Application Load Blancer (ALB)
        * only for HTTP (Layer 7)
        * load balancing to multiple apps across machines (target groups)
        * multiple apps on same machine (containers)
        * supports redirects
        * routing tables to different garget groups
            * based on URL (/users or /posts)
            * routing based on hostname
            * routing based on query string, headers
        * great for micro services & container based apps
        * target groups can be
            * EC2s
            * ECS tasks
            * lamda functions 
            * IP addresses
        * fixed hostname
    * Netowrk Load Balancer (NLB)
        * Layer 4 (TCP & UDP traffic)
        * handles millions of requests per seconds
        * high performance, less latency
        * has one static IP per AZ
        * target groups can be
            * EC2s
            * IP Addresses (must be private IPs)
            * health checks support TCP, HTTP, & HTTPS
    * Gateway Load Balancer (GWLB)
        * use if you want all traffic to be monitored
        * operates at Layer 3 (network layer) IP packets
        * combines network gateway w/ load balancer
        * uses the GENEVE protocol on port 6081
        * trarget groups can be
            * EC2s
            * IP addresses (must be private)
    * Sticky Sessions (Session Affinity)
        * client is always redirected to same instance behind load balancer
        * works for ALB
        * cooke used for stickiness
        * use case (make sure user doesn't lose session data)
        * cookie types
            * application based cookies
                * custom cookie
                    * generated by target
                    * name must be specified be each target group
                    * can't use AWSALB, AWSALBAPP, AWSALBTG
                * Application cookie
                    * generated by ALB
                    * cookie name is AWSALBAPP
            * Duration Based Cookie
                * cookie generated by ALB
                * cookie name is AWSALB for ALB
    * Cross Zone Load Balancing
        * each load balancer distributes evenly across all registered insances in all AZs
        * without cross zone load balance distributes traffic by AZs 
        * for ALB
            * enabled by default on ALB
            * no charges for interAZ data
        * for NLB and GWLB
            * disabled by default
            * pay money for AZ data 
    * SSL/TLS Certificates
        * in flight encryption (between clients and your load balancer)
        * Secure Sockets Layer
        * Transport Layer Security (newer version)
        * can manage certificat4es using AWS Certificate Manager (ACM)
        * Server Name Indication (SNI)
            * solves problem of loading multiple SSL certs onto one web server
            * requires client to indicate hostname of target server
    * Connection Draining/ Deregistration Delay
        * time to complete in-flight requests while the instance is unhealthy
        * stops sending new requests to EC2 instance which is de-registrating
    * Auto Scaling Groups (ASG)
        * scale in/out EC2s automatically
        * can set min and max number of EC2s
        * automatically registers new instances to ALB
        * recreates an EC2 instance if unhealthy
        * Attributes
            * launch template 
            * AMI , user data, EBS volumes etc.
        * able to scale based on CloudWatch Alarms
    * ASG Scaling Policies
        * Dynamic Scaling Policies
            * Target tracking scaling
                * most simple
                * ex. average ASG CPU to stay around 40%
            * Simple/Step Sclaing
                * when cloudWatch alarm is triggered remove/add x units
            * Scheduled Actions
                * Anticipate scaling based on know usage patterns
                * ex increase ASG on friday afternoons
            * Predctive scaling
                * continuously forcast load and schedule ahead
        * good metrics to scale on
            * CPU Utilization
            * Request Count Per Target
            * Average Network In/Out
        * scaling cooldown
            * default 5 mins, 
            * gets activated when scale action happens
            * ASG will not launch or terminate during cooldown
# Section 9: RDS + Aurora + Elasticache
    * RDS (Relation Database Service)
        *  managed SQL database
            * Postgres
            * MySQL
            * MariaDB
            * Oracle
            * Microsoft SQL Server
        * RDS vs deploying own DB?
            * RDS is managed by AWS (less maintenance)
            * continuous backups
            * multi AZ setup for disaster recovery
            * scaling capability
        * storage auto scaling
            * RDS can auto scale if storage about to run out
            * have to set Max Storage Threshold
            * can auto modify storage if
                * free storage is < 10% usage
                * low storage lasts > 5 min
                * 6 hours have passed since last modification
    * RDS Read Replicas for read scalability
        * can create up to 5 read replicas 
            * within AZ, Cross AZ or Cross region
        * replication is ASYNC, eventually consistent
        * application MUST update connection string to leverage read replicas
        * Use Cases
            * have a one time load to handle analytics
        * Network Cost
            * normal cost when data goes from one AZ to another
            * read replicas within same region transfer for free
            * cross region replicas incure network costs
        * RDS Multi AZ (Disaster recovery)
            * SYNC replication
            * One DNS name - auto app failover
            * failover in case of loss of AZ, loss of network
            * NOT used for scaling
            * read replicas CAN be set up as Multi AZ for disaster recovery
        * RDS - Single AZ to multi AZ
            * zero downtime operation
            * click modify on database -> multi AZ
    * RDS Custom
        * managed database with OS and DB customization
        * you can
            * configure settings
            * install patches
            * Enable native features
            * access underlying EC2 instance
    * Amazon Aurora
        * AWS propiertary tech
        * Postgres and MySQL supported by AuroraDB
        * cloud optimized
        * auto grows in increments of 10GBk up to 128 TB
        * can have 15 replicas
        * failover in Aurora is instant
        * Aurora High Availability and read scaling
            * 6 copies of data across 3 AZ
            * self healing 
            * one aurora instance takes writes
        * Aurora DB Cluster
            * reader endpoint connects to all replicas
                * load balancing at connection level
        * Aurora Custom Endpoints
            * define subset of Instances for custom endpoint
            * replaces default reader endpoint
        * Aurora Serverless
            * good for infrequent, intermittent, unpredictable workloads
            * no capacity planning needed
            * pay per second
        * Aurora Multi-Master
            * use for immediate failover for write node
            * every node does Read/Write 
        * Global Aurora
            * Aurora Cross Region read replicas
            * aurora global database (recommended)
            * 1 primary region  (read/write)
            * 5 secondary read-only regions
            * typical cross-region replication takes < 1 second
        * Aurora Machine Learning
            * add ML based predictions via SQL
            * supported services
                * Amazon SageMaker
                * Amazon Comprehend
     * RDS backups
        * automated backups
            * daily full backup of database
            * transaction logs backed up every 5 min
            * 1-35 days of retention, 0 to disable
        * manual DB snapshots
            * manually triggered but retained forever
            * for stopped RDS database do snapshot & restore
        * Aurora backups
            * automated backups
                * 1-35 days, CANT be dsiabled
                * point in time recovery
            * manual DB snapshots
                * kept forever
    * RDS/Aurora Restore Options
        * restoring a backup or snapshot creates new database
        * restore MYSQL DB from S3
            * create backup
            * store on S3
            * restore backup
        * restore MQSQL Aurora from S3
            * create backup using Percona XtraBackup
            * store backup file on S3
            * restore backup file on new Aurora cluster
    * Aurora DB cloning
        * create new DB cluster from existing one
        * faster than snapshot & restore
        * very fast & cost-effective
        * does not impact production DB
    * RDS/Aurora Security
        * At-rest encryption
            * volumes encrypted with KMS, defined at launch time
            * if master not encrypted, read replicas not encrypted
            * to encrypt existing DB, snapshot & resotre as encrypted
        * in-flight encryption
            * TLS ready be default
        * IAM auth - IAM roles to connect to DB 
        * Control network access through Security groups
        * No SSH available except through RDS Custom
        * Audit logs can be enabled and sent to CloudWatch
    * Amazon RDS Proxy
        * fully managed DB proxy for RDS
        * allows apps to pool and share DB connections with DB
        * improves DB efficiency by redusing stress/open connections
        * serverless, autoscaling, higly available, multi-AZ
        * supports MySQL, MariaDB, Aurora
        * enforce IAM auth for DB and securely store creds in Secrets Manager
        * must be accessed through VPC (not publicly available)
        * good for lambda functions
    * Elasticache
        * managed Redis or Memcached
        * in-memory DBs with high performances
        * used for read intensive workloads
        * makes apps stateless
        * Elasticache involves HEAVY app code changes
        * have cache invalidation strategy to make sure data current
        * Redis vs Memcached
            * Redis
                * multi-AZ with auto failover
                * read replicas
                * backup and restore features
            * Memcached
                * multi-node partitioning
                * no replication
                * non persistent
                * no backup and restore, multi-threaded
        * Cache Security
            * does not support IAM auth
            * RedisAUTH
                * set apssword/token for Redis cluster
                * supports SSL in flight encryption
            * Memcached
                * supports SASL auth
            * patterns for Elasticache
                * lazy loading
                * write through
                * Session store
            * Redis use case
                * gaming leaderboads
                * redis sorted sets
                    * guarantee both uniqueness and element ordering
# Section 53
## What is a DNS
    
    


        

        
    
        
        
            





        

        
    
            



        
    
        




                

        


        
            



        






        
    







            









            

        
        






    


    

        
        








    

    
        

