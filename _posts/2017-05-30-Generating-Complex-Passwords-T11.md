---
layout: post
title:  "Generating Complex Passwords"
date:   2017-05-31 09:10
comments: true
description: "Script to generate a single or multiple complex passwords"
categories: 
    - PowerShell
tags: 
    - PowerShell
---
This is a handy little script to create a single or multiple complex passwords. 
I found the need on a recent project to generate and pass a list of random complex passwords for many new users
to the admin team. 
 By making use of System.Web.Security.Membership class I could call the static method GeneratePassword, which takes two
integer parameters. Refering to the MSDN documentation, it explains the usage of both parameters and the output value:
 
***Length***  
**Type:** System.Int32  
> The number of characters in the generated password. The length must be between 1 and 128 characters. 

***numberOfNonAlphanumericCharacters***  
**Type:** System.Int32  
> The minimum number of non-alphanumeric characters (such as @, #, !, %, &, and so on) in the generated password. 

***Return Value***  
**Type:** System.String   
> A random password of the specified length.

To generate a single password simple pass the password length and number of special characters required:  

    New-ComplexPassword -PasswordLength 8 -SpecialCharCount 1

To generate multiple passwords, pass strings or integers across the pipeline:  

    PS > 1,2,3,4 | New-ComplexPassword -PasswordLength 16 -SpecialCharCount 5
    PS > 'John','Paul','George','Ringo' | New-ComplexPassword -PasswordLength 10 -SpecialCharCount 2


----------

    Function New-ComplexPassword {
    
    [Cmdletbinding(DefaultParameterSetName='Single')]
        Param(
        [Parameter(ParameterSetName='Single')]
        [Parameter(ParameterSetName='Multiple')]
        [Int]
        $PasswordLength,
        
        [Parameter(ParameterSetName='Single')]
        [Parameter(ParameterSetName='Multiple')]
        [int]
        $SpecialCharCount,

        [Parameter(ValueFromPipeline=$true,
                   ValueFromPipelineByPropertyName=$true,
                   ParameterSetName='Multiple')]
        [String[]]
        $GenerateUserPW
        )
    Begin {   
        # The System.Web namespaces contain types that enable browser/server communication
        Add-Type -AssemblyName System.Web 
    }
    Process {
        switch ($PsCmdlet.ParameterSetName) {
            'Single' {
                # GeneratePassword static method: Generates a random password of the specified length
                [System.Web.Security.Membership]::GeneratePassword($PasswordLength, $SpecialCharCount)
            }
            'Multiple' {
                $GenerateUserPW | Foreach {
                    # Custom Object to display results
                    New-Object -TypeName PSObject -Property @{
                        User = $_
                        Password = [System.Web.Security.Membership]::GeneratePassword($PasswordLength, $SpecialCharCount)
                   }
                }
            }
        } # End of Switch
    }
    } # End of Function


As always script is on my GitHub pages.

[Code Link on GitHub](https://github.com/Graham-Beer/New-ComplexPassword.git)

