---
layout: post
title: "Deployment, What's my status?!"
date: 2016-11-05
comments: true
description: "Deployment, What's my status?!"
categories:
    - PowerShell
    - SCCM
tags:
    - PowerShell
    - SCCM
---

Deployments...as an SCCM administrator you are always deploying something ... 

be it applications or software updates or even a task sequence. Through the console you go into monitoring, select deployments .... 
find what you have deployed...then go through and check status to see what targeted devices have been successful or even failed. Maybe 
you make a note of the failed devices, but this isn't practical for lots of machines and certainly isn't the best way to spend your 
day !By using PowerShell I can access the WMI class 'SMS_AppDeploymentAssetDetails' and get the status of an application. There are five status that an application could be in. They are:

**Success –** The application deployment succeeded or was found to be already installed.
**In Progress –** The application deployment is in progress.
**Unknown –** The state of the application deployment could not be determined. This state is not applicable for deployments with a purpose of Available. This state is typically displayed when state messages from the client are not yet received.

**Requirements Not Met –** The application was not deployed because it was not compliant with a dependency or a requirement rule, or because the operating system to which it was deployed was not applicable.

**Error –** The application failed to deploy because of an error. 

##The script:

<script src="https://gist.github.com/Graham-Beer/4c66ceb5340f85a2acf664be466d8b63.js"></script> 

An application could be deployed to more than a one collection so along 
with the application name, I will need to specify the targeted collection. So far this gets me every status of the application deployed 
to a collection. But it would be nice to get back a chosen status.As there are a potential of five possible states, I’ve used the 
advanced parameter, ValidateSet. A great parameter to set some defined options. I will pass one of the five options to a 'where' clause 
to filter out the status. I've not made it mandatory, so if you want to see all, simply leave it out.Remember to add your site code to  
the namespace, 'root\SMS\site_***'.  

The beauty of this script is you can leverage some PowerShell features to get a count or pass the data to a CSV file. Maybe even create a collection in SCCM to pass failed clients into. (I might make a function to do this and write in a future blog.) 

![](/images/Blog_Pictures/5309265_orig.png)  

I hope you find this script useful and as always with the goal of automation, save valuable time !
