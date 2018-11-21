---
layout: post
title: "Create and view EC2 security groups with PowerShell"
date: 2018-11-21 09:00
comments: true
description: "Create and view EC2 security groups with PowerShell"
categories:
    - PowerShell
    - AWS
tags:
    - PowerShell
    - AWS
---

For those of you who are new or unfamiliar with security groups in Amazon Web Services (AWS), they are a virtual firewall for your Elastic Compute Cloud (EC2) instance to control inbound and outbound traffic. I will be looking at how we can create and view our security groups with PowerShell using the AWSPowerShell.NetCore module.

Before delving into creating security groups with PowerShell, it is worth noting their basics:

- First, you are creating AWS security groups on the EC2 instance itself and not at the subnet level.
- By default, a security group includes an outbound rule that allows all outbound traffic.
- When creating rules, you can specify allowing them, but there is no deny feature.
- You are creating rules for inbound and outbound traffic as separate rules.

To continue reading please follow the below link.

[4sysops article continues here...](https://4sysops.com/archives/create-and-view-ec2-security-groups-with-powershell/)
