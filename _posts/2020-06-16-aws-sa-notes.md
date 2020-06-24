---
layout: post
title: AWS Certification Notes for Solutions Architect Associate
tags : [aws,certification,solutions-architect]
published: false
description: notes compiled for people studying for aws solutions architect associate certification.
---

## Solutions Architect Associate Exam 
- Its multiple choice and multiple response questions.
- 130 mins to complete the exam.
- It contains 65 questions and costs `$150` dollars.
- It requires 720 in order to pass the exam.

## Exam Domains
- The Exam consists of following domains.
- **Design Resilient Architectures**
    * Design a multi-tier architecture solution.
    * Design highly available or/ fault-tolerant architectures.
    * Design decoupling mechanisms using `AWS` services.
    * Choosing appropiate resilient storage.
- **Design High-Performing Architectures**
    * Identify elastic and scalable compute solutions for the workload.
    * Selecting high performance and scalable storage solution for a workload.
    * Selecting high performance networking solutions for a workload.
    * Choosing high performance database solutions for the workload.
- **Design Secure Applications and Architectures**
    * Designing secure access to `AWS` resources.
    * Designing secure applications tiers.
    * Selecting appropiate data security options
- **Design Cost-Optimized Architectures**
    * Identify cost-effective storage solutions
    * Identify cost-effective compute and database services.
    * Design cost-optimized network architectures


## IAM (Identity Access Management)
- Its a service that provides `users`,`groups`,`IAM policies`.

- **IAM USER**: Its an entity that represents a person or a service and you associate **IAM Policy** directly with the user and it defines its permissions and what the user is allowed to do within `AWS` environment. Furthermore, a user can be assigned the following.
    + An access `key-pair` that allows user programmatic access to the `AWS API`,`CLI`,`SDK` and other development tools.
    + A password for access to the management console.
    + By default users can't do anything within their accounts.
    + the account user crendentials are usually the email address used to create the account and a password.
    + Root account has full admin priviledges and you can think of it as `sudo`.
    + The best practice is to not use the root crendentials instead create an IAM user assign admin priviledges
    + Never share root crendentials.
    + Make sure you enable `MFA`.
    + IAM users can be created to represent applications and these are known as service accounts
    + You can have upto 5000 users per `AWS` account.
    + Each user account has a friendly name and an ARN(Amazon Resource Name) which uniquely identifies the user across AWS.
    + You should always create individual IAM accounts for the users(Not to share them).
    + A password policy can be defined for users enforcing them to have stronger passwords (applies to all users)

- **IAM GROUP**: Its a collection of users that have policies attached to them such as group for  _developers_,_sys-admins_ .
    + Its not an identity and cannot be identified as principal in an IAM policy.
    + use groups to assigns permissions to the users.
    + always assign the least priviledges when assigning permissions.

- **IAM ROLES**: Think of it as assigning access to the `AWS` services such as you might set a role for `DynamoDB` to readonly.
    + They are created and then assumed by trusted entities and define a set of permissions for making `AWS` requests.
    + with roles you can delegate permissions to resources for users and services without using a permanent credentials.
    + `AWS` users or services can assume a role to obtain temporary security crendentials that can used to make `aws` api calls.
    + 

- **IAM Policies**: They are documents that defines the permissions and can be applied to users,groups and roles.
    + Policy documents are written in `json`.
    + All permissions are implicitly denied by default.
    + the most restrictive policy is applied.
    + The `IAM` policy simulator is a tool that helps you understand,test and validate the effects of access controls policies.

