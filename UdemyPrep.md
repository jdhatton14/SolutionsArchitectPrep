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
# Section 10: Route 53
    * What is a DNS
        * Domain Name System
        * translates human friendly hostnames into IP addresses
        * DNS terminologies
            * Domain registrar: goDaddy
            * DNS records
            * Zone file
            * Name Server
            * Top Level domain - .com, .gov, etc
            * Second level domain: amazon.com, google.com
    * How DNS works
        * web browser asks local DNS server
        * local DNS miss calls Root DNS server
        * root server sends to domain (.com) server
    * Route 53
        * fully managed and Authoritative DNS
        * Authoritative - customer can update DNS records
        * Route 53 is also a Domain Registrar
        * 53 - port for DNS services
        * Route 53 records
            * domain/subdomain Name
            * record Type
            * Value
            * Routing Policy
            * TTL (Time to Live)
        * Route 53 supported DNS record types
            * A/ AA / CNAME / NS
        * Route 53 Record Types
            * A - maps hostname to IPV4 address
            * AAAA  - maps hostname to IPV6
            * CNAME - maps hostname to another hostname
                * ca't create CNAME record for top node of DNS namespace
            * NS - Nameservers for hosted zone
        * Hosted Zones
            * container for records that deine how to route traffic
            * Public hosted zone
                * contains records that specify how to route public internet traffic
            * Private hosted zone
                * contains records that show how traffic within VPC works
            * pay $.50 per month per hosted zone
        * Route 53 Record TTL
            * cache the result for the TTL
            * high TTL
                * less traffic on route 53
                * possibly outdated records
            * low TTL
                * more traffic on Route 53, more cost
                * re orcs are outated for less time
                * easy to change records
            * TTL mandatory for Alias records
        * CNAME vs Alias
            * AWS resources expose an AWS hostname
            * CNAME - point hostname to another hostname
                * ONLY WORKS FOR NON ROOT DOMAIN 
                    (aka.something.mydomain.com)
            * Alias - points hostname to an AWS resource
                * Worrks for root domain AND non root
                * free of charge
                * automatically recognizes changes in resource IP
                * Alias record is always of type A/ AAAA
                * can't set TTL
                * targets
                    * ELB, CloudFront, API Gateway, S3 Website
                    * ElasticBeanstalk env, VPC interface endpoints
                    * Route 53 record in same hosted zone
                    * CANT use EC2 DNS names for target
        * Route 53 Routing Polices
            * defines how Route 53 responds to DNS queries
            * Route 53 supports the following
                * Simple
                    * route traffic to single resource
                    * can specify multiple values in same record
                    * if multiple, random one chosen by client
                    * when alias enabled, only specify 1 AWS resource
                    * CAN'T be used for health checks
                * Weighted
                    * control % of requests go to each resource
                    * assign each record a relative weight
                        * sum of weights don't need to be 100
                    * DNS records must have same name and type
                    * good for load balancing from regions
                    * weight of 0, stop sending traffic to resource
                * Latency based
                    * redirect resource that has lowest latency
                    * latency is base on traffic between users and AWS regions
                    * can be associated with health checks
            * Route 53 health Checks
                * only available for PUBLIC resources
                * health check -> automated DNS failover
                * health checks for an endpoint
                    * 15 global health checkers
                    * if > 18% of health checkers return healthy, considered healthy
                    * health checks can check text in first 5120 bytes of response
                    * configure firewall to allow
                * health checks that monitor health checks
                    * calculated heath checks
                    * combine health checks with OR,AND,NOT
                    * can monitor up to 256 health checks
                * health checks that monitor CloudWatch
                    * used for route 53 health checkers for private endpoints
            * Routing Policies - Failover (Active-Passive)
                * primary/seconday with primary health check mandatory
                * if unhealthy, use secondary
            * Routing Policies - GeoLocation
                * routing based on physical user location
                * specify locaiton by continent, country or US state
                * should create default record
            * Routing Policies - Geoproximity
                * route traffic based on location of users AND resources
                * ability to shift more traffic to resources based on bias
                * change size of geographic region, spcify bias values
                    * to expand - more traffic 
                    * to shrink - less traffic
                * must use Route 53 traffic flow to use this feature
            * Routing Polcies - Multi-value
                * use to route traffic to multiple resources
                * can be associated with health checks
                * up to 8 healthy records are returned for each query
                * is not a substitute for having an ELB
            * Domain Registrar vs DNS Service
                * can buy domain name with any domain registrar
                * domain registrar usually provides you with a DNS service
                * can use DNS service to manage DNS records
                * to use route 53 for non AWS domain
                    * create hosted zone in route 53
                    * update NS records on 3rd party site to use route 53 Name Servers
# Section 11 - Solutions Architecture discussion
    * WhatisTheTime.com
        * no DB, start small and accept downtime
        * public vs Private IP and EC2 instances
        * Elastic IP vs Route 53/ Load Balancers
        * Route 53 TTL, A records and Alias records
        * maintain EC2s manually vs ASG
        * Multi AZ
        * ELB health checks
    * MyClothes.com
        * session stickiness
        * cookies
        * server session using Elasticache  
    * MyWordPress.com
        * fully scalable with picture uploads
    * Instantiating Apps quickly
        * it can take time to
            * install applications
            * insert intial data
            * configure everything
            * launch app
        *  EC2 instances
            * use Golden AMI: install apps OS, etc before and launch EC2 from Golden AMI
            * bootsrap using User Data scripts
            * Hybrid mix using golden AMI and user Data (elastic beanstalk)
        * RDS Databases
            * restore from snapshot instead of large insert statement
        * EBS volumes
            * restore from snapshot when possible
    * Elastic Beanstalk
        * devloper centric view of deploying an application on AWS
        * automatically handles capacity provision, load balancing etc
        * just the app code is the responsibility of the developer
        * components
            * application - collection of elastic beanstalk components
            * app version
            * environment
                * collection of AWS resources
                * Tiers (Web server vs Worker environment)
                * can create mult environment (dev, test, prod)
