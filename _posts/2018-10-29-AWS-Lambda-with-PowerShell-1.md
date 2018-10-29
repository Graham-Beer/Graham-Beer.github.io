---
layout: post
title: "AWS Lambda with PowerShell"
date:   2018-10-29 09:00
comments: true
description: "AWS Lambda with PowerShell"
categories:
    - PowerShell
    - AWS
tags:
    - PowerShell
    - AWS
---

Amazon Web Services (AWS) released some exciting news back in September, announcing Lambda support for PowerShell Core. The feature now enables us to execute PowerShell scripts and functions to respond to events in AWS.

Before we start writing our scripts and functions, we need to know what prerequisites we need to create a PowerShell AWS Lambda environment. A few steps need to occur to achieve this:

1.    Install PowerShell Core  
    https://github.com/powershell/powershell
2.    Install the .NET Core 2.1 Software Development Kit (SDK)  
    https://www.microsoft.com/net/download
3.    Install the AWSLambdaPSCore module  
    Install-Module AWSLambdaPSCore -Scope CurrentUser

When installing the .NET Core 2.1 SDK, make sure you install the SDK and not the runtime. Once you have installed these components, you're ready to start!

To continue reading please follow the below link.

[4sysops article continues here...](https://4sysops.com/archives/aws-lambda-with-powershell/)
