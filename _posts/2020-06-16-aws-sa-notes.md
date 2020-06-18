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