---
layout: post
title: "Where has my Application been Deployed to?"
date: 03/11/2016  
comments: true
description: "Where has my Application been Deployed to?" 
categories: 
    - PowerShell
    - SCCM
    - WMI
tags: 
    - PowerShell
    - SCCM
---

First off, welcome to my blog. As my blog develops you will see i'm always looking for a way to do things a bit quicker and hopefully easier!

So some background. I work for a medium size company using SCCM 2012 R2 SP1 with many applications from in-house developed to 3rd party. Our software library is separated into folders of application suppliers, i.e. Microsoft, Adobe. Many of these applications have some weird and wonderful suppliers making it a little time consuming finding the one you want to look over. Sure, I can navigate into software library and do a search on subfolders for the application name. But with PowerShell this task can be done much quicker.
To continue on the background on why I created the below script, our company deploy most applications through AD groups. So its not unusual for an application to be deployed to a few collections. (The AD group collection and a test collection.) 
The script I wrote taps into Configuration Manager namespace in WMI. A lot of the "out of the box" cmdlets are good, but there is a wealth of data in WMI.  

<script src="https://gist.github.com/Graham-Beer/241db1c0f627fb4a343f0567b0323dde.js"></script>

The first part of the script, the Parameter block, enforces the requirement of an application name. I've used 'ValidateScript' advanced function to confirm the application exists before carrying on with the script. Inside the $query variable I've used the splatting technique to set the namespace and the WQL query. Make sure you change the last three letters of the namespace with your own site for the script to work.
At this point I create an empty array to put the results in at the end. I run the WQL query and store the results in the variable $Results. The last part of the script uses the foreach command. So if the query brought back two deployments the foreach command would process them individually and add to the array. By creating a custom object I can organise my data better and finally pass the results to the array to be displayed.

I hope you find this useful. Please let me know your thoughts !
