---
layout: post
title:  "The Adam Bertram Pester Challenge!"
date:   2017-10-03 14:00
comments: true
description: "My thoughts on learning Pester"
categories: 
    - PowerShell
    - Pester
tags: 
    - PowerShell
    - Pester
---

During September Adam Bertram set a challenge, to learn Pester.   
Here is what Adam said: 

> Do you want to learn about testing PowerShell with Pester?  
> Have you been putting it off just because you “don’t have time”?  

> I’m formally putting out the challenge to anyone that is willing to dedicate some serious time to learning Pester and is willing to share their story about it. I will send anyone a FREE coupon for the Pester Book if you can guarantee you will submit your story and your first set of real tests.  

[Adam's website link](http://www.adamtheautomator.com/Pester-book-challenge/)  

Pester has been on my to-do list for a while. As DevOps is now stronger than ever, the need to add tests to your pipeline is crucial. I needed to get cracking and Adam's challenge gave me the perfect reason to.

The code I decided to write a Pester test for was a function that took input from a CSV file containing a list of UserPrincipalNames.  The 2nd parameter took a target OU group which would be used to move users to.
Here is the function I'm writing the test for:  

```powershell
Function Move-ADUserToTargetOU {
    
    [cmdletbinding()]
    param (
        [parameter(Mandatory = $true, ValueFromPipelineByPropertyName = $True)]
        [ValidateScript( { Test-Path $_ } )]
        [string]$Path,

        [parameter(ValueFromPipelineByPropertyName = $True)]
        [string]$OUGroup
    )

    Begin {
        Write-Verbose -Message "[BEGIN  ] Function to move Users to '$OUGroup' OU"
    }

    Process {
        $csv = @{
            Path     = $Path 
            Encoding = 'UTF8' 
            Header   = 'UserPrincipalName'
        }

        # import CSV and set header
        $UPNlist = Import-Csv @csv | Select-Object -Skip 1

        # Obtain Forest
        $forest = (Get-ADForest).name

        foreach ($upn in $UPNlist.UserPrincipalName) {
    
            # Get User Parameters
            $params = @{
                Identity    = $upn
                Server      = "${forest}:3268"
                ErrorAction = 'Stop'
            }

            # Set Get-ADOrganizationalUnit Parameters
            $targetOUParams = @{
                Filter      = { Name -eq $OUGroup } 
                ErrorAction = 'Stop'
            }
        
            # Find User
            Try {
                Write-Verbose -Message "[PROCESS] Getting User details from Active Directory"
                $ADAccount = Get-ADUser @params

                # Get child domain user is in with string manipulation on DistinguishedName
                $null, $targetOUParams.Server = $ADAccount.DistinguishedName -split 'DC=', 2 -replace ',DC=', '.'
                
                $UserOU = ($ADAccount.DistinguishedName -split ",", 2)[1]
                $targetOU = Get-ADOrganizationalUnit @targetOUParams

                if (-not $targetOU) {
                    throw 'Invalid OU, cannot move user.'
                } elseif (@($targetOU).Count -gt 1) {
                    throw 'Ambiguous OU, cannot move user.'
                } elseif ($targetOU.DistinguishedName -ne $userOU) {
                    $Move = @{
                        Identity    = $ADAccount.DistinguishedName
                        TargetPath  = $TargetOU
                        Server      = $targetOUParams.Server
                        ErrorAction = 'Stop'
                    }

                    # Perform Move
                    Write-Verbose -Message "[PROCESS] Moving user: $($ADAccount.UserPrincipalName)"
                    Move-ADObject @Move
    
                    Write-Verbose -Message "[PROCESS] Target Server: $domain"
                    Write-Verbose -Message "[PROCESS] User moved to Target OU: $($Move.TargetPath)"
                } else {
                    Write-Verbose -Message "[PROCESS] No changes were required: $($ADAccount.UserPrincipalName)"
                }
            } catch {
                Write-Error -Message $_.Exception.Message -ErrorAction Stop
            }
        }
    }
    
    End {
        Write-Verbose -Message "[END    ] Completed OU moves"
    }
}
```

Getting started in the world of testing can be mind blowing, so many types to get your head round!  
Pester is a unit testing framework built with PowerShell in mind. A unit test is the smallest testable part of an application like functions, classes, procedures, interfaces. Unit testing is a method by which individual units of source code are tested to determine if they are fit for use.

For me, getting my head around "What should I be testing" was one of the biggest challenges. To think of good tests, you REALLY need to understand your code. What is each bit doing?  
You need to handle if something does or does not work. So, for my case, what if the user is not in Active Directory? What if the user is in Active Directory? What if I pass an invalid file?  
I found slowly walking through my code and writing a list of things that relied on an output, helpful. The point of the tests is to verify that your function does indeed do as you want and expect.  

Once you get over working out what to test, you need to get to grips with Pester syntax. The most complicated concept to grasp is 'mocking'. Mocks, in Pester, are ways to force commands to return what we need them to, given the situation. So, we are giving fake returns to test various scenarios of our function. 

Working with smaller chunks of code certain helps. I've since started writing smaller functions which you can call together. This helps manage your code better and writing smaller Pester tests is much easier and less daunting! I found myself as well, taking more time on my original function to optimize its use better. Messy code is much harder to write tests for.   

No doubt about it, seeing the return of successful green output gives you a buzz!

This is the finished Pester test. (One thing to note, I’ve heavy commented the Pester test to help describe what I’ve done. I've found this useful going forward as a reference for my future tests):    

```powershell
Describe 'Move-ADUserToTargetOU' {   
    # Setup mock's first 
    # Mock's are overrided if added in a context or it block

    BeforeAll {
        Mock Get-ADForest {
            [PSCustomObject]@{
                Name = 'DomainName'
            }
        }

        Mock Get-ADOrganizationalUnit {
            [PSCustomObject]@{
                DistinguishedName = 'OU=something,DC=domain,DC=com'
            }
        } 

        Mock Get-ADUser {
            [PSCustomObject]@{ 
                DistinguishedName = 'CN=someone,OU=something,DC=domain,DC=root,DC=com' 
            }
        } 

        Mock Import-Csv {
            [PSCustomObject]@{
                UserPrincipalName = 'Header'
            }
            [PSCustomObject]@{
                UserPrincipalName = 'bob@domain.com'
            }
        }

        Mock Move-ADObject
        Mock Test-Path { $true }
    }

    Context 'Given path parameter' {    
        # Inherits the Mock of Test-Path { $true } from main BeforeAll block. Path is correct then 
        # should not throw an error
        It 'Should give a valid file path' {

            { Move-ADUserToTargetOU -Path TestDrive:\import.csv -OUGroup Something } | Should Not Throw
        }

        It 'Should given an invalid file path Then throw a terminating error' {
            # Add a mock to fail test-path which overrides the main BeforeAll block.
            Mock Test-Path { $false }            

            { Move-ADUserToTargetOU -Path TestDrive:\import.csv -OUGroup Something } | Should Throw 
        }
    }

    Context 'Given Parameter validation - User' {
        # Mocking Get-ADUser to throw a terminating error to test invalid user
        Mock Get-ADUser { throw 'Invalid user' }

        It 'Should give an invalid AD user, throws a terminating error' {
            { Move-ADUserToTargetOU -Path TestDrive:\import.csv -OUGroup Something } | Should Throw 'Invalid user'
            Assert-MockCalled Get-ADOrganizationalUnit -Times 0
            Assert-MockCalled Move-ADObject -Times 0
        }
    }

    Context "Given OU Checks" {
        It 'Should give an invalid TargetOU, does not call Move-ADObject' {
            # Mock an empty Get-ADOrganizationalUnit so TargetOU will have an invalid entry ( Null ) 
            Mock Get-ADOrganizationalUnit { }

            { Move-ADUserToTargetOU -Path TestDrive:\import.csv -OUGroup Something } | Should Throw 'Invalid OU, cannot move user.'
            Assert-MockCalled Move-ADObject -Times 0 -Scope It
        }

        It 'Should give an ambiguous TargetOU and not call Move-ADObject' {
            # Mock two DistinguishedName's in different child domains, but with same OU name. The results check in main
            # function should terminate if 2 or more with the same OU
            Mock Get-ADOrganizationalUnit {
                [PSCustomObject]@{
                    DistinguishedName = 'OU=something,OU=child1,DC=domain,DC=com'
                }
                [PSCustomObject]@{
                    DistinguishedName = 'OU=something,OU=child2,DC=domain,DC=com'
                }
            }
            
            { Move-ADUserToTargetOU -Path TestDrive:\import.csv -OUGroup Something } | Should Throw
            Assert-MockCalled Move-ADObject -Times 0 -Scope It
        }
    }

    Context 'Successful move' {
        It 'Should give a valid user and OU and it moves the user' {
            # Mock the the identity parameter
            Mock Get-ADUser -ParameterFilter { $Identity -eq 'bob@domain.com' } -MockWith {
                [PSCustomObject]@{ 
                    DistinguishedName = 'CN=someone,OU=something,DC=domain,DC=root,DC=com' 
                }
            } 

            { Move-ADUserToTargetOU -Path TestDrive:\import.csv -OUGroup Something } | Should Not Throw
           
            # Just passing an Assert-MockCalled without any parameters asserts it's been called one or more times.
            # For a certain number pass the -Times parameter and use switch 'Exactly' to confirm its only been run by the value in 'Times'
            Assert-MockCalled Get-ADUser -Times 1 -ParameterFilter { $Identity -eq 'bob@domain.com' } -Exactly
            Assert-MockCalled Get-ADOrganizationalUnit -Times 1 -Exactly
            Assert-MockCalled Move-ADObject -Times 1 -Exactly
        }
    }
} 
```  

I hope you found my thoughts on starting with Pester useful. Its a brilliant tool which makes you a better coder all round I believe.   

Both the function and the test is located in my GitHub, found here:  
[My GitHub Pester Challenge code](https://github.com/Graham-Beer/The-Pester-Book-Challenge)  