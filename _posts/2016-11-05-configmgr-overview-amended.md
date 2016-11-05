---
layout: post
title: "ConfigMgr overview: quick view of servers and roles"
date: 2016-11-05
comments: true
description: "ConfigMgr overview: quick view of servers and roles"
categories:
    - PowerShell
tags:
    - PowerShell
    - SCCM
---

If you have a reasonable sized Configuration Manager environment, then quickly displaying servers, with a list of roles installed and 
the number roles for each site is not easy. 

This sort of information can be useful to your manager, a 3rd party coming in to do a review or walking into a new job.
This script I created will display this information for you and quickly. Running the script has two options:

**1.** Get-CMSiteOverview : This will display all the data in the 'SMS_SCI_SysResUse' class  name, unformatted. This will give you the option to choose what you select by piping to a select-object, Where-object type filter. 

```powershell
(i.e. Get-CMSiteOverview | select NetworkOSPath, RoleName)
```

**2.** Get-CMSiteOverview -format : The '-format' is a switch which will format the Servers, roles and role count in a table format for you. This is the quick no fuss approach to viewing your environment...quickly !

I hope you find this a useful tool to check out your environment. 

<script src="https://gist.github.com/Graham-Beer/bf9a4096e4e02fc3cbbdf30368ca6f06.js"></script>
