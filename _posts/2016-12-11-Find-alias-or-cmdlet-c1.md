
---
layout: post
title:  "What's my Alias? Write less for the same results"
date:   2016-12-11 13:00
comments: true
description: "What's my Alias? Write less for the same results"
categories: 
    - PowerShell
tags: 
    - PowerShell
---

PowerShell Cmdlets are by nature verbose. When you write a script those helpful cmdlets make a script easier to understand.
But when working with the shell interactively, its useful and quicker to adopt less is more approach. Well, if you are like me
and want to type less !

Take a simple example:
```
get-content 'D:\temp\machinelist.csv' | select-string -SimpleMatch "Server"
```
The commandline above gets the content of a csv file with a list of machines, pipes it to select-string to search for text patterns
for 'server'. 

If we uses Aliases for cmdlets then we dramitically save typing. Lets take a look:
```
gc 'D:\temp\machinelist.csv' | sls "Server"
```

Great, but how do we go about finding available aliases ? There is the 'Get-Alias' cmdlet. 
I wanted to make it even easier though to find an alias and even reverse this and find the cmdlet name. You see a something on the
internet and wonder what it is. What cmdlet is the alias of 'saps' ?

I came up with this short function to find either of these quickly:

```PowerShell
Function Get-AltName ($CmdletToAlias,$AliasToCmdlet) {
# Alternate name: Alias or cmdlet         
    if ($CmdletToAlias) {
        $Alias = Get-Alias | where {$_.ResolvedCommandName -eq "$CmdletToAlias"} | select @{Name="Alias Name";Expression={$_.name}}
            if ($Alias -eq $Null) { "No Alias Found" }Else{ Return $Alias }     
    }else{       
        $Cmdlet = get-alias | where {$_.Name -eq "$AliasToCmdlet"} | select @{Name="Cmdlet Name";Expression={$_.ResolvedCommand}}
            if ($Cmdlet -eq $Null) { "No Cmdlet Found" }Else{ Return $Cmdlet }      
    }
}
```

I called it 'Get-AltName' name as, get alterntive name. 

Examples:

```
Get-AltName -CmdletToAlias select-string

Alias Name
----------
sls
```

and...

```
Get-AltName -AliasToCmdlet sls

Cmdlet Name
-----------
Select-String
```

This script can be downloaded from my github page.
[](https://github.com/Graham-Beer/Find-Alias-or-Cmdlet)
