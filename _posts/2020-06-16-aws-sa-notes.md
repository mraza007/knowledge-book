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

### EC2 Autoscaling - Termination Policy
- Termination policies control which instances are terminated first when scale in event occurs.
- There is default termination policy and options for configuring your own customized termination policies.
- The default termination policy is designed to help ensure that instances span AZs evenly for High Availability Zones.
- The default policy is kept generic and flexible to cover a range of scenarios.

## Virtual Private Cloud
- A Virtual Private Cloud (VPC) is logically isolated from other VPCs on AWS.
- VPCs are regions specific
- A default VPC is created in each region with a subnet in each AZ.
- You can define dedicated tenancy for a VPC to ensure instances are launched on a dedicated hardware.
- The default VPC has all public subnets.
- Public Subnets:
  + `Auto Assign public IPv4 address` set to `Yes`
  + The subnet route table has an attached Internet Gateway.
- Instances in the default VPC always have both a `public` and `private` IP addresses.
- AZ names are mapped to different zones for different users.

### Amazon VPC Components.
- VPC: A logically isolated virtual network in the AWS cloud. You define VPC's IP address space from ranges you select.
- Subnet: A segment of a VPC's IP address range where you can place groups of isolated resources (maps to sinlge AZ).
- Internet Gateway: The Amazon VPC side of a connection to the public internet.
- NAT Gateway: A highly available,managed Network Address Translation (NAT) service for your resources in a private subnet to access the internet.
- Hardware VPN Connection: A hardware based VPN connection between your AWS VPC and your data center,home network, or co location facility.
- Virtual Private Gateway: The VPC side of an VPN Connection.
- Customer Gateway: Our side of a VPN Connection.
- Router: Routers interconnect subnets and direct traffic between Internet gateways,virtual private gateways, NAT gateways and subnets.
- Peering Connection: A peering connection allows you to route traffic via private IP addresses between two peered VPCs
- VPC Endpoints: Enables private connectivity to services hosted in AWS from within VPC without using an Internet Gateway,VPN,NAT Devices or firewall proxies.
- Egress-only Internet Gateway: A stateful gateway to provide egress only access for IPv6 traffic from the VPC to the internet.

### Amazon VPC - Routing
- The VPC router performs routing between AZs within a region.
- The VPC router connects different AZs together and connects the VPC to the internet Gateway.
- Each subnet has a route table the router uses to forward traffic withing the VPC.
- Route tables also have entries to external destinations.

### Amazon VPC - Subnets
- Types of subnets:
  + If a subnet's traffic is routed to an internet gateway the subnet is known as a public subnet.
  + If a subnet doesn't have a route to the internet gateway the subnet is known as private subnet.
  + The VPC is created with a master address range(CIDR block,can be anywhere from 16-28 bits) and subnet ranges are created within that range.
  + New subnets are always associated with the default route table
  + Once VPC is created you cannot change the CIDR block.
  + You cannot created additional CIDR blocks that overlap with existing CIDR blocks
  + You cannot create additional CIDR blocks in a different RFC 1918 range.
  + Subnets with overlapping IP address ranges cannot be created
  + The first 4 and last 1 IP address in a subnet are reserved.
  + Subnets are created within AZs
  + Subnets map 1:1 to AZs and cannot span AZs.

### Amazon VPC - Internet Gateways.
- An Internet Gateway serves two purposes:
  + To provide a target in your vpc route tables for internet routable traffic.
  + To perform network address translation(NAT) for instances that have been assigned public IPv4 addresses.
- Internet Gateways must be created and then attached to a VPC, be added to a route table and then associated with the relevant subnets.
- No availability risk or bandwidth constraints.
- You cannot have multiple Internet Gateways in a VPC
- Egress-Only internet Gateway provides outbound Internet Access for IPv6 addressed instances.

