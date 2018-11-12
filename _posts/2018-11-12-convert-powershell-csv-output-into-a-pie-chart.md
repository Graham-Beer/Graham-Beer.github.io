---
layout: post
title: "Convert PowerShell output into a pie chart"
date:   2018-11-12 09:00
comments: true
description: "Convert PowerShell output into a pie chart"
categories:
    - PowerShell
tags:
    - PowerShell
---

The idea to display information as a pie chart came about when I needed to write a summary email about a task I had done with PowerShell. I had the results captured in a CSV file and was about to hit send, when I thought: how quick and easy would it be to interpret this information as a visual chart?

Opening a CSV file, going to the bottom to look at the totals of successful and failed or enabled and disabled settings might only take a minute or so. But how much easier is it to view this information as a pie chart?

So I had the idea, how could I do this in PowerShell? Working with PowerShell 5.1, I have the full capabilities of .NET, so you can be sure someone has done this already, be it in C#, VB.NET, or even PowerShell. Initially I wanted to create a solution in Windows Presentation Foundation (WPF), but this wasn't easy. However, I found a way using the Winforms assembly System.Windows.Forms.DataVisualization.

To continue reading please follow the below link.

[4sysops article continues here...](https://4sysops.com/archives/convert-powershell-csv-output-into-a-pie-chart/)
