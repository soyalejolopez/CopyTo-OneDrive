#CopyTo-OneDrive
## Summary
Script to copy content like HomeDrive and Known Folders to OneDrive for Business. 

If you're migrating file shares sitting on a file server running SMB 2.0+ , then I recommend first starting with Microsoft FastTrack. This is a great service benefit that's available as part of your subscription. [Check here for more info.](https://technet.microsoft.com/en-us/library/mt651702.aspx)    

If however you also need to help end users migrate content on their PCs and want an end user driven migration, this script can help facilitate that.The script can be made available to your end users so they can run locally and copy/sync their content from their HomeDrive File Share or Known Folders (My Documents, Desktop, Pictures, etc.) to their local OneDrive for Business location.   

Benefits of doing an end user driven migration using the sync client:   
1. Decentralized model allows for more data to be migrated and migration traffic is dispersed   
2. Migrate files up to 15GB 
3. Take advantage of the new 400 path character capability
4. Exclude file types like .PSTs or .BAK files (note you'll need to also exclude the file types from the OneDrive Admin Center"
5. Only migrate files that are newer than X date
6. New special characters are supported (# and %). Note this needs to be enabled in your tenant. 
7. Only need to traverse your network once. Instead of migrating from a file share to OneDrive and then having users to synchronize that data back down. 
8. You can setup the OneDrive GPO bandwidth policy to limit how much bandwidth is used (prevent taking down your network). This can be used in conjunction with QoS policies. 

## Applies To
* OneDrive for Business in Office 365

## Pre-requisites
* Powershell v3+

* OneDrive for Business sync client (OneDrive.exe) needs to be installed and configured. Check here for guidance: https://aka.ms/odsync . Also check out the "Prepare Rollout" section below. 

## Disclaimer
THIS CODE IS PROVIDED AS IS WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESS OR IMPLIED, INCLUDING ANY IMPLIED WARRANTIES OF FITNESS FOR A PARTICULAR PURPOSE, MERCHANTABILITY, OR NON-INFRINGEMENT.

# How it works 
![Alt text](https://github.com/alejandr0x0/CopyTo-OneDrive/blob/master/Screenshots/Copyto-Onedrive.png?raw=true "CopyTo-OneDrive")  

## Script Option 1:  
This will use Robocopy to copy the contents of the folder selected to the local OneDrive sync folder. The OneDrive sync folder must be in the default location.  

## Script Option 2:   
This will use the SHSetKnownFolderPath function to redirect the folders to the OneDrive location. This will allow users to seamlessly backup any of their "new" content to OneDrive.
Because the existing content will remain the previous, default location, the script will also copy the existing to the OneDrive location using Robocopy. 

## Script Option 3:   
Performs both Option 1 and Option 3  

## Script Option 4:  
This is to undo the redirect of known folders done in step 2.  

## Script Option 5:  
Exit the script.  

## Want to customize the copy settings?   
Check the variables region with the script and update those copy settings. Here's a quick look at what those look like:    
```
$copysettingExcludeFileTypes = "/xf *.pst" #Example: "/xf *.pst *.bak" 
$copysettingExcludeLongPaths = "/256"  #Example: "/256" 
$copysettingExcludeFilesLargerThanXBytes = "/MAX:2000" #Example: "/MAX:2000"
$copysettingExcludeFilesOlderThan = "/MAXAGE:20170101"  #Example: "/MAXAGE:20170101"
```  

# Need to do a large scale deployment?  
## Prepare the rollout
1. Provision the OneDrive for Business site for users [PowerShell script](https://technet.microsoft.com/en-us/library/dn800987.aspx)
2. Install the [Next Gen Sync Client](https://support.office.com/en-us/article/Get-started-with-the-new-OneDrive-sync-client-in-Windows-615391c4-2bd3-4aae-a42a-858262e42a49)
3. Configure the sync client settings
* [Set the local sync folder](https://support.office.com/en-us/article/Use-Group-Policy-to-control-OneDrive-sync-client-settings-0ecb2cf5-8882-42b3-a6e9-be6bda30899c?#defaultrootdir)
* [Restrict changing the local sync folder location](https://support.office.com/en-us/article/Use-Group-Policy-to-control-OneDrive-sync-client-settings-0ecb2cf5-8882-42b3-a6e9-be6bda30899c?#disablecustomroot)
* [(Optional) Disable Personal Sync](https://support.office.com/en-us/article/Use-Group-Policy-to-control-OneDrive-sync-client-settings-0ecb2cf5-8882-42b3-a6e9-be6bda30899c?#disablepersonalsync)
* [(Optional) Throttle the bandwidth usage for uploads](https://support.office.com/en-us/article/Use-Group-Policy-to-control-OneDrive-sync-client-settings-0ecb2cf5-8882-42b3-a6e9-be6bda30899c?#maxbandwidth)
* [(Optional) Block file types from sync like PST](https://support.office.com/en-us/article/Block-syncing-of-specific-file-types-7d7168dd-9015-4245-a971-61b504f834d6)
* [(Optional) Set update ring](https://support.office.com/en-us/article/Use-Group-Policy-to-control-OneDrive-sync-client-settings-0ecb2cf5-8882-42b3-a6e9-be6bda30899c?#enableenterpriseupdate)
* [(Optional) Redirect Known Folders](https://support.office.com/en-us/article/Redirect-known-folders-to-OneDrive-for-Business-e1b3963c-7c6c-4694-9f2f-fb8005d9ef12)  
** Note: The script does this redirect of known folders as well if you prefer to give this control to the end user instead of deploying a group policy to users. 

## Communications and rollout
1. Communicate instructions on how to copy/sync their content 
2. Inform them by when they need to complete this. If migrating home drive set it to read-only after that deadline for 2wk period and then decommission. 
3. Provide support channels

How you communicate to your end users will be up to you. Potential options could be: 
* Setup a migration portal in SPO with Flow Templates to send out emails to users with migration instructions including link to script.
* Send an email with link to script to end users who should start getting their content migrated
* Setup a wiki and email end users a link to the wiki
* Send an email with a PDF that has instructions on how to migrate their content

## Monitor and wrap up
* Have users remediate sync client errors due to long file paths, extra large files, etc.
* Monitor the migration using the [OneDrive for Business Usage Report](https://support.office.com/en-us/article/Office-365-Reports-in-the-new-Admin-Center-OneDrive-for-Business-usage-0de3b312-c4e8-4e4b-a02d-32b2f726a680)

# Contributing
If you want to contribute and/or provide feedback, please reach out to:

Alejandro Lopez - Alejanl@microsoft.com  


