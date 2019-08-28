---
layout: post
title: "Remove users and groups in AWS Identity and Access Management (IAM) with PowerShell"
date: 2019-08-28 09:00
comments: true
description: "Remove users and groups in AWS Identity and Access Management (IAM) with PowerShell"
categories:
    - PowerShell
    - AWS
tags:
    - PowerShell
    - AWS
---

Identity and Access Management (IAM) in AWS is not just about creating users and groups. You must also consider renaming and removal of users and groups. My last article focused on the creation of these items. In this one, I will be looking at other administration tasks.

Dealing with unused groups, users with additional policies after changing roles, and users who have left your company are all tasks that need to be taken care of to keep a clean AWS IAM environment. Using PowerShell, I will be looking to achieve these tasks to allow for automation and reusability.

Some of the areas we will cover are:

+ Renaming a user
+ Group removal process
+ User removal process

Several steps are required before users and groups can be removed. I recommend using a process to automate these steps as much as possible. We will come back to this in a bit. 

[4sysops article continues here...](https://4sysops.com/archives/remove-users-and-groups-in-aws-identity-and-access-management-iam-with-powershell/)
