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

```PowerShell
    .Synopsis
       Find where an application has been deployed to.
    .DESCRIPTION
       To determine which collection an application has been deployed to. Results will show Name of application, the deployment ID
       and the collection the application has been deployed to.
    .EXAMPLE
       get-AppDeployedToCollection -Application 'Microsoft Office 2016'
    .OUTPUTS
        Application_Name                                                               DeploymentID                                                   Deployed_To_Collection                                                      
        ----------------                                                              ------------                                                  ----------------------                                                      
       Microsoft Office 2016                                           {49B9BBF1-5D60-4633-A3DE-D43693G52921}                                       Current_Office_Collection                                                                       

    .NOTES
       General notes
    .COMPONENT
       This tool is used with SCCM. The WMI queries the SCCM Namespace and classes.
    .ROLE
       The role this cmdlet belongs to SCCM
    .CREATOR
       Graham Beer
#>

Function get-AppDeployedToCollection {

Param
    (
[Parameter(Mandatory, 
    ValueFromPipeline=$true,  
    Position=1,
    ParameterSetName='Define Application')]
[ValidateScript({(Get-CimInstance -Namespace Root\SMS\Site_*** -query "select * from SMS_application where LocalizedDisplayName like '$_'")})]             
[string[]]$Application        
    )

Begin 
{
$query = @{
    Namespace='ROOT\SMS\Site_**'
    Query="SELECT DeploymentID,TargetName,CollectionName FROM 
           SMS_Deploymentinfo
           WHERE 
           SMS_Deploymentinfo.DeploymentID IN 
          (SELECT DeploymentID 
           FROM SMS_Deploymentsummary 
           WHERE 
           SMS_Deploymentsummary.SoftwareName = '$Application')" }

$AllFoundUpdates = @() #Create empty array
}#End of Begin block

process
{
#Run the WQL query
$Results = Get-CimInstance @query

if($Results.count -gt 1){Write-host -ForegroundColor yellow "`nThere are $($Results.count) instances of $($Results.TargetName[0])"}
    else
        {<#Say nothing!#>}

#use foreach as Application could be deployed to more than one collection
foreach ($Result in $Results){

#Create custom object
$UpdateObj=[pscustomobject]@{"Application_Name"="";"Deployed_To_Collection"="";"DeploymentID"=""}
    
    $UpdateObj.Application_Name = $Result.TargetName
    $UpdateObj.Deployed_To_Collection = $result.CollectionName
    $UpdateObj.DeploymentID = $result.DeploymentID 

    $AllFoundUpdates += $UpdateObj #Add results to array

    }
}#End of Process block

End
{
#Display results
Return $AllFoundUpdates 

}#End of End block

}#End of Function
```

The first part of the script, the Parameter block, enforces the requirement of an application name. I've used 'ValidateScript' advanced function to confirm the application exists before carrying on with the script. Inside the $query variable I've used the splatting technique to set the namespace and the WQL query. Make sure you change the last three letters of the namespace with your own site for the script to work.
At this point I create an empty array to put the results in at the end. I run the WQL query and store the results in the variable $Results. The last part of the script uses the foreach command. So if the query brought back two deployments the foreach command would process them individually and add to the array. By creating a custom object I can organise my data better and finally pass the results to the array to be displayed.

I hope you find this useful. Please let me know your thoughts !
