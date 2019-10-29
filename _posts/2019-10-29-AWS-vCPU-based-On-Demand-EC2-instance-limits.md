---
layout: post
title: "AWS vCPU-based On-Demand EC2 instance limits"
date: 2019-10-29 09:00
comments: true
description: "AWS vCPU-based On-Demand EC2 instance limits"
categories:
    - AWS
tags:
    - AWS
---

You may be aware that Amazon sets default limits on resources on a per-region basis. Currently there is a limit—sometimes known as a quota—on the number of Elastic Compute Cloud (EC2) instances you can launch in a region. As of October 24, 2019, Amazon Web Services (AWS) will change On-Demand EC2 instance limits, <https://aws.amazon.com/about-aws/whats-new/2019/09/vcpu-based-on-demand-instance-limits-are-now-available-in-amazon-ec2/>

## How is this changing?
Currently you have limits for each EC2 instance by type, limiting you to running a maximum of 20 On‑Demand instances across an instance family. Going forward, Amazon will limit On-Demand EC2 instances by virtual central processing units (vCPUs) only.

This change on how limits occur will now make it easier because you have just one limit to manage all of your EC2 instances. This helps remove the confusion of having limits for the many EC2 instances when it comes to working with such features as autoscaling.

[4sysops article continues here...](https://4sysops.com/archives/aws-vcpu-based-on-demand-ec2-instance-limits/)
