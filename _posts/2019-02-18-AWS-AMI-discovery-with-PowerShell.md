---
layout: post
title: "AWS AMI discovery with PowerShell"
date: 2019-02-18 09:00
comments: true
description: "AWS AMI discovery with PowerShell"
categories:
    - PowerShell
    - AWS
tags:
    - PowerShell
    - AWS
---

Using PowerShell is one way of finding available Amazon Machine Images (AMIs). The two main cmdlets Amazon's AWSPowerShell.NetCore module provides are Get-EC2Image and Get-EC2ImageByName. Get-EC2Image uses the powerful type Amazon.EC2.Model.Filter to allow detailed filtering. By looking at the Amazon Web Services (AWS) documentation, you can see the many filtering options available. In this article, we will look into some of these and will also show a function I've created to simplify the AMI search.

AMIs are virtual appliances used to create instances in AWS. The best way to describe an AMI is as a template used to create a virtual machine in Amazon's Elastic Compute Cloud (EC2). A single AMI can serve to launch one or thousands of instances. There are many choices when it comes to finding an AMI, ranging from vendors, the public, and Amazon themselves to name a few.

[4sysops article continues here...](https://4sysops.com/archives/aws-ami-discovery-with-powershell/)
