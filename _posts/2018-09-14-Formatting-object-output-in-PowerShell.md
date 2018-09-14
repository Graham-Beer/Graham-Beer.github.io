---
layout: post
title: "Formatting object output in PowerShell with Format.ps1xml files"
date:   2018-09-14 09:00
comments: true
description: "Formatting object output in PowerShell with Format.ps1xml files"
categories:
    - PowerShell
tags:
    - PowerShell
---

PowerShell uses Format.ps1xml files to set the default display of objects to the console, and you can create or change your own Format.ps1xml files for your own object types. This article will provide background on how PowerShell uses Format.ps1xml files and how to define your own custom format file.

PowerShell is a type-based system. It assigns a type to constructs like variables, expressions, functions, or modules, which provides a set of rules. Types help display information, but a normal object doesn't know how to display itself. To resolve this, PowerShell uses something called the Extended Type System (ETS). The ETS gives the ability to add additional dynamic members to types and objects. The ETS area we're looking at in this article is formatting the displayed output.

[4sysops article continues here...](https://4sysops.com/archives/formatting-object-output-in-powershell-with-format-ps1xml-files/)