### Amazon VPC - Secuirty Groups
- Security Group act like a firewall at the instance level (network interface) level.
- Can only assign permit rules in a security group, cannot assign deny rules.
- All rules are evaluated until a permit is encountered or continues until the implicit deny.
- Can control ingress and egress traffic.
- Security groups are stateful
- By default, custom security groups do not have inbound allow rules (all inbound traffic is denied by default).
- By defualt, default security groups do have inbound allow rules (allowing traffic from within the group).
- All outbound traffic is allowed by default in custom abd default security groups.
- You cannot delete the security group that's created by default within a VPC.
- You can use security group names as the source of destination in other security groups.
- You can use the security group name as a source in its own inbound rules.
- Secuirty group membership can be changed whilst instances are running.
- Any changes made will take effect immediately.
- You cannot block specific ip addresses using the security groups use NACLs instead.

### Amazon VPC - Network ACLs
- Network ACLs function at the subnet level.
- With NACLs you can have permit and deny rules.
- Network ACLs contain a numbered list of rules that are evaluated in order from the lowest number until the explicit deny.
- Network ACLs have separate inbound and outbound rules and each rule can allow or deny traffic.
- Network ACLs are stateless so responses are subject to the rules for the direction of traffic.
- NACLs only apply to traffic that is ingress or egress to the subnet not to traffic within the subnet.
- A VPC automatically comes with a default network ACL which allows all inbound/outbound traffic.
- A custom NACL denies all traffic both inbound and outbound by default.
- All subnets must be associated with a network ACL.
- You can create custom network ACLs. By default each custom network ACL denies all the inbound and outbound traffic until you add rules.
- You can associate a network ACL with multiple subnets; however a subnet can only be associated with one network ACL at a time.
- Network ACLs do not filter traffic between instances in the same subnet.
- NACLs are preffered option when it comes to blocking specific IPs or ranges.
- Security groups cannot be used to block specific ranges of IPs.
- NACL is the first line of defence, the secuirty group is the second.
- Changes to NACL take immediately.

<table>
  <tr>
    <th>Security Group</th>
    <th>Network ACL</th> 
  </tr>
  <tr>
    <td>Operates at the instance level</td>
    <td>Operates at the subnet level</td>
  </tr>
  <tr>
    <td>Support allow rules only</td>
    <td>Supports allow and deby rules only</td>
  </tr>
  <tr>
    <td>Stateful</td>
    <td>Stateless</td>
  </tr>
   <tr>
    <td>Evaluates all rules</td>
    <td>Processes rules in order</td>
  </tr>
   <tr>
    <td>Applies to an instance only if associated with a group</td>
    <td>Automatically applies to all instances in the subnets its associated with</td>
  </tr>
</table>


### Amazon VPC - Connectivity
- There are several methods of connecting to a vpc and these include
- AWS Managed VPN
  + What: AWS Managed IPSec VPN Connection over your existing internet.
  + When: Quick and usually simple way to establish a secure tunnelled connection to a vpc; Redundant link for direct connect or other VPC VPN
  + Pros: Supports static routes or BGP peering and routing
  + Cons: Dependant on your internet connection.
  + How: Create a virtual private gateway on AWS and Customer gateway on the on premise.
- AWS Direct Connect
  + What: Dedicated network connection over private lines straight into AWS backbone.
  + When: Requires a large network link into AWS; lots of resources and services being provided on AWS to your coporate users
  + Pros: More predictable network performance; potential bandwidth cost reduction; upto 10 GBps provisioned connections; supports BGP peering and routing.
  + Cons: May require additional telecom and hosting provider relationships and /or network circuits; costly;takes time to provision.
  + How: Work with your existing data networking provider;create virtual interfaces (VIFs) to connect to VPCs (Private VIFs) or other AWS services like s3 or glacier (public VIFs).
- AWS Direct Connect plus a VPN
  + What: IPSec VPN connection over private lines (DirectConnect).
  + When: Need the added security of encrypted tunnels over direct connect.
  + Pros: More secure (in-theory) than Direct Connect Alone.
  + Cons: More complexity introduced by VPN Layer
- AWS VPN CloudHub
  + What: Connect location in the hub and spoke manner using AWSs VPC.
  + When: Link remote offices for backup or primary WAN access to AWS resources.
  + Pros: Reuses existing Internet Connections;supports BGP routes to direct traffic
  + Cons: Dependant on Internet Connection; No inherent redundacny
  + How:  Assign multiple Customer Gateways to Virtual Private Gateway, each with their own BGP ASN and unique IP ranges.
