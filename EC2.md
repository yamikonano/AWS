# EC2 #

## IaaS ##

EC2 - virtual machine\
EBS - storing data on virtual device\
ELB - load balance\
ASG - auto scaling group

## Bootstrap ##

- Lauching command when a machine starts
- User data

**Be careful of auto-scaling group**
<br/></br>

> ### **Advance setup** ###

> Metadata
>
> - enable the HTTP endpoint (default)
> - Metadata hot pot response:
>   - The number of network hops that the metadata token can travel. Maximum is 64<br/>
> ![capture of advance setting](assets/Screenshot%202022-06-29%20172006.png)
<br/><br/>

```bash
#!/bin/bash
# Use this for your user data (script from top to bottom)
# install httpd (Linux 2 version)
# add 'sudo'
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello World from $(hostname -f)</h1>" > /var/www/html/index.html
```

## [Instance Types](https://aws.amazon.com/ec2/instance-types/)

### **m5.2xlarge** ###

- m: instance class
- 5: generation
- 2xlarge: size of instance

### **Computer Optimized**

- compute intensive tasks
- e.g. HPC (high performance computing), machine learn...

### **Memory Optimized**

- Fast performance
- real-time tasks, processing unstructure data
- web scale cache
- RDB, non RDB, DB for BI (business intelligence)

### **Storage Optimized**

- high frequency transaction processing
- NoSQl  DB
- RWA (read write & access) large dataset
- data warehousing applicaiton
<br/><br/>

# Security

-firewall on EC2 instances (outside)

- contains only **allow** rule
- by IP / security groups
- **Timeout** = blocked
- connection refuse = application error
</br></br>

## SSH ##

Secure shell

- Cli / Putty / Instance connect (through browser)
- Port 22

```cli
ssh -i [keyfile] username@public-ip
```

### how to fix 'Permission 0644 for pem file' ###

- occur because file permission setting
- should not be accessed by others

```cli
chmod 0400 [file]
```

</br></br>
Windows Putty

- putty gen
- load private key file
- save private key
- (optional) passphrase more secure
- goto session
- load a session
- connection -> SSH -> Auth -> load ppk
- save session
</br></br>

## Instance with IAM ##

Actions -> security -> Modify IAM
![IAM load](assets/Screenshot%202022-07-04%20104729.png)

# How to Choose EC2 #

## On-Demand

- predictable pricing
- bill per sec/hour
- high cost, no upfront payment

> short terms and un-interrupted workload

## Reserved (at least 1 year) ##

- long workload
- flexible
- Convertible reserved
  - can change instance type
  - up to 54% discount
- Scheduled reserved (deprecated)
- up to 75% discount
  - no/partial/all upfront

> steady state usage (database?)

## Spot

- highest discount (90%)
- short workload
- less reliable
- cheap
- can lose instances(?) at any spot
  - resilient to failure

> batch jobs
> data analysis
> image processing
> any distributed workloads
> flexible start/end time

## Dedicated host

- entire physical server
- control placement
- 3-year allocation

> Bring your own license
> have strong regulatory or compliance needs

## Dedicated instance

- instance on hardware
- can share hardware in same account
- no control over instance placement

![different dedicated](assets/dedicated.png)
<br></br>

## Spot Instance ##

**Max Spot price**

- current < Max
- if greater, then terminate/stop within 2 mins

**Spot Block**

- within a time frame (1-6 hours) not reclaim
- one time request
  - fail, cancel, close
- persistent (*have to stop this request before terminate*)
  - active, disabled

## Spot Fleets

define a pool

- lowest price
- diversitified
- capacity optimized

> automatically request a spot with lowest price

</br></br>

## Elasic IP ##

static ip

1. choose elastic ip
2. allocate ip
3. action -> associate ip with a instance
4. release ip (delete the ip)
</br></br>

## Placement group

- Cluster
  - all in same rack
  - <span style="color:green"> great network</span>
  - <span style="color:green"> lowest latency</span>
  - <span style="color:red"> single point failure</span>

> big data job, need complete fast

- Spread
  - locate on different hardwares
  - <span style="color:green"> low risk on simultaneous failure</span>
  - <span style="color:red"> limit to 7 instances per AZ per placement group</span>
  - <span style="color:red"> certain size of application</span>

> Maximise high availability of App
> requires isolation of one point failure

- Partition
  - up to 7 partiion per AZ
  - have many instances in each partition (up to 100 EC2)
  - <span style = "color:green">isolation within partition</span>
  - <span style = "color:green">EC2 access partition info as metadata</span>

> HDFS, Kafka, Cassandra

## [ENI](https://docs.aws.amazon.com/zh_tw/AWSEC2/latest/UserGuide/using-eni.html) ##

- local components in VPC
- virtual network card

|ENI attributes |   |    |   |   |   |   |
|:---| :--- | :---| :---| :---| :---| :---|
|Primary private IPv4|one or more secondary IPv4
|One elastic IP per IPv4|
|One public IPv4|
|One or more security groups|
|A MAC address|

- attach IP to instance
- network failover
- ENI alongside with intances will be deleted when instances terminated

## [EC2 Hibernate](https://docs.aws.amazon.com/zh_tw/AWSEC2/latest/UserGuide/ebs-volumes.html) ##

- <g>Preserved in RAM</g>
- <g>boot fast</g>
- <g>encrypted, EBS </g>
- < 150GB, < 60 days

## EC2 nitro ##

- unerlying platform
- next generation of EC2 instance
- better performance
  - enhanced networking, HPC IPv6
  - high speed EBS
- better security

## EC2 nitro ##

- multithreading
- e.g. m5.2xlarge = 4 CPU * 2 threads
- **decrease # of cores**: <g>lower cost, higher RAN
- **decrease # of threads**: <g>HPC workloads
<br></br>

## [EBS Volume](https://docs.aws.amazon.com/zh_tw/AWSEC2/latest/UserGuide/ebs-volumes.html) ##

- network drive
- mount to only one instance (CCP level, aka certified cloud partitioner)
- bound to a specific AZ. cannot attach to other AZ
- delete on termination: default tick to root
- snapshot of volume can across AZ / region -> help to **move** volume / AMI

![volumn setting first on instance](assets/volume-setting-on-instance.png)

## EC2 intance store ##
- <g> higher performance
- <r> short term ()
- <g> good for bufferm cache...
- need back-up

*Type*

> Characterized in size || throughput || IOPS\
> Only GP && IO can used as boot volume

**gp2 / gp3**
- <y> ssd
- general purpose
- wide variety of work
- 1GB - 16TB
> Boot volumes, virtual desktop, DT enviornment\

**io1 / io2**
- <y> ssd
- highest-performance
- low latency, high throught-put

**sl1**
- <b> hhd </b>
- low cost
- frequent access

**sc1**
- <b> hhd </b>
- lowest cost



<style>
    r {color:red}
    b {color:blue}
    g {color:green}
    y {color:yellow}
</style>
