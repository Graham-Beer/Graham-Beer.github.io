---
layout: post
title: "Displaying PowerShell members with my Format-Member function"
date:   2018-09-04 09:00
comments: true
description: "Displaying PowerShell members with my Format-Member function"
categories:
    - PowerShell
tags:
    - PowerShell
---

With the help of my Format-Member function, you can display members from PowerShell objects in a formatted way. This article also introduces the member concept in PowerShell.

As you probably know, PowerShell is an object-oriented programming (OOP) language. An object may contain data in the form of fields or properties and code to perform an action called a method. Properties and methods are called members.

Using the Get-Member cmdlet lets you view the structure of an object. All objects in PowerShell are wrapped around PSObject, giving us the capability to view and extend members. If you wish to extend an object, you would generally use the Add-Member cmdlet. Objects you extend with other objects are called synthetic members, named so because they are not part of the original object.

[4sysops article continues here...](https://4sysops.com/archives/displaying-powershell-members-with-my-format-member-function/)
