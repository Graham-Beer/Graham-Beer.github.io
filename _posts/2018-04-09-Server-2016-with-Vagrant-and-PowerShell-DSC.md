---
layout: post
title:  "Provision Windows Server 2016 with Vagrant and PowerShell DSC"
date:   2018-04-09 09:00
comments: true
description: "Provision Windows Server 2016 with Vagrant and PowerShell DSC"
categories: 
    - PowerShell
    - Vagrant  
tags: 
    - PowerShell
    - Vagrant 
---

If you have not heard of or used Vagrant before, then it's well worth checking out some of the excellent blogs here on 4sysops by Adam Bertram. 
What is so good about Vagrant is that there are many ways to provision a machine, Ansible, Chef, Puppet, and shell scripting to name just a few.

The ability to provision with shell scripts allows us to use all the PowerShell language, including PowerShell DSC. PowerShell DSC is a 
declarative platform we can use to configure and manage systems.

There really are only two parts to this: the DSC configuration and the Vagrantfile to set up and provision the machine. In this example, I've created a 
DSC configuration to install PowerShell Core and OpenSSH on a Windows Server 2016. I will also be making use of the Vagrant boxes available 
from the Vagrant site.

[4sysops article continues here...](https://4sysops.com/archives/provision-windows-server-2016-with-vagrant-and-powershell-dsc/)
