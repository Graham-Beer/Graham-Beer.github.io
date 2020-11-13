---
layout: post
title: "Encrypt in AWS using the Key Management Service (KMS)"
date: 2020-10-28 09:00
time_to_read: true
comments: true
description: "Encrypt in AWS using the Key Management Service (KMS)"
categories:
    - AWS
    - KMS
tags:
    - AWS
    - KMS
---

The AWS Key Management Service (KMS) allows you to create and manage cryptographic keys that you can use across a wide range of services in Amazon's cloud and your applications. We will walk through an example of encrypting your files in S3 by using KMS.

### How secure are KMS keys?
AWS KMS is a fully managed service and will ensure the security of your keys. AWS provides server-side encryption of your data. When you send unencrypted, raw data to AWS, the AWS infrastructure will encrypt this data and then store it to disk. When you need to retrieve the data, AWS will read and decrypt it before sending it back to you. As the user of AWS, you do not see this process happening; it all happens under the hood.

[4sysops article continues here...](https://4sysops.com/archives/encrypt-in-aws-using-the-key-management-service-kms/)
