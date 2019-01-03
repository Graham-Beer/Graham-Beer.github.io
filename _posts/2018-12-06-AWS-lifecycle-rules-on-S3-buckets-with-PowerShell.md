---
layout: post
title: "AWS lifecycle rules on S3 buckets with PowerShell"
date: 2019-01-03 09:00
comments: true
description: "AWS lifecycle rules on S3 buckets with PowerShell"
categories:
    - PowerShell
    - AWS
tags:
    - PowerShell
    - AWS
---

One of the features Amazon Simple Storage Service (S3) provides to storage buckets is lifecycle rules. You can use a lifecycle rule to remove or archive objects. I will be looking at creating a lifecycle rule with PowerShell to remove objects from a S3 bucket.

Creating a lifecycle rule is a good way to automate the housekeeping of your S3 bucket. By using lifecycle rules, you can remove objects after so many days, by a certain date, or use Amazon's S3 Glacier for archiving. It's worth noting that you can set lifecycle rules by bucket, prefix, or tags, and you can also set them for current and non-current (previous) versions. I will demonstrate a simple example of removing all objects by a specified date.

[4sysops article continues here...](https://4sysops.com/archives/aws-lifecycle-rules-on-s3-buckets-with-powershell/)