### IAM Authentication Methods.
- You can use `key-pair` access keys and its used for programmatic access especially CLI (You can't add MFA to this).
    + A combination of access key ID and secret key access.
    + this is used to make programmatic calls to aws when using the api. For example `boto`. Its also  used to access `AWS` using `CLI`.
    + you can _create_,_modify_,_view_ or _rotate_ access keys.
    + When created IAM returns the access key ID and secret access key.
    + The secret access key is returned only at the creation time and if lost new key must be created.
    + Make sure access keys and secret access keys are stored securely.
    + Users can be given access to change their own keys through IAM policy(Not from console).
    + You can disable user's access key which prevents it from being used for API calls.
- Simple username and password method to access the management console.
    + The password that user uses to sign in into `aws` web console 
- Some `AWS` services uses signing certificate.
    + `SSL/TLS` certificates that can be used to authenticate with some `AWS` services.
    + `AWS` recommends that you use `ACM`(AWS Certificate Manager) to provision manage and deploy your server certificates.
    + You can also use `IAM` only when you need to support `HTTPS` connections in a region that is not supported by `ACM`.

### MFA (MultiFactor Authentication)

- Having physical token or soft token that will allow you to access the `AWS` 
    + Soft token can be `Google Authenticator`
    + Hard Token can be `YuBi Key`.
    + AWS also provides soft token and physical access keys for MFA.
    + By having two factors of authentication it makes it very secure.

### STS (AWS Security Token)
- STS is a web service that enables you to request temporary, limited-priviledge crendentials for IAM users or for the users that you authenticate (federated users).
- By default `AWS` STS is available as global service and all AWS STS requests go to a single endpoint at  `https://sts.amazonaws.com`
- All regions are enabled by default for STS but can be disabled.
- The region in which temporary crendentials are requested must be enabled.

## AWS Global Infrastructure Overview
- **Region**: A geographical area with 2 or more AZs, isolated from other AWS regions. There are 23 regions around the world at the moment.
- **Availability Zone**: One of more data centers that are physically separate and isolated from other AZs.
- **Edge Location**: A location with Cache Content that can be delivered at low latency to the users. Mostly used by CloudFront for delivering content such as static files or video content.
- **Regional Edge Cache**: Also part of the CloudFront network.These are larger caches that sit between `AWS` services and Edge Locations.
- **Global Network**: Highly Available, low latency private global network interconnecting every data center, AZ and AWS region.

## VPC (Virtual Private Cloud)
- Its an isolated section of AWS where you can launch your own resources.
- Within VPC you can create your own networks with your own `IP` ranges.
- A VPC sits within a region and you create an VPC within that region and then you create subnets within that regions that sits within AZs. The subnets can public or private and then we launch resources within those subnets.
_You can have 5 VPCs within the region by default._
- A VPC Router is used to communicate within subnets and availablity zones and it has a route table that we can configure and it has an IP address range. Basically every VPC has a `CIDR`(Classless Inter-Domain Routing) block.
- You define the IP range for your VPC.
- You can also attach internet gateway to your VPC that sends requests to the outside internet and for that we need `igw-id` and `IP-ADDR` as destination.
- Internet Gateways allows you to make requests to the public internet and for that we have to add the entry to the route table.
- Each VPC has its own `CIDR` block.

## EC2 (`Elastic Compute Cloud`)
- Its an elastic service that allows you to launch compute resources on the AWS cloud. In AWS context we call EC2 instances but you can think of them as virual machines.
- Each instance has an operating system,storage and virtual hard drive.
- When launching instances you can choose from `AWS MarketPlace` and 
`Community AMIs`.
- You can connect to EC2 instances using `ssh`.

### Security Groups.
- These are firewalls that are applied at the instance level.
- They monitor traffic going in and out of EC2 instances.
- You can have multiple instances in a security group and you can have multipe security groups applied to the instances.
- Security groups are stateful.
- For example having a security group with `port 22` access applied to the ec2 instances will allow you to launch secure shell.

### Instance Metadata
Instance metadata is data about your instance that can be used to configure or manage the running instance. Its divided into categories and gives you the information about your instance such as `hostname`,`ami-id` and `etc`.

You can run this command within your `ec2` in commandline to get the meta data

```console
curl http://169.254.169.254/latest/meta-data/
```

### Instance Userdata

Its basically the information that you can pass into instance when it boots up.

Think of it as a bash script contains all the commands that you need to run when booting up the `ec2` instance.
This can be your regular ubuntu `apt` commands. For example

```bash

#!bin/bash
sudo apt-get upgrade
sudo apt install xyz

```

You can basically paste this into `userdata` located in advance details when launching an instance.

you can also make a request to userdata by following command.

```console
curl http://169.254.169.254/latest/meta-data/
```

### Status Checks and Monitoring
You can setup cloudwatch alarms on `ec2` instances to help monitor the instances effectively and futhermore you can also install `stress` tool to perform stress testing on the instances.

### Public,Private and Elastic IP addresses.
- **Public IP Address**: 
    + Lost when the instance is stopped.
    + Used in public subnets.
    + No Charge
    + Associated with private IP address on the instance.
    + Cannot be moved between instances.
- **Private IP  Address**:
    + Retained when the instance is stopped.
    + Used in public and private subnets.
- **Elastic IP Address**:
    + Static Public IP Address.
    + You are charged if not used.
    + Associated with a private IP address on the instance.
    + **Note:** Elastic Fabric Adapter is a network device that you can attach to reduce latency and increase throughput for distributed HPC(High Performance Computing)

### Private Subnets and Bastion Hosts
- _Public Subnets are easily accessible through public internet_
- Private Subnet route table doesn't have the `igw` route and its not configured to provide public ip addresses to the instances launched into this.
- There's no way to directly manage this instance through the internet.
- A bastian host is a public instance that you used to jump to private instance and it is also known as jump host and through bastian host you will be able to `ssh` into your private instance.
- We use agent forwarding and use the bastian host to connect to the private subnet instance.

### NAT Instances and NAT Gateways

- **NAT Instance:** Network Address Translation(NAT).Its basically a process of taking the private ip address and translating it to public ip address so it allows you to connect to the public internet.
    + It's managed by you
    + The only way to scale this is to do it manually which means using a powerful and bigger instance with more resources and enhanced networking with additional bandwith.
    + There's no high availability and it has to be done manually.
    + Need to assign security groups.
    + Can be used as bastion host.
    + You can use an Elastic IP address or a public address with a NAT instance.
    + Can implement port forwarding manually
- **NAT Gateway:** A better version of NAT Instance. 
    + Its managed by AWS
    + Its elastically scaled upto 45 GBps
    + Provides automatic high availability within an AZ and can be placed in multiple AZs.
    + No security groups
    + Cannot be accessed through `ssh`
    + Choose the Elastic IP address to associate with a NAT instance gateway at creation.
    + Doesn't support port forwarding.
- Lastly Private Subnet contains `nat-gateway-id` instead of `igw-id` and anything that isn't defined within `ip cidr block` of private subnet route table is handled by the `nat-gateway-id`.

### EC2 Placement Groups
- **Cluster**: packs instances close together inside an Availability Zone. This strategy enables workloads to achieve the low latency network performance necessary for tightly coupled node to node communication that is typical of HPC applications.
    + **WHAT:** Instances are placed into a low latency group within a single AZ
    + **WHEN:** Need a low network latency or high network throughput.
    + **Pros:** Get most out of enhanced networking instances.
    + **Cons:** Finite capacity recommends launching all you might need upfront.

- **Partition**: spreads your instances across logical partitions such that groups of instances in once partition do not share the underlying hardware with groups of instances in different partitions.This strategy is typically used by large distributed and replicated workloads such as Hadoop,Cassandra and Kafka.
    + **WHAT:** Instances are grouped into logical segments called partitions which use distinct hardware.
    + **WHEN:** Need control and visibility into instance placement.
    + **Pros:** Reduces likelihood of correlated failures for large workloads.
    + **Cons:** Partition placement groups are not supported for dedicated hosts.

- **Spread**: strictly places a small group of instances across distinct underlying hardward to reduce correlated failures. 
    + **WHAT:** Instances are spread across underlying hardware.
    + **WHEN:** Reduce the risk of simultaneous instance failure if underlying hardware fails.
    + **Pros:** Can span multiple AZs
    + **Cons:** Maximum upto 7 instances running per group,per AZ.

### Few Notes
- Amazon EC2:
    + Its a compute cloud basically a web service that provides resizeable compute capacity in the cloud.
    + With EC2 you have full control of the operating system layer.
    + You use key-pair to securely connect to ec2 instances using ssh.
    + A keypair consists of public key that AWS stores and private key that we store on our local machine.
    + `user-data` is a script that you provide when starting an instance.
    + `meta-data` is the data of your instance that you can use to configure the instance.
- EC2 Pricing Models:
    + **On Demand**:
        * No upfront fee.
        * Charged per hour or second
        * No Commitment
        * Ideal for short term needs or unpredictable workloads
    + **Reserved**:
        * Options: No Upfront,Partial Upfront or all Upfront.
        * Charged by hour or second.
        * 1year or 3year commitment.
        * Ideal for steady-state workloads and predictable usage.
    + **Spot**:
        * No upfront fee
        * Charged by hour or second
        * No commitment
        * Ideal for cost sensitive, compute sensitive use cases that can withstand interruption. (Batch Processing)
    + **Dedicated Hosts**:
        * Good for enterprise customers who are looking for isolated environments
- Amazon EC2 AMIs:
    + An Amazon Machine Image provides the information required to launch an instance
    + An AMI includes the following
        * A template for the root volume for the instance (OS,system,an application server and applications).
        * Launch permissions that control which AWS accounts can use the AMI to launch instances.
        * A block device mapping that specifies the volumes to attach to the instance when its launched (which EBS to attach to the instance).
    + Volumes attached to the instances are either EBS or Instance store.
        * Amazon EBS provided persistent store.EBS snapshots resides on Amazon S3 are used to create the volume.
        *  Instance store volumes are ephermeral(non-persistent).this means data is lost when the instance is shut down/
    + AMIs are regional.You can only launch an AMI from the region in which it is stored. However you can copy AMI's to other regions using console,CLS or API.
- Elastic Network Interface (ENI):
    + A logical networking component in a VPC that represents a virtual network card.
    + Can include attributes such as IP addresses,security groups,MAC addresses, source/destination check flag,description.
    + You can create and configure network interfaces in your account and attach them to your instances in VPC.
    + `eth0` is the primary network interface and cannot be moved or detached.
    + An ENI is bound to an availability zone and you can specify which subnet/AZ you want the ENI to be loaded in.
- Elastic Fabric Adapter (EFA):
    + An AWS Elastic Network Adapter (ENA) with added capabilities.
    + Enables customers to run applications requiring high levels of internode communications at scale on AWS.
    + With EFA, High Performance Computing (HPC) applications using the Message Passing Interface (MPI) and Machine Learning (ML) applications using NVIDIA Collective Communications Library (NCCL) can scale upto thousands of CPUs or GPUs.
- ENI vs ENA vs EFA:
    + **When to use ENI**: This is the basic adapter type for when you don't have any high performance requirements. Can use with all instance types.
    + **When to use ENA**: Good for use cases that require higher bandwidth and lower inter-instance latency. Supported for limited instance types (HVM only).

## Elastic Load Balancing and Auto Scaling.

### Elastic Load Balancing
Load Balancing refers to efficiently distributing incoming network traffic across a group of backend servers.

### Application Load Balancer
- Operates at the request level.
- Routes based on the content of request (Layer 7)
- Supports path based routing,host based routing,query string parameter based routing and source IP address based routing.
- Supports IP addresses, Lambda Functions and containers as targets.
- `HTTPS`,`HTTP`
- 
### Network Load Balancer
- Operates at the connection level.
- Routes connections based on IP protocol Data (layer 4)
- Offers ultra high performance,low latency,and TLS offloading at scale.
- Can have static IP / Elastic IP
- Supports UDP and static IP addresses as targets.

### Classic Load Balancer
- Old generation; not recommended for new applications.
- Performs routing at Layer 4 and Layer 7
- Use for existing applications running on EC2-Classic instance

### Internet Facing VS Internal
- Internet Facing:
    + ELB nodes have public IPs.
    + Routes traffic to the private IP addresses of the EC2 instances.
    + Need one public subnet in each AZ where ELB is defined.
    + ELB dns name format `<name>-<id-number>.<region>.elb.amazonaws.com`
- Internal only ELB:
    + ELB nodes have private IPs.
    + Routes traffic to the private IPs of the EC2 instances.
    + ELB dns name format `internal-<name>-<id-number>.<region>.elb.amazonaws.com`

### Elastic Load Balancing
- EC2 instances and containers can be registered against an ELB
- ELB nodes use IP addresses within your subnets, ensure at least a /27 subnet
and make sure there are at least 8 IP addresses available in that order for the ELB to scale.
- An ELB forwards traffic to `eth0` (primary IP address).
- An ELB listener is the process that checks for the connection requests:
    + Listeners for CLB provide options for `TCP` and `HTTP`/`HTTPS`.
    + Listeners for ALB only provide options for `HTTPS` and `HTTP`
    + Listeners for NLB only provide only `TCP` as an option
### ELB Security Groups
- Security Groups control the ports and protocols that can reach the front-end listener.
- You must assign a security group for the ports and the protocols on the front end listener.

### ELB Monitoring
- CloudWatch every 1 min.
- ELB service sends information when requests are active.
- Access Logs:
    + Disabled by Default.
    + Includes information about the client(not included in the Cloud Watch Metrics)
    + Can identify requester IP,request type etc.
    + Can be optionally stored and retained in S3.
### EC2 Auto Scaling
- You can attach one or more classic ELBs to your existing Auto Scaling Groups.
- You can attach one or more Target Groups to your ASG to include instances behind an ALB.
- The ELBs must be in same region.
- Launch configuration is the template used to create new EC2 instances and includes parameters such as instance family,
instance type,AMI,keypair and security groups.

<table>
  <tr>
    <th>Scaling Option</th>
    <th>What is it?</th> 
    <th>When to use?</th>
  </tr>
  <tr>
    <td>Maintain</td>
    <td>Ensures the required number of instances are running</td>
    <td>Use when you always need a known number of instances running at all times</td>
  </tr>
  <tr>
    <td>Manual</td>
    <td>Manually change the desired capacity via console or CLI</td>
    <td>Use when your needs change rarely enough that you are Ok! to make manual changes.</td>
  </tr>
  <tr>
    <td>Schedule</td>
    <td>Adjust Min/Max instances on specific dates/times or recurring time periods</td>
    <td>Use when you know you are busy and quiet times are. Useful for ensuring enough instances are available before busy times</td>
  </tr>
    <tr>
    <td>Dynamic</td>
    <td>Scale in response to a system load or other triggers using metrics</td>
    <td>Useful for changing capacity based on system usage eg if cpu hits 80%</td>
    
  </tr>
</table>

### EC2 Autoscaling - Scaling Types
<table style="width:50%">
  <tr>
    <th>Scaling</th>
    <th>What is it?</th> 
    <th>When to use?</th>
  </tr>
  <tr>
    <td>Target Tracking Policy</td>
    <td>The scaling adds or removes capacity as required to keep the metric at or close to the specified target value</td>
    <td>A use case,when you want to keep the aggregate CPU usage of your ASG at 70%</td>
  </tr>
  <tr>
    <td>Simple Scaling Policy</td>
    <td>Waits until health check and cool down period expires before revaluating</td>
    <td>This is more conservative way to add/remove instances.Useful when load is eratic.AWS recommend step scaling instead of simple in most cases</td>
  </tr>
  <tr>
    <td>Step Scaling Policy</td>
    <td>Increase or decrease the current capacity of your Auto Scaling group based on a set of scaling adjustments known as step adjustments</td>
    <td>Useful when you want to vary adjustments based on the size of the alarm breach</td>
  </tr>
</table>

- Can also scale based on AWS SQS.
- Uses a custom metric that's sent to Amazon Cloud Watch that measures the number of messages in the queue per EC2 instance in the auto scaling group.
- Then use a target tracking policy that configures your ASG to scale based on the custom metric and a set target value. Cloud watch alarms invoke the scaling policy.
- Use a custom `backlog per instance` metric to track not just the number of messages in the queue but the number available for retrieval.

### EC2 Autoscaling - Terminatio Policy
- Termination policies control which instances are terminated first when scale in event occurs.
- There is default termination policy and options for configuring your own customized termination policies.
- The default termination policy is designed to help ensure that instances span AZs evenly for High Availability Zones.
- The default policy is kept generic and flexible to cover a range of scenarios.

