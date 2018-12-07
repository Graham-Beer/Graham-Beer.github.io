---
layout: post
title: "Working with AWS credentials using PowerShell"
date: 2018-12-06 09:00
comments: true
description: "Working with AWS credentials using PowerShell"
categories:
    - PowerShell
    - AWS
tags:
    - PowerShell
    - AWS
---

If you want to automate tasks in AWS with PowerShell, then you'll need a safe way to store your credentials. To have programmatic access, AWS provides you with an access key and a secret key. I'll explore how you can keep them secure on your system and how to call them when you come to work with AWS.

Before continuing, note that I'll be using the PowerShell core module in this article. The details can be found in the PowerShell Gallery. To install from the console, make sure you are using PowerShell Core, and run the following:

```powershell
Install-Module - Name AWSPowerShell.NetCore
```

## Creating access keys

To use AWS programmatically from PowerShell, you need to generate your access keys. To do this, sign into the AWS console, and from the Services tab, select IAM under Security, Identity, & Compliance. From the left-hand side, select Users, and find the username you want to generate access keys for. Change the tab to Security Credentials, and then click on Create access key. You'll be able to continue viewing the access key, but the secret key generated is a one off, so make a note of this.

To continue reading please follow the below link.

[4sysops article continues here...](https://4sysops.com/archives/working-with-aws-credentials-using-powershell/)