# Section 12: Amazon S3
    * advertised as "infitely scaling" storage
    * many webapps use S3 as a backbone
    * use cases
        * backup and storage
        * disaster recovery
        * archive
        * hybrid cloud storage
        * app/media hosting
        * data lakes & big data analytics
    * S3 buckets
        * stores objects (files) in "buckets" (directories)
        * buckets must be globally unique
        * buckets defined at region level
        * naming convention 
            * letters, numbers, hyphens
    * S3 objects
        * files have a key
        * key is the FULL path
            * prefix + object name (s3://myBucket/myfolder/myfile.txt)
        * values are the content of the body
        * max object size is 5 TB
        * if uploading > 5 GB, use multi-part upload
    * Security
        * User-based
            * IAM Policies
        * Resource-based
            * Bucket policies - bucket wide rules from SC3 console
                * allows cross account access
            * Object Access Control List (ACL) - finer grain
            * Bucket Access Control List (ACL) - less common
        * An IAM principal can access S3 object if IAM allows it OR resource policy allows AND there's no explicit deny
    * S3 Bucket policies
        * JSON based
            * resources: buckets and objects
            * Effect: Allow/Deny
            * Actions: Set of API to allow or deny
            * Principal: the account or user to apply policy
        * use S3 policy to grant public access
        * buckets settings for block public address will override any bucket policy
            * can be set at account level if never needed public
    * Amazon S3 - Static website
        * bucket must allow public reads
    * Amazon S3- Versioning
        * enabled at bucket level
        * same key overwrite will change "version"
        * best practice to version your buckets
            * protect against unintended deletes
            * easy roll back to previous version
        * any file not versioned prior to enabling version has null version
        * suspending versioning does not delete previous versions
    * S3 Replication
        * must enable versioning in source and destionation buckets
        * Cross Region Replication (CRR)
        * Same Region Replication (SRR)
        * buckets can be in different AWS accounts
        * copying is asynchronous
        * must give proper IAM permission to S3
        * use cases
            * CRR - compliance, lower latency
            * SRR - log aggregation, live replication between prod
        * only new objects are replicated after enabling replication
        * can use S3 Batch replication
        * no chaining of replications
    * S3 Storage Classes
        * Standard
            * 99.9999% availability
            * used for frequently accessed data
            * use cases - big data, mobile
        * Standard-Infrequent Access
            * less frequent access but rapid access when needed
            * 99.9% availability
            * use cases - disaster recovery, backups
        * One Zone- Infrequent Access
            * high durability in single AZ; data lost when AZ is destroyed
        * Glacier Instant Retrieval
            * millisecond retriveal
            * min storage duration 90 days
        * Glacier Flexible Retrieval
            * Expedited (1-5 min), Standard (3-5 hrs), Bulk (5-12 hrs)
            * min storage duration 90 days
        * Glacier Deep Archive
            * standard (12 hours)
            * bulk (48 hrs)
            * min storage duration 180 days
        * Intelligent Tiering
            * small monthly monitoring and auto-tiering fee
            * move objects automatically between access tiers based on usage
            * tiers
                * frequent access (automatic): default
                * infrequent access: objects not access for 30 days
                * archive instant access: object not accessed for 90 days
                * archive access tier: 90-700 days
                * deep archive access tier
# Section 13: IAM Roles and Policies
    * Hands on stuff
# Section 14: Advanced Amazon S3
    * Moving between storage class
        * can go from standard down
        * for infrequently accessed object use standard IA
    * Lifecycle Rules
        * configure objects to transition to another storage class
        * Transition actions - set objects to move to another class
        * Expieration actions - delete after some time
        * rules can be created for a certain prefix
    * Amazon S3 Analytics
        * helps you decide when to transition objects
        * recommendations for Standard and Standard IA
        * 24 -48 hours to see data analysis
    * S3 Requester Pays
        * in general, bucket owners pay for S3 operations
        * if this option enabled, network costs of request paid by requester
        * helpful when you want to share large datasets with accounts
        * requester must be auth by AWS
    * S3 Event Notifications
        * can notify on object creation, removed, etc
        * use case: generate thumbnail of images uploaded to S3
        * can pipe event notifications to SNS, SQS, Lambda
            * can also use Amazon eventBridge
    * S3 baseline performance
        * 3,500 Put/COPY/Post actions per second per prefix
        * 5,500 Get request per second per prefix
        * optimizing performance
            * multi-part upload recommmend for > 100 mb
                * REQUIRED fore > 5 GB
            * S3 transfer acceleration
                * increase transfer speed by transfer file to edge locaton
                * compatible with multi-part upload
            * S3 Byte-Range Fetches
                * parralelize GETs by requesting specific byte ranges
    * S3 select & Glacier Select
        * use SQL to perform server-side filtering
        * can filter by rows & columns
        * less network transfer, less CPU cost
    * S3 batch operations
        * perform bulk operations on existing S3 objects
            * modify object metadata & properties
            * Encrypt un-encrypted objects
        * can use S3 inventory to get object list and use S3 Select
# Section 15: Amazon S3 Security
    * Can encrypt objects 4 ways
        * Server Side Encryption (SSE)
            * SSE with Amazon S3 Managed Keys
                * uses keys handled, managed, and owned by AWS
                * objects encrypted server side
                * encryption type is AES-256
                * must set header "x-amz-server-sid-encryption":"AESw56"
            * SSE with KMS Keys
                * use keys managed by Key Management Service
                * user control + audit key usage using cloud trail
                * must set header "x-amz-server-side-encryption":"aws:kms"
                * may be inpacted by KMS limits
                    * throttling if limits outreached
            * Server Side Encryption - Customer
                * fully managed by customer outside AWS
                * S3 does not store encryption key
                * HTTPS must be used 
                * encryption key must be in every HTTP request header
        * Client-side Encryption
            * can use S3 client-side encryption library
            * client encrypts data before sending to S3
            * client decrypts data when retrieving from S3
        * Encryption in transit (SSL/TLS)
            * encryption in flight
            * S3 exposes two endpoints (HTTP or HTTPS)
            * HTTPS is recommended
            * HTTPS is MANDATORY for SSE-C
    * Default encryption vs Bucket Policies
        * one way to force encryption is to use bucket policy and refuse any PUT to S3 object without encryption headers
        * other way is to use default encryption opetion
        * bucket policies enacted before default encryption
    * CORS
        * Corss-Origin Resource Sharing
        * Orgin = scheme (protocol) + host (domain) + port
            * ex https://www.example.com
        * Wbe browser based mechanism to allow requests to other origins while visiting the main origin
        * requests to cross origin won't be fulfilled without CORS header
        * Amazon S3 - CORS
            * to allow cross-origin request enable correct CORS headers
            * can allow for specific origin or for * (all origins)
    * S3 MFA Delete
        * Multi-factor auth in order to delete
        * required when
            * permanently delete an object version
            * suspend version on the object
        * not required when
            * Enabling versioning
            * list deleted versions
        * to use MFA delete, versioning must be enabled on bucket
        * only root account can enable/disable MFA delete
    * S3 Access Logs
        * log all access to S3 buckets
        * any request made to S3 will be logged into another S3 bucket
        * target logging bucket must be in same AWS region
    * pre-signed URLs
        * generated by S3 console, CLI, or SDK
        * URL Experiation
            * console - 12 hrs
            * CLI - 168 hours
        * users given pre signed URL inherit permissions of the user that generate the URL for GET/PUT
        * temporary to one specific file for download
    * S3 Glacier Vault Lock
        * Adopt a WORM model (Write Once Read Many)
        * create a vault lock policy
        * lock the poicy for future edits
        * helpful for compliance and data retention
    * S3 Object lock (versioning enabled)
        * adopt WORM model
        * block an object version deletion for a specified time
        * retention mode - compliance
            * object versions can't be overwritten or deleted
            * objects retiontion modes cant be changed
            * retention periods can't be shortened
        * retention mode - governance
            * most users can't overwrite or delete object version
            * some (admin) users can change retiontion or delete
        * retention period - protect object for fixed period of time
        * Legal hold
            * protects object indefinitely, independen from retention period
            * can be freely placed and removed with "s3:PutObjectLegalHold" IAM permission
    * S3 Access Points
        * grant access to specific prefix of S3 bucket
        * each access point gets its own DNS and policy to limit who can access it
        * S3 Object Lambda
            * use lambda functions to change the object before it is retrieved by the caller app
            * Only one S3 bucket is needed, on top of which we create S3 Access Point and S3 Object Lambda Access points
# Section 16: CloudFront & Global Accelerator
    * CloudFront
        * Content Delivery Network (CDN)
        * Improves read performance, content cached at edge locations
        * DDoS protection, integration with Shield, AWS WAF
        * Origins
            * S3 Buckets
                * for distributin files and caching at edge
                * enhanced security with Origin Access Control (OAC)
                    * OAC is replacing Origin Access Identity (OAI) 
                    * CloudFront can be used as an ingress (upload)
            * Custom Origin (HTTP)
                * ALB
                * EC2 instance
                * S3 website (must be static)
        * use ALB or EC2 as an origin
            * use security group that allows public IP of edge locations
        * Cloudfront Geo Restriction
            * can restritc access to your distribution
            * allowlist - allow users from cetain contry
            * blocklist - block users from certain country
        * Price Classes
            * cost of data out per edge location varies
            * can reduce number of edge locations for cost reduction
            * three price classes:
                * Price class all: all regions - best performance
                * Price class 200: most regions except most expensive
                * Price Class 100: only least expensive regions
        * Cache Invalidations
            * if you update back-end origin cloudfront won't know about it
            * have to wait for TTL to expire
            * can force entire or partial cache refresh through cloudfront invalidation
            * invalidate all files (*) or with specific path
    * Global Accelerator
        * Unicast IP
            * one server holds one IP address
        * Anycast IP 
            * servers hold the same IP and client is routed to nearest one
        * global accelerator uses Anycast IP
        * leverage AWS internal netowrk to route to your app
        * talks to closest edge location on private AWS
        * 2 anycast IPs are created for your application
        * the anycast IPs send traffic directly to edge location
        * work with Elastic IP, EC2 instances, ALB, NLB
        * has health checks for your app
        * auto failover to healthy endpoint
        * DDoS protection through AWS shield
# Section 17: AWS Storage Extras
    * AWS Snow Family 
        * highly secure, portable devices
        * Data Migration
            * transfer a lot of data fast
            * can save on high network cost
            * Three devices
                * Snowball Edge
                    * storage optimized
                        * 80TB
                    * compute optimized
                        * 42TB  
                * Snowcone
                    * small little box, light
                    * 8TB of storage
                    * use snowcone where snowball does not fit
                    * must provide own battery/ cables
                    * can use AWS offline or AWS datasync
                * Snowmobile
                    * huge truck, huge amounts of data (1 mil TB)
        * Edge Computing
            * Edge Locations
                * process data while it's being created on edge location
                * truck on road, ship on sea, etc
                * location may have
                    * limited/ no internet access
                    * limited / no easy access
                * use cases of edge computing
                    * preprocess data
                    * ML at edge
                    * transcoding media streams
            * Snowcone
                * 2 CPUS, 4 GB Ram
                * USB-C power
            * Snowball Edge
                * Compute Optimized
                    * 52 vCPUs, 208 GB Ram
                * storage Optimized
            * all can run EC2 instances & AWS lambda functions
        * AWS OpsHub
            * OS to handle snowball devices
        * cannot import from snowball into Glacier directly
            * move to S3 first, then use lifecycle policy
    * Amazon FSx
        * launch 3rd party file systems on AWS
        * fully managed service
        * supported systems
            * Lustre
                * distributed file system for large scale computing
                * ML, High Performance Computing (HPC)
                * seamless integration with S3
                    * read S3 as file system
                    * write output of computation back to S3 
                 * deployment options
                    * scratch file system
                        * temp storage
                        * data is not replicated
                        * high burst
                        * usage: short-term processing, optimize costs
                    * persistent file system
                        * long term storage
                        * data is replicated within same AZ
                        * replace failed files within minutes
            * Windows File Server
                * Supports SMB & Windows NTFS
                * Active Directory integration, ACLs, user quoatas
                * can be mounted on Linux EC2 instances
                * supports Microsofts Distributed File System Namespaces
                * can be Multi-AZ
                * data backed up to S3
            * NetApp ONTAP
                * compatible with NFS, SMB, iSCSI protocol
                * move workloads from ONTAP or  NAS to AWS
                * storage shrinks or grows automatically
                * instantaneous cloning
            * OpenZFS
                * compatible with NFS
                * move workloads running on ZFS to AWS
                * snapshots, compression and low-cost
                * instantaneous cloning
    * Stroage Gateway
        * AWS is pushing for "hybrid cloud"
            * part of infra in cloud, part on-prm
        * Storage Cloud native options
            * Block storage - EBS
            * File storage - EFS/FSx
            * Object storage - S3/ S3 Glacier
        * storage gateway bridge between on-prm and cloud data
        * use cases
            * disaster recovery
            * backups
            * tiered storage
            * on-prm cache & low-latency file access
        * types of storage gateway
            * S3 File Gateway
                * configured S3 buckets accessed through NFS/SMB
                * most recently used data is cached in the gateway
                * supports S3 except glacier
                    * transition to Glacier using lifecycle policy
                * bucket access usign IAM roles
            * FSx file Gateway
                * local cache for frequently accessed data
                * windows native compatibility
            * Volume Gateway
                * block storage usiing iSCSI protocol backed by S3
                * backed by EBS snapshots
                * cached volumes - low latency access to most recent data
                * stored volumes: entire dataset is on-prm, scheduled backups to S3
            * Tape Gateway
                * backup process using physical tapes
                * backed up by S3 and Glacier
                * works with leading backup vendors
        * storage gateway - hardware appliance
            * can buy on amazon.com
            * used when virtualizaiton not available
    * AWS Transfer family
        * fully managed service for file transfers using FTP
        * supported protocols
            * AWS transfer for FTP (File transfer Protocol)
            * AWS Transfer for FTPS (FTP over SSL)
            * AWS Transfer for SFTP (Secure FTP)
        * pay per provisioned endpoint + data transfer in GB
        * store and manage user credentials with service
    * AWS DataSync
        * move large amount of data to and from 
            * On-prm/ other coluds to AWS (needs agent)
            * AWS to AWS (diff storage service) - no agent needed
        * can synch to
            * S3
            * EFS
            * FSx
        * replication tasks can be scheduled hourly, daily, weekly
        * File permissions and metadata are preseerved (NFS POSIX, SMB)
        * one agent task can use 10 Gbps
    * Stroage Comparison
        * S3 - Object Storage
        * S3 Glacier: Object Archival
        * EBS Volumes: network storage for one EC2 isntance
        * Instance storage: physical storage for EC2, high IOPS
        * EFS: Network file system for LInux, POSIX
        * FSx for WIndows: NFS for WIndows
        * FSx for Lustre: high performance computing
        * FSx for NetApp ONTAP: High OS Compatibility
        * FSx for OpenZFS: Managed ZFS 
        * Stroage Gateway: 
            * S3 & FSx file gateway
            * volume gateway (cache & stored)
            * EFS gateway
        * Transfer Family : FTP, FTPS, SFTP to AWS
        * DataSync: Schedule data sync from onprem to AWS, AWS - AWS
        * Snowcone/ SnowBall/ Snowmobile : move large data to cloud
        * dabtabase: for specific workloads usually for indexing
# Section 18 - Decoupling apps: SQS, SNS, Kinesis, Active MQ
    * two patterns of communicaiton
        * Synchronous communication (app to app)
            * can become problem with sudden spikes of traffic
        * Asynch/ Event based (app to queue to app)
    * decouple apps
        * SQS: queue model
        * SNS: pub/sub model
        * Kinesis: real time streaming model
    * SQS (SImple Queue Service)
        * Standard Queue
            * oldest offering
            * fully managed service, used to decouple apps
            * unlimited throughput, unlimited # of messages
            * default retention - 4 days, max 14 days
            * low latency
            * < 256KB per message sent
            * can have duplicate messages
            * can have out of order messages
        * producing message
            * put on queue using SDK (SendMessage API)
            * msg persisted until consumer deletes it
        * consuming message
            * Poll SQS for messages
            * Process message
            * delete message using DeleteMessage API
        * consumers recieve and process messages in parallel
        * SQS security
            * Encryption
                * in-flight encryption using HTTPS
                * at-rest encryption using KDS
                * client-side encryption if client wants DIY 
            * Access Controls: IAM poicies regulate access
            * SQS access policies (similar to S3 bucket policies)
                * useful for cross account access to SQS queries
        * SQS message visibility timeout
            * after msg polled by consumer, it becomes invisible to other consumers
            * default timeout is 30 seconds
            * message must be processed in that 30 secs
            * if message not processed within timeout, msg processed twice
            * a consumer can call ChangeMessageVisibility API to get more time
            * high timeout will cause long re-processing time
            * low timeout may get duplicates
        * SQS Long Polling
            * when conusmer requests messages from queue, it can optionally wait for messages to arrive
            * decreases number of API calls to SQS while increasing efficiency and latency
        * FIFO Queue
            * First in First out ordering
            * limited throughput: 300 msg/s up to 3000 msgs/s
            * excactly once send capability
            * messages processed in order
        * SQS with ASG (good use case)
            * can scale on CLoudWatch metric - queue length
                * approxNumberOfMessages
            * alarm when NumMsgs eclipsed to add another consumer
            * if load is too big some transactions may be lost
            * can use SQS as buffer to DB writes
    * SNS (Simple Notification Service)
        * send one message to many receivers
        * event producer only sends msg to one SNS topic
        * many event receivers get notification
        * publish/subsribe model
        * how to publish
            * Topic Publish
                * create topic
                * create subsription
                * publish to topic
            * Direct Publish
                * create platform app
                * create platform endpoint
                * publish to platform endpoint
                * works with Google, Apple, Amazon
        * SNS Security
            * in flight encryption with HTTPS
            * at rest encryption using KMS keys
            * IAM policies to regulate Access
            * SNS access policies
        * SNS + SQS Fan Out Pattern
            * push once in SNS, receive in all SQS queues that subsribe
            * fully decoupled, no data loss
            * SQS allows for data persistence, delayed processing
            * Cross-Region delivery with SQS in different regions
            * SNS FIFO Topic
                * preserves message ordering
            * Message filtering
                * use JSON policy to filter only specific messages
    * Kinesis
        * collect, process, analyze streaming data in real time
        * data can be app logs, metrics, clickstreams, etc
        * Kinesis Data Streams
            * capture, process, store data streams
            * made of multiple shards
            * producers send records to data stream shard
                * partition key, data blob
            * consumers recieve record to process
            * retention between 1 day to 365 days
            * ability to reprocess data
            * once data inserted, it can't be deleted
            * data that shares same partition goes to same shard
            * producers - SDK, Kinesis Producer Library, Kinesis agent
            * Consumers
                * write your own, Kinesis Client Lib, AWS SDK
                * Managed: Lambda, Data Firehose, Data Analytics
            * Capacity Modes
                * Provisioned mode
                    * choose # of shrads, scale manually 
                    * pay per shard provisioned per hour
                * On-demand mode
                    * no need to manage capacity
                    * auto scaling based on throughput peak
                    * pay per stream per hour & data in/out per GB

        * Kinesis Data Firehose
            * load data streams into AWS data stores
            * knows how to write data to destinations
                * S3, RedShift, ElasticSearch
                * 3rd party apps (SPlunk, mongoDB, etc)
            * near real time
            * supports many data formats, conversions
            * can send backups to S3
        * Kinesis Data Analytics
            * analyze data streams with SQL or Apache Flink
        * Kinesis Video Streams
            * capture, process, store video streams
        * Kinesis Security
            * Controll access using IAM
            * in flight using HTTPS, at rest with KMS
            * VPC endpoints available for Kinesis
            * monitor API using CLoudTrail
    * Ordering data into Kinesis
        * send using a partition key with a value of importance
        * same parition key will be in same shard
    * Ordering data in SQS
        * standard queue, no ordering
        * for FIFO queue, with no group ID, messages consumed in order
        * scale number of consumers, use group ID
    * SQS vs SNS vs Kinesis
        * SQS
            * conumer pulls data
            * data is deleted after being consumed
            * can have as many workers as needed
            * idvidual message delay capability
            * fully managed
        * SNS
            * publish/ subscribe architecture
            * data is not persisted
            * fully managed
        * Kinesis
            * standard - pull data
            * enahanced fan out - push data
            * possibility to replay data
            * meant for real time big data, analytics, ETL
            * ordering at the shard level
            * data expires afte X days
            * provisioned mode or on demand capacity mode
    * Amazon MQ
        * managed message broker service for RabbitMQ/ActiveMQ
        * doesn't scale as much as SQS/SNS
        * runs on servers, can run in multi-AZ with failover
        * has both queue feture and topic features
# Section 19: Containers on AWS (ECS, Fargate, ECR & EKS)
    * Docker
        * apps packaged in containers that can be run on any OS
        * apps run the same, regardless of where they're run
        * good for microservices architecture
        * where are images stored
            * Docker Hub
            * Amazon ECR (Elastic Container Registry)
                * private container repository
    * Amazon ECS (Elestic Container Service)
        * launch docker containers on AWS -> launch ECS tasks on clusters
        * EC2 launch type: must provision & maintain infra (EC2s)
            * each EC2 instance must run the ECS Agent
        * ECS - Fargate Luanch type
            * do not provision the infrastructure
            * serverless, to scale increase number of tasks
        * IAM roles for ECS
            * EC2 instance profile (EC2 Launch type only)
                * used by ECS agent
            * ECS Task Role
                * allows each task to have a specific role
                * use different roles for different ECS services you run
            * Load balancer integrations
                * ALBs supported for ECS
                * Network load balancer recommend for high thorughput
                * ELB supported but not recommended (no Fargate)
            * ECS Data Volumes (EFS)
                * mount EFS file systems onto ECS tasks
                * works for both EC2 and Fargate launch types
                * Fargate + EFS = serverless
                * S3 cannot be mounted on a file system
        * ECS Service Auto Scaling
            * auto increase/decrease disired # of ECS tasks
            * auto scaling uses
                * Average CPU utlization
                * Average Memory Utilization - scale on RAM
                * Request Count Per Target - metric coming from ALB
            * EC2 launch type - auto scaling EC2 instances
                * auto scaling group scaling
                    * scale ASG based on CPU utlization
                    * add EC2s over time
                * ECS Cluster Capacity Provider
                    * used to auto provision and scale infra
                    * capacity provider paired with ASG
                    * Add EC2s when missing capacity (CPU, Ram)
        * ECS tasks invoked by event bridge
            * event bridge can run ecs tasks such as S3 object upload
        * ECS tasks invoked by event bridge schedule
            * run tasks at certain interval for batch processing
    * Amazon ECR (Elastic Container Registry)
        * store and manage Docker images on AWS
        * private and public repositories
        * fully integrated with ECS, backed by Amazon S3
        * access is controlled through IAM
            * permission errors -> check IAM policies
        * supports image vulnerability, scanning, versioning, etc
    * Amazon EKS (Elastic Kubernetes Service)
        * launch managed Kubernetes cluster on AWS
        * Kubernetes
            * alternative to ECS, open source container manangement
        * EKS supports EC2 or Fargate
        * pods - group of EKS containers
        * Node types
            * managed node groups
                * nodes are part of an ASG managed by EKS
                * supports on-demand or Spot instances
            * self-managed nodes
                * nodes created by user
                * can use prebuilt AMI (EKS optimized)
            * Fargate
                * no maintenance required, no nodes managed
        * EKS - Data volumes
            * need to specify StorageClass manifest
            * leverage a Container Storage Interface (CSI) driver
            * supports
                * Amazon EBS
                * Amazon EFS (works with fargate)
                * FSx for Lustre
                * FSx for NetApp ONTAP
    * AWS App Runner
        * fully managed service to easily deploy web apps and APIs
        * start with source courd or container image
        * auto builds and deploy the web app
        * VPC access support
        * use cases: microservices, rapid production deployments
# Section 20: Serverless services Overview
    * serveless is developers don't manage servers anymore
    * Lambda
        * virtual functions - no servers to manage
        * limited by time - short executions
        * runs on demand
        * benefits
            * easy pricing
                * pay per request and compute time
            * integrated with AWS services
            * integration with many programming languages
            * increasing RAM will also improve CPU and network
        * language support
            * NOde.js, Python, Java, C#, Golang, Ruby
            * Lambda Container Image
                * must implement Lambda Runtime API
                * ECS/ Fargate preferred for most docker images
        * Lambda integrations
            * API Gateway
            * Kinesis
            * DynamoDB
            * S3
            * CloudFront
            * CloudWatch Events/Event Bridge
        * Lambda Limits (per region)
            * Execution
                * Memory: 128 MB - 10 GB (1 MB increment)
                * max execution time: 900 sec (15 min)
                * 4 KB env variables
                * disk copacity in "function container"  10 GB
                * concurrency executions 1000
            * Deployment
                * lambda function size: 50 MB
                * size of uncrompressed deployment: 250 MB
                * can use /tmp directory to load other files at start
                * size of env variable 4 KB
        * Customization at the edge
            * Edge function
                * code that you write and attach to Cloiudfront
            * Two types
                * CLoudFront Functions
                    * written in JavaScript
                    * used to change viewer requests and responses
                    * native 
                * Lambda@Edge
                    * written in NodeJS or Python
                    * author your functions in one region, gets replicated
                * use cases
                    * website security & privacy
                    * dynamic web app at Edge
                    * Search Engine Optimization
        * Lambda in VPC
            * lambda function launched outside VPC
            * can't access resources in your VPC
            * how to luanch Lambda in VPC
                * define VPC ID, subnets, and Security Gorups
                * lambda will create ENI in your subnets
            * Lambda with RDS Proxy
                * use proxy so lambda functions do not directly connect to database under high load
                * pools connections and shares them w/ functions
                * lambda function must be deployed in your VPC
    * Amazon DynamoDB
        * fully managed, replication across multiple AZs
        * NoSQL DB with tranasaction support
        * scales to massive workloads
        * fast and consistent in performance
        * no maintenance or patching
        * standard & infrequent access table classes
        * Dynamo Basics
            * made of tables
            * each table with primary key
            * can add attributes (columns) over time
        * read/write capacity modes
            * provisioned mode (default)
                * specify # of read/writes per second
                * pay for provisioned Read Capacity Units (RCU)
                * pay for provisioned Write Capacity Unit (WCU)
                * possibility to auto scale RCU & WCU
            * on demand mode
                * read/writes auto scale up/down with owrkloads
                * more expensive, pay for what you use 
                * greate for upredictible work, steep sudden spikes
        * Advanced Features
            * DynamoDB Accelerator (DAX)
                * fully managed memory cache
                * microseconds latency for cached data
            * Stream Processing
                * ordered stream of items in a table
                * react to changes in real-time (welcome email)
                * DynamoDB Streams
                    * 24 hours retention
                    * limited # of consumers
            * Global Tables
                * multi region table with low latency 
                * apps read write in any region
                * must have dynamoDB streams enabled
            * Time To Live
                * auto delete items after an expiry timestamp
                * reduce stored data by keeping only current items
                * web session handling
            * backups for disaster recover
                * pont in time recovery (PITR)
                    * enabled for last 35 days
                    * recovery process creates new table
                * on-demand backups
                    * full backups for long term retention
                    * can be configured in AWS Backup (cross region)
            * integration with S3
                * export to S3 (must enable PITR)
                    * works for any point of time in last 35 days
                    * doesn't affect read capicity of table
                * import to S3
                    * doesn't consume write capacity
                    * creates new table
    * API Gateway
        * build serverless API
        * handles security
        * integrates with
            * lambda function
                * espose rest API backed by AWS lambda
            * HTTP
        * Endpoint Types
            * Edge-Optimized (default): for global clients
                * requests routed through cloudfront edge locations
                * gateway still lives in only one region
            * Regional
                * for clients within the same region
                * could manually combine with Cloudfront
            * Private
                * Can only be accessed from your VPC using VPC endpoint (ENI)
                * use resoruce policy for access
        * Security
            * user auth through
                * IAM Roles
                * Cognito (external users)
                * Custom Authorizer (lambda functions)
            * Custom domain name HTTPS through AWS Cert Manager (ACM)
                * edge optimized cert must be in us-east 1
                * must setup CNAME or A-alias record in route 53
    * Step Functions
        * build serverless visual workflow to orchestrate lambda func
# Section 21: Serverless Architecures
    * scnenario hands on stuff
# Section 22: Databases
    * Database Types
        * RDBMS: RDS, Aurora
            * great for joins
        * NoSQL database:  DynamoDB, Elasticache, 
            * no joins, no SQL
        * Object store: S3
        *  Data Warehouse: Redshift, Athena, EMR
        * Search: OpenSearch
            * free text, unstructured searches
        * Graphs: Amazon neptune
        * ledger: amazon quantum ledger
    * RDS
        * managed postgres/ MySQL/ Oracle/ Sql Server
        * provisioned RDS instance size and EBS volume type
        * Auto scaling capability for storage
        * supports read replicas and multi AZ
        * security thorugh IAM, SG, KMS, SSL in transit
        * managed and scheduled maintenance
        * use case - store relational databases
    * Aurora
        * compatible for Postgres/MySQL
        * storage and compute are seperate
        * self healing, auto scaling 
        * aurora serverless - unpredictable/intermittent workloads
        * aurora multi-master - continuous writes failover
    * Elasticache
        * managed redis/ memcached cache service
        * must provision an EC2 instance type
        * require code changes
        * use cases
            * frequent reads, less writes, store session data
    * DynamoDB
        * proprietary tech, serverless noSQL database
        * provisioned capacity with opt auto-scaling or on-demand capacity
        * can replace elasticache for storing session data
        * DAX cluster for reach cache
        * security, atuh done through IAM
        * use cases
            *  serverless app development (small docs 100s KB)
            * distributed serverless cache
    * S3
        * key/value store for objects
        * great for bigger objects
    * DocumentDB
        * cloud native version of MongoDB
        * noSQL database
        * store, query, index JSON data
    * Neptune
        * fully managed graph database
    * Keyspaces (for Apache Cassandra)
        * noSQL distributed database
        * servless
        * auto scale tables up/down
        * use cases
            * IoT devices, time series data
    * Amazon QLDB
        * Quantum Ledger Database
        * ledger - book recording financial transactions
        * immutable systems
        * no decentralization, in accordance with regulation rules
    * Amazon Timestream
        * time series database
        * use cases
            * IOT
            * operational apps
            * real time analytics
# Section 23: Data & Analytics
    * Athena
        * serverless query service to analyze S3 data
        * uses SQL 
        * don't have to move data
        * commonly used with Quicksight for dashboards
        * athena performance improvement
            * use columnar data for cost-savings
                * Apache Parquet or ORC is recommended
                * use Glue to convert data
            * compress data for smaller retrievals
            * partition data sets in S3 for easy querying
            * use larger files to minimize overhead
        * federated query
            * run sql queries across several diff sources
            * uses data source connectors (lambda function)
    * Redshift
        * based on Postgres
        * used for OLAP (online analytical processing)
        * 10x better performance than other data warehouses
        * columnar storage of data
        * faster queries than Athena, because of indexes
        * better for more complicated queries
        * only available in one AZ, use snapshots to transfer
        * can auto copy snapshots to another region
        * Reshift Spectrum
            * query data that is already in S3
    * OpenSearch (Amazon ElasticSearch)
        * dynamoDB queries only  by primary key or indexes
        * openSearch can search any field, even partial matches
        * used in conjunction with other databases
        * does NOT support SQL
    * Amazon EMR (Elastic MapReduce)
        * helps create Hadoop clusters (big data)
        * EMR comes bundled with Apache Spark, HBase, etc
        * auto scaling and integrated with Spot instances
        * use cases: data processing, machine learning, big data
        * Node types & purchasing
            * master node: manages cluster - long running
            * core node: run tasks and store data - long running
            * task node (optional): just run tasks - usually spot
    * Amazon QuickSight
        * serverless Business Intelligence service for dashboards
        * integrates with RDS, Aurora, etc
        * in-memory computation using SPICE engine if data imported
        * Dashboard & analysis
            * define users (standard version) & groups (enterprise)
            * users & groups only exist within Quicksight
    * Glue
        * managed extract, transform, and Load (ETL) service
        * useful to prepare and transform data for analytics
        * converting data into Parquet format
            * on S3 put event notification, use lambda to call glue
        * Glue Data Catalog
            * glue data crawlers writes metadata of tables
        * glue job bookmarks - prevent re-processing old data
        * glue elastic views
            * combine and replicate data across mult sources
            * no custome code, glue monitors changes
        * glue databrew: clean and normalizes data
        * glue Studio: new GUI to create jobs in Glue
        * Glue Streaming ETL (build on Spark)
    * AWS Lake Formation
        * data lake - central place to have all your data 
        * fully managed service to setup data lake
        * automates many complex manual steps
        * have blueprints for many different data sources
        * lake formation centralized permissions example
            access control w/ column & row level security
    * Kinesis Data Analytics
        * Kinesis Data Analytics for SQL applications
            * can come from Kinesis data streams or firehose
            * output can be sent to data stream or firehose
                * firehose can be used to send to S3
                * streams can be sent to Lambda or other apps
        * Kinesis Data Analytics for Apache Flink
            * use Fink (Java, Scala, or sql) to process stream data
    * Amazon managed streaming for Apapche Kafka (MSK)
        * alternative to Amazon Kinesis
        * fully managed Kafka on AWS
        * Data is stored on EBS volumes for as long as you want
        * MSK serverless
            * run kafka on MSK without dealing with capacity
        * Kinesis vs MSK
            * kinesis has 1 mb limit, MSK can go higher
            * MSK can only add partitions not delete
        * MSK Consumers
            * Flink , Glue (spark powered)
            * lambda
            * custom consumers (write code)
    * Big Data Ingestion Pipeline
        * IoT Devices (IoT Core service)
            * IoT -> kinesis data stream -> S3 bucket
# Section 24: Machine Learning
    * Rekognition
        * find objects, people, text, images using ML
        * facial analysis and facial search
        * create database of familar faces
        * content moderation
            * detect innapropriate content
            * used in social media, advertising for safe user exp
            * set minimum confidence threshold for items to flag
            * flag Amazon Augmented AI (A2I) for manual review
    * Transcribe
        * automatically convert speech to text
        * uses Automatic Speech Recognition (ASR)
        * auto remove PII (personally identifiable info) using redaction
        * support auto language identification for multi-language
    * Polly
        * turn text into lifelike speech using deep learning
        * allows you to create applications that talk
        * customize pronunciation of words with lexicons
        * Speech ßynthesis markup language (SSML)
            * used for more customization
                * phonetic pronunciation, breathing sounds, whisper
    * Translate
        * natural and accurate language translation
        * allows you to localize content for webapps
    * Lex + Connect
        * Lex - powers Alexa devices
            * has ASR
            * helps build chatbots, call center bots
        * Amazon Connect
            * recieve calls, cloud-based virtual contact center
            * no upfront payments, 80% cheaper
    * Comprehend
        * Natural Language Processing (NLP)
        * fully managed and serverless
    * Comprehend Medical
        * detects and returns useful info in unstructured text
    * SageMaker Overview
        * fully managed service to build ML models
        * typically difficult to do all processes in one place
        * helps with labeling, training, and tuning of ML model
    * Forecast
        * fully managed ML service for highly accurate forecasts
        * reduce forecasting time from months to hours
    * Kendra
        * fully managed, document search service using ML
        * extract answers from within a document
        * will learn from interactions/feedback
    * Amazon Personalize
        * fully managed ML service with real time recommendations
        * same tech used by Amazon.com
        * don't need to deploy ML solutions
    * Amazon Textract
        * extract text, handwriting, data from scanned docs
# Section 25: AWS Monitoring & Audit (Cloudwatch, Cloudtrail)
    * CloudWatch Metrics
        * provides metrics for every service in AWS
        * metric - variable to monitor (CPU, networking, etc)
        * Dimension is an atrribute of a metric (< 10 per metric)
        * metric streams
            * stream with real-time delivery (Kinesis firehose)
        * cloudwatch logs
            * log groups - name, usually represents app
            * log stream: instance within app
            * log aggregation - multi account and multi region
        * cloudwatch agent
            * need agent to run on EC2 for log files'
            * Logs Agent (old version)
            * Unified Agent (new version)
                * gives metrics and logs
        * cloudwatch alarms
            * three states
                * OK, INSUFFICIENT_DATA, ALARM
            * alarm targets
                * stop, terminate, recover EC2 instance
            * composite alarms - combining other alarms
            * alarms can be created on logs metrics filters
            * can tests alarms using CLI
    * Amazon EventBridge (formerly Cloudwatch Events)
        * sechdule: cron jobs
        * Event pattern: event rules react to service doing something
        * trigger lambda functions
        * EventBridge Rules
            * can be used to trigger lambda
            * can replay archive events sent to event bus
        * schema registry
            * eventbridge can analyze events in your bus and infer the schema
            * schema registry allows you to generate code from app
        * cloudwatch insights
            * container insights 
                * collect aggregate, summarize metrics/logs
                * available from ECS, EKS, K8s, Fargate
            * lambda insights
                * monitor and troubleshoot serverless Lambda
            * contributor insights
                * see top contributor(s) data
                * ex heaviest network users
            * application insights
                * show automated dashboards for apps
                * powered by SageMaker
    * CloudTrail
        * provides governance, complicance, and audit AWS account
        * enabled by default
        * cloudTrail events
            * management events
                * ops that are performed on resources in AWS 
                * enabled to log by default
                * can seperate read events from write events
            * Data events
                * not logged by default
                * S# object level activity (GETObj, PUTObj)
                * Lambda function execution acivity
            * CloudTrail Insight events
                * enable insights to detect unusual activity
                * continuously analyzes write events
            * EventBridge - intercept API calls
                * CloudTrail logs API calls
                * can look for "delete" API to create rule
    * AWS Config
        * helps with auditing and record complience of resources
        * helps record configurations and changes over time
        * examples
            * is there unrestricted SSH access to my SG?
            * do my buckets have any public access?
        * per-region service
        * config rules
            * AWS managed
            * custom config rules (defined in Lambda)
            * does not prevent actions from happening
    * cloudwatch vs cloudtrail vs Config
        * CloudWatch
            * performance monitoring
        *CloudTrail
            * record API calls within your Account
        * Config
            * record configuration changes
# Section 26: IAM Advanced
    * Organizations
        * global service, allows to manage multiple AWS accounts
        * main account management account, other member accounts
        * consolidated billing across all accounts
        * share reserved instances across accounts
        * organization units (OU)
            * groupings of accounts
        * Service Control Policies (SCP)
            * policies applied to OU or accounts to restrict users
            * do not apply to management account
            * must have an explicit allow
            * any deny will overide explicit allows
            * SCP examples
                * blocklist and Allowlist strategies
    * IAM conditions
        * aws:SourceIP
            * retrict the client IP FROM which API calls are made
        * ec2:ResourceTag
            * restrict based on tags
        * S3:ListBucket -> bucket level permission
        * aws:PrincialOrgID
            * can be used in resource policies to resstricts access to accounts that are member of an AWS org
    * IAM Roles vs Resource based policies
        * cross account
            * attach a resource-based policy to a resource
            * OR use a role as a proxy
        * when you assume a role, you give up your original permissions and take premissions assigned to the role
        * when using resource -based policy, the principal doesn't have to give up permissions
        * EventBridge Security
            * when a rule runs it needs permissions on the target
            * resource based policy: Lambda, SNS, SQS, Cloudwatch
            * IAM role: kinesis stream, systems manaager, ECS task
    * IAM Permission boundaries
        * supported for users and roles (BUT not groups)
        * set the maximum permissions an IAM entity can get
        * useful to restrict one specific user
    * Amazon Cognito
        * gives users identity for outside AWS
        * Cognito User Pools
            * sign in functionality
            * creates serverless db for users
            * simple login (basic auth)
            * integrates with API gateway and ALB
        * Cognito Identiy Pools (Federated Identity)
            * provide AWS credentials to users 
            * integrate with Cognito User Pools as identity provider
            * users get temporary AWS credentials
        * Cognito vs IAM
            * use cognito for multiple users mobile users, SAML
    * IAM Identity Center
        * successor to AWS SSO (Single sign on)
        * can sign into 
            * AWS accounts in AWS org
            * business cloud apps
            * SAML2.0 enabled apps
            * EC2 Windows Instances 
        * identity providers
            * Built in identity store in IAM Identity Center
            * 3rd party Active Directory, Okta, etc
        * permissions set in Identity Center
    * AWS Directory Services
        * Microsoft Active Directory
            * centralized security management
            * database of objects, user accounts etc
            * objects are organized in trees
        * AWS Directory services types
            * AWS Managed Microsoft AD
                * create your own AD in AWS, manage users locally
                * establish trust connections with on prem AD
            * AD Connector
                * Redirects to on-prem AD
                * users are managed on prem
            * Simple AD
                * AD compatible managed directory on AWS
                * cannot be joined with on-prem AD
        * AD setup
            * connect to AWS Managed Microsoft AD
            * Connect to a self-managed Directory
                * create two way trust relaitonship using AWS AD
    * AWS Control Tower
        * easy way to set up secure and compliant multi-account AWS environment
        * control tower uses AWS Orgs to create accounts
        * Control Tower Guardrails
            * preventive guardrails
                * prevents users from doing something
                * uses Service Control Protocols (SCP) to restrict regions
            * Detective Guardrail - uses AWS config
                * monitor user actions

# Section 27: AWS Security & Encryption
    * Encryption
        * Encryption in flight (SSL) - encryption while transporting data
            * SSL certificates help with enryption (HTTPS)
            * encryption in flight ensures no "man in middle" attack
        * server side encryption at rest
            * data is encrypted after being recieved by the server
            * data is decrypted before being sent to user
            * uses data key stroed in Key Management Service (KMS)
        * client side encryption
            * data encrypted by client, server never decrypts data
            * could leverage envelope encryption
    * KMS
        * anytime you hear "encryption" for AWS, most likely KMS
        * never store secrets in plaintext, especially in your code
            * call KMS API to get creds files
        * KMS Key types
            * Symmetric key
                * single key to encrypt and decrypt data
                * AWS services uses symmetric keys
                * you never get access to KMS Key
            * Asymmetric key
                * public (encrypt) and private (decrypt) pair
                * public key is downloadable
                * use case encrypt outside of AWS
        * KMS keys
            * AWS managed key: free
            * customer managed key (CMK): $1/month
            * customer managed keys imported: $1/month
            * also pay for API call to KMS
            * Automatic key rotation
                * AWS managed: every 1 year
                * customer managed key: 1 year
        * KMS Key Policies
            * similar to S3 bucket policies
            * key policies needed to access KMS keys
            * default KMS key policy
                * complete access to key to entire AWS account
        * KMS Multi-region keys
            * identical KMS keys in different AWS regions
            * KMS multi-region is not global
            * each multi region key is managed independently
            * can be used for DynamoDB global tables & multi region key client-side encryption
            * can be used with aurora DB to encrypt a specific column (ex SSN column in financial table)
        * S3 replication
            * unencrypted and server side encrypted S3 keys are auto replicated
            * Client encrypted keys are never replicated
            * SSE-KMS need to enable option to replicate
        * AMI Sharing process encrypted via KMS
            * must modify attribute launch permission which links to AWS account
            * must share KMS keys through key polict
            * create IAM role/user to have access to KMS key
    * SSM Paramter Store
        * secure storage for config and secrets
        * seamless encryption using KMS
        * security through IAM
        * can have paramater polices
            * assign TTL to paramater to force updating
            * can assign multiple policies at a time
    * AWS Secrets Manager
        * capabality to force rotation of secrets every X days
        * auto generation of secrets on rotation (uses Lambda)
        * Mult-region secrets with replica secrets
    * AWS Certificate Manager (ACM)
        * easily manage TLS certificates
        * provides in-flight encryption for websites
        * free of charge for public TLS certificates
        * integrates with ELBs, cloudfront, or API gateway
        * cannot use ACM with EC2
        * requesting public certificates
            * list domain names to be included in the cert
            * select validation method (DNS or email)
                * DNS validation is preferred
            * need to get verified
            * public cert will be enrolled for auto renewal
        * importing public certs
            * no auto renewal, muust import new cert
            * ACM sends daily expiration events
        * ACM w/ API Gateway
            * create custom domain name in API gateway
            * edge-optimized (default)
                * request routed through cloudFront
                * TLC certs must be in same region as cloud-front, us-east-1
            * regional
                * TLS cert must be imported to gateway in same region
    * AWS WAF (Web App Firewall)
        * protect web apps from Layer 7 common web exploits
        * deploy on
            * ALB, API Gateway, CloudFront, AppSync GraphQL API, Cognito USer Pool
        * DOES NOT SUPPORT network load balancer
        * Define Web ACL (Access Control List) rules
        * protects against SQL injection and Cross-site scripting
        * adds DDoS protection
        * use case: fixed IP with load balancer using global accelerator 
    * AWS Shield
        * protects against DDoS attacks
        * Shield Standard
            * free service available to all AWS users
        * Shield Advanced
            * pay money
            * protect against more sophisticated attacks on
                * EC2, ELB, Cloudfront, AWS Global Accelerator, Route 53
                * protect against higher fess during spikes
                * auto deploy WAF rules to mitigate attacks
    * Firewall Manager
        * manage rules in all accounts of an AWS org
        * security policy: common set of security rules
            * WAF Rules
            * AWS Shield Advanced
            * Security Groups
            * AWS network Firewall
            * policies are applied at region level
        * rules applied to new resources when created
    * AWS best practices for DDoS mitigation
        * BP1 - cloudfront
            * webapp delivery at edge
            * protect from DDoS using Shield
        * BP1 - global accelerator
            * integrates with Shield for DDoS protection
        *BP3 - Route 53
            * DDoS included
        * infrastructure layer defense (BP1, BP3, BP6)
            * Protect EC2 against high traffic
            * amazon EC2 with ASG (BP7)
                * helps scale in case of sudden traffic surges
            * Elastic Load Balancing (BP6)
                & scales with traffic increase and distributes
        * app layer defense
            * detect and filter malicious web requests
                * use Cloudfront, WAF to filter requests
                * shield advanced can auto create WAF rules
        * Attack surface reduction
            * obfuscating AWS resources (hide backend resources)
            * use security groups and netowrk access control lists
            * protect API endpoints
    * Amazon GuardDuty
        * inteligent threat discovery to protect your AWA account
        * uses ML to find anomalies
        * input includes cloudtrail logs, VPC flow logs, DNS logs
        * can protect against cryptocurrency attacks
    * Amazon Inspector
        * automated vulnerability detection for
            * EC2 instances
                * leverage AWS system manager agent
            * Container Images pushed to Amazon ECR
            * Lambda Functions
                * look for dependency problems
        * only for running EC2s, container images & lambda
    * Amazon Macie
        * data security and privacy service
        * alerts for personally identifiable information (PII)
# Section 28: VPC
    * CIDR - classless inter-domain routing
        * method for allocating IP addresses
        * consists of two components
            * Base IP (XX.XX.XX.XX)
            * Subnet Mask
                * defines how many bits can change in the IP
                * /0, /24, /32 OR 255.0.0.0 format
                * the less the "/" number the more IPS allowed
                    * ex 192.168.0.0/32 allows one IP (192.168.0.0)
                    * 192.168.0.0/31 allows 2 IP (powers of 2 2^1)
        * public vs private IP
            * private IP can only allow certain values
                * 10.0.0.0 - 10.255.255.255 (10.0.0.0/8) big networks
                * 172.16.0.0-172.31.255.255 (172.16.0.0/12) AWS default VPC range
                * 192.168.0.0/16 (home network)
            * all the rest of the IP addresses are public
    * Default VPC overview
        * all new accounts have a default VPC
        * default VPC has internet connectivity and public IP addr
    * Custom VPC Overview (Virtual Private cloud)
        * can have multiple VPCs in an AWS region (soft max of 5)
        * max CIDR per VPC is 5, for each CIDR
            * min size /28 (16 IP addresses)
            * max size is /16 (65,536 addresses)
            * only private IPv4 ranges are allowed
        * make sure VPC CIDR should NOT overlap with your other networks
    * Subnets
        * both public or private
        * subrange of IPv4 addresses
        * AWS reserves 5 IP addresses (first 4 & last 1)
        * 10.0.0.0/24 CIDR example reserved addresses
            * 10.0.0.0 - Network address
            * 10.0.0.1 - reserved by VPC router
            * 10.0.0.2 - reserved for mmapping to AWS DNS
            * 10.0.0.3 - reserved for future use
            * 10.0.0.255 - network broadcast address
        * exam tip, if you need 29 IP addresses don't use /27
    * Internet Gateway (IGW)
        * allows resources in a VPC to connect to public internet
        * must be created seperately from a VPC
        * one to one VPC <-> IGW connection
        * route tables need editing to enable internet connectivity
    * Bastion Hosts
        * users want to access instance in private subnet
        * host in public subnet can ssh to private instance
        * laison between user and private instances
        * bastion host security group must allow inbound from internet on port 22 from restricted CIDR (public CIDR)
        * security group of private EC2 must allow private IP of Bastion host
    * NAT Instances (outdated but still on exam)
        * Network Address Translation
        * allows EC2s in private subnets to connect to internet
        * must be launched in a public subnet
        * must disable EC2 setting: Source/Destination check
        * must have elastic IP attached to it
        * route tables must b configured to route traffic from private subnets to NAT instance
    * NAT Gateways
        * AWS managed NAT instances
        * pay per hour of usage and banwidth
        * created in specific AZ, uses an Elastic IP
        * Cant be nused by EC2 in the same subnet
        * requires an IGW
        * gateway is resilient within a single AZ
        * need multiple NAT gateways in multiple AZs for fault tolerance
    * NACL & Security Groups
        * Network Access Control List
            * stateless (all traffic evaluated)
            * rules that are processed at subnet level
            * controls traffic to/from subnets
            * NACL Rules
                * rules have number, higher precedence with lower num
                * first rule match will drive the decision
                * last rule is an asterisk to deny request w/ no match
            * gets processed before security groups
            * Default NACL
                * accepts everything indbound/outbound with the subnet it's associated with
        * Security Group
            * stateful (once allowed in, auto allowed out)
            * operates at instance level
            * supports allow rules only
        * Ephemeral Ports
            * clients connect to defined port, expect response from ephemeral port
            * the port that opens so server can send resposne to client
            * only lives for life of connection
        * Ephemeral Ports with NACL
            * must set up ephemeral port range to allow connections
    * VPC Peering
        * privately connect two VPCs using AWS network
        * make them behave as one connected network
        * VPC Peering connection is NOT transitive
        * must route tables in each VPC subnet to allow peering
    * VPC endpoints
        * use to connect instances to AWS services privately
        * less cost than going through NAT Gateway to public service
        * redundant and scale horizontally
        * remove need of IGW, NATGW,... to access AWS services
        * two types
            * Interface Endpoints (powered by PrivateLink)
                * Provisions an ENI as an entry point (use SG)
                * supports most AWS services
                * cost per hour and per GB of data used
            * Gateway Endpoint (preferred for S3/DynamoDB)
                * provisions gateway and use as a target in route table (no security groups)
                * supports both S3 and DynamoDB
                * Free
    * VPC Flow Logs
        * capture info about IP traffic going into interfaces
            * VPC flow logs
            * subnet Flow Logs
            * Elastic Network Interface (ENI) flow logs
        * helps moitor & troubleshoot connectivity logs
        * how to troubleshoot - look at action field
    * Site to Site VPN
        * encrypted public connection to on prem data centers
        * virtual private gateway (VGW)
            * VPN concentrator on the AWS side of connection
        * Customer Gateway (CGW)
            * software app to connect to VPC
            * what IP to use?
                * public address for CGW device
                * use public IP address of NAT device if private
        * must enable Route Propagation for VGW
        * enable ICMP protocol on inbound rules for SG to ping EC2s
        * AWS VPN CloudHub
            * provide secure comms between multiple sites/VPN connection
    * Direct Connect (DX) & Direct Connect Gateway
        * provices dedicated private connection to VPC
        * needs Virtual Private Gateway to VPC
        * access public and private resources on same connection
        * use cases
            * increased badnwith throughput
            * more consistent network experience
            * supports hybrid environments
        * connection types
            * dedicated connections (1, 10, or 100 Gbps)
                & physical ethernet prot dedicated to customer
            * Hosted Connection (50Mbps, 500Mbps, 10Gbps)
                * capacity can be added or removed on demand
            * lead times are often longer than 1 month to establish connection
        * Encryption
            * data in transit is not encrypted, but not private
            * Direct Connect + VPN provides encryption
        * Resiliency
            * for critical workloads
                * mult direct connects w/ multi DX locations
            * max resiliency for critical workloads
                * mult locations with multiple connections 
    * Site-to-Site VPN connection as backup
        * if DX fails, set up site-to-site VPN connection
    * Transit Gateway
        * transitie peering between thousands of VPC, on prem, hub and spoke connection
        * regional resource, can work cross region
        * create route tables for transit gateway connections
        * works with DX gateway, VPN connections
        * supports IP Multicast
        * Site-to-Site VPN ECMP (Equal-cost multi path) routing
            * forward a packet over multiple best path
        * can share DX connection with multiple AWS accounts via transit gateway
    * VPC Traffic Mirroring
        * allows capture and inspect network traffic on VPC
        * route traffic to security services
        * capture traffic from (source) or to (target)
    * IPv6
        * designed to succeed IPv4
        * format is X.X.X.X.X.X.X.X where X is hex range 
            ( 0000 -> ffff)
        * can use IPv6 in VPCs for public addresses
    * Egress Only Internet Gateway
        * NAT Gateway for IPv6 traffic only
        * must update route tables
    * Networking Costs in AWS
        * same AZ
            * any incoming traffic to EC2 is free
            * private IP comm between EC2s is free
        * diff AZ
            * EC2 -> EC2 not free
            * private IP communication is cheaper
        * use PRIVATE IP when possible
        * minimizing egress traffic network cost
            * egress - outbound traffic
            * ingress traffic - inbound traffic (typically free)
        * S3 data transfer pricing
            * S3 ingress - free
            * S3 to Internet: 9 cents/ GB
            * S3 Transfer Acceleration costs extra money
            * S3 to CloudFront: free
            * Cloudfront to Internet: 8.5 cents / GB
    * AWS Netowrk Firewall
        * protects entire VPC
        * uses AWS Gateway Load Balancer
        * supports 1000s of rules
        * traffic filtering
# Section 29: Disaster Recovery & Mitigations
    * Disaster Recovery Overview
        * types of disaster recovery
            * On prem -> On Prem, traditional & very expensive
            * on prem -> AWS cloud, hybrid recovery
            * AWS region A -> Region B
        * RPO (Recovery Point Objective)
            * how often do you run backups
            * time between RPO and disaster is data loss
        * RTO (Recovery Time Objective)
            * time between disaster and recovery
    * Disaster recovery strategies
        * Backup & restore
            * high RPO
            * full backups at certain interval
        * Pilot light
            * small version with critical systems is always running in the cloud
            * lower RPO and RTO than backup & restore
        * Warm standby
            * full backup system is up and running, but at min size
            * upon disaster, scale up to production load
        * hot site/ multi-site approach
            * very low RTO  but very expensive
            * full production scale on prem and AWS
    * DMS (Database Migration Service)
        * quickly migrate databases to AWS, self healing
        * source DB remains available during migration
        * must create EC2 instance to perform the replication
        * Schema Conversion Tool (SCT)
            * convert schema from one engine to another
    * RDS & Aurora MySQL Migrations
        * RDS MySQL to Aurora
            * Option 1: DB snapshots from RDS mySQL restored as Aurora
            * Option 2: Aurora Read replica from RDS MySQL
        * External MySQL to Aurora MySQL
            * Opt 1: Use Percona XtraBackup to create file backup in S3
                * create Aurora DB from S3
            * Opt 2: Use mysqldump utility into Aurora (slower than S3)
        * Use DMS if both databases are up and running
    * On-Prem strategy with AWS
        * ability to download Amazon Linux 2 AMI as a VM
        * VM Import/Export
        * AWS app Discovery Service
        * AWS Databse Migration Service (DMS)
        * AWS Server Migration Service
    * AWS Backup
        * fully manged service to manage and automate backups
        * no need to create custom scripts
        * EC2, S3, RDS, EFS, Storage Gateway supported
        * supports Point in Time Recovery
        * crearte backup policies known as Backup Plans
        * Vault Lock
            * write once Read Many policy
            * backups can't be deleted
    * AWS App Discorvery Service
        * plan migration projects by scanning servers
        * Agentless Discovery (AWA Agentless Discovery Connector)
            * VM inventory, config, and performance
        * Agent-based Discovery 
            * system config & performance, running processes, and deatails of network connections between systems
        * data can be viewed in AWS Migration Hub
    * AWS Application Migration Service (MGN)
        * lift and shift solution which simplifiy migrating to AWS
        * converts physical, virtual, and cloud-based server to run natively on AWS
    * Transferring large amount of data into AWS
        * ex transfer 200 TB of data, 100 Mbps internet
    * VMware Cloud on AWS
        * VMware used for managing on-prm data center
        * use AWS while also using VMware cloud
# Section 29: More Solution Architectures
    * Event Processing in AWS
        * SQS + Lambda
            * Lambda will do try,try poll
            * dead letter queue (DLQ) to stop infinite loop
        * SQS FIFO + Lambda
            * try, retry and blocking
            * use dead letter queue to stop infinite loop
        * SNS + Lambda
            * asyncronous, lambda retries internally
            * send lambda retries to SQS DLQ
        * Fan Out Pattern: deliver to multiple SQS queues
            * SQS queues subscribe to central SNS subscriber
        * S3 Event notifications
            * object create, remove, restore, etc
            * use case - generate thumbails uploaded to S3
            * can send to SNS, SQS, Lambda
            * can also send to Amazon Event Bridge
        * EventBridge - intercept API calls (ex dydamoDB delete)
        * API Gateway -> kinesis data streams -> firehose -> S3
    * Caching Strategies in AWS
        * caching through cloudfront edge
            * run risk of outdated content, use TTL to force refresh
        * API Gateway caching
            * regional caching
        * Redis, Memcached, Dax
    * Blocking an IP address
        * NACL deny rules
        * SG whitelist
        * ALB connection termination w/ Security group
        * Network Load Balancer does not have security (passthrough)
        * WAF IP address filtering on ALB
        * WAF on Cloudfront, CloudFront geo restriction
    * High Performance Computing (HPC) on AWS
        * Data management & transfer
            * Direct Connect, good for moving data over private net
            * Snowball & Snowmobile
                * move PB of data at once
            * AWS DataSync
                * move large amount of data between on prem & S3, EFS, FSx
        * EC2 Enhanced Networking (SR-IOV)
            * higher bandwitdth, packets per second, lower latency
            * Option 1: Elastic Network Adapter (ENA) up to 100 Gbps
            * Option 2: Intel 82599 VF up to 10 Gbs - LEGACY
        * Elastic Fabric Adapter (EFA)
            * Improved ENA for HPC, only for LINUX
            * great for inter-node communication
        * Storage
            * instance attached storage
                * EBS: 256,000 IOPS 
                * Instance Store: scale to millions IOPS
            * Network Storage: 
                * S3 - large blob
                * EFS - cale IOPS based on total size
                * Amazon FSx for Lustre
        * Automation and Orchestration
            * AWS Batch
                * supports multi-node parallel jobs
            * AWS ParallelCluster
                * open-source cluster management
                * ability to enable EFA on the cluster
    * Creating highly available EC2 instance
        * use elastic IP
        * have standby EC2 instance, failover elastic IP
            * use Cloudwatch event to trigger lambda function
        * have ASG of 1 min, 1 max, 1 desired, 2 AZs
            * use EC2 user data script to attach elastic IP
        * ASG + EBS volumne
            * on EC2 termination, use lifecycle hook to create snapshot
            * create launch lifecycle hook to attach EBS volume
            * use User Data script to attach elastic IP
    
        

        


        

        
    


        



        

    
        
        
        


    

    
    








        


        
    










        

            


            
    





    

            




            


        

    
        
    


        





        




            



                
            




        

            

            


    
        
    

    


        



        

        

    
        
        






                
       


        






            




    
        




        




    

    




    
         









    

    


        

        
    
        
        
            





        

        
    
            



        
    
        




                

        


        
            



        






        
    







            









            

        
        






    


    

        
        








    

    
        