- Software VPN
  + What: You provide your own VPN endpoint and software
  + When: You must manage both ends of the vpn connection for compliance reasons or you want to use a VPN option not supported by AWS
  + Pros: Ultimate flexibility and manageability
  + Cons: You must design for an needed redundancy across the whole chain
  + How:  Install VPN software via marketplace appliance of on an EC2 instance.
- Transit VPC
  + What: Common strategy for connecting geographically dispersed VPCs and locations in order to create a global network transit center.
  + When: Locations and VPC-deployed assests across multiple regions that need to communicate with one another.
  + Pros: Ultimate flexibility and manageability but also AWS-managed VPN hub-and-spoke between VPCs
  + Cons: You must design for any needed redundancy across the whole chain 
  + How:  Providers like cisco,juniper networks and riverbed have offerings that work with their equipment and AWS VPC
- VPC Peering:
  + What: AWS provided network connectivity between two VPCs
  + When: Multiple VPCs need to communicate or access each others resources.
  + Pros: Uses AWS Backbone without traversing the internet.
  + Cons: Transitive peering is not supported
  + How:  VPC peering request made; accepter accepts the request (either within or across the accounts)
- AWS Private Link:
  + What: AWS provided network connectivity between VPCs and or AWS services using interface endpoints.
  + When: Keep private Subnets truly private by using AWS backbone to reach other AWS or Marketplace services rather than the public internet.
  + Pros: Redundant;uses AWS backbone
  + How : Create endpoint for required AWS or Marketplace service in all required subnets;access via provided DNS hostname.
- VPC Endpoints:
<table>
  <tr>
    <th> </th>
    <th>Interface Endpoint</th>
    <th>Gateway Endpoint</th>
  </tr>
  <tr>
    <td>What</td>
    <td>Elastic Network Interface with a private IP</td>
    <td>A gateway that is a target for a specific route</td>
  </tr>
  <tr>
    <td>How</td>
    <td>Uses DNS entries to redirect traffic</td>
    <td>Uses prefix lists in the route table to redirect traffic</td>
  </tr>
  <tr>
    <td>Which services</td>
    <td>API Gateway,CloudFormation,CloudWatch</td>
    <td>Amazon S3,DynamoDB</td>
  </tr>
  <tr>
    <td>Security</td>
    <td>Security Groups</td>
    <td>VPC Endpoint Policies</td>
  </tr>
</table>

## Route 53
According to wikipedia, _Amazon Route 53 is a scalable and highly available Domain Name System._ It was lauched in December 2010 and has been part AWS since then.
Route 53 allows you register domain name for your service and it offers following functions:
- Domain Name Registry.
- DNS Resolution
- Health Checks of the resources

Route 53 is located alongside of all edge locations and when you register a domain with Route 53 it becomes the authoritative DNS server for that domain and creates a public hosted zone.

Route 53 also you to transfer your domains to it as long as it supports TLD(Top Level Domain) is supported and you can even transfer to another registrar by contacting aws support.

You can also transfer a domain to another account in AWS however it does not migrate the hosted by default(optional) and its also possible to have domain registered in one aws account and hosted zone in the other aws account

Route 53 also allows you have to private DNS which lets you have authoritative DNS within your VPCs without exposing DNS records to the public internet.

Lastly you can use AWS Management Console or API to register new domain names with route 53 

### Route 53 Hosted Zones
- A hosted zone is a collection of records for a specified domain.
- A hosted zone is analogous to a traditional DNS zone file; it represents a collection of records that can be managed together.
- There are two types of zones
    + Public Hosted Zone: Determines how much traffic is routed on the internet
    + Private Hosted Zone for VPC: Determines how traffic is routed within VPC (Not accessible outside of VPC).
- For Private hosted zones you must set the following VPC settings to `true`
    + enableDnsHostname
    + enableDnsSupport
You also need to create a DHCP options set.

### Route 53 Health Checks
- Health checks ensures the instance health by connecting to it.
- Health can be pointed at:
    + Endpoints (IP addresses or Domain Names)
    + Status of other health checks
    + Status of cloudwatch alarm 

Route 53 supports most of the DNS record types.