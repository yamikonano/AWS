# EC2 #
## IaaS ##
EC2 - virtual machine\
EBS - storing data on virtual device\
ELB - load balance\
ASG - auto scaling group

## Bootstrap ##
- Lauching command when a machine starts
- User data
<br/><br/>
> ### **Advance setup** ###
> Metadata
> - enable the HTTP endpoint (default)
> - Metadata hot pot response: 
>   - The number of network hops that the metadata token can travel. Maximum is 64<br/>
> ![capture of advance setting](assets/Screenshot%202022-06-29%20172006.png)
<br/><br/>
```bash
#!/bin/bash
# Use this for your user data (script from top to bottom)
# install httpd (Linux 2 version)
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
# Security group
-firewall on EC2 instances
- contains only **allow** rule
- by IP / security groups
- Timeout = blocked