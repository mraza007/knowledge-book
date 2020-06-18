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