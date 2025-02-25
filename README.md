# Automatic Warranty retrieval and reporting
See https://www.cyberdrain.com/automating-with-powershell-automating-warranty-information-reporting/ for more information.

This is a PowerShell module that helps you get warranty information for items in your CMDB, PSA, or RMM. This module was made due to some shady businesses going on in the world of Warranty reporting and lookups, and to democtratize the warranty lookups. :)

# Installation instructions

This module has been published to the PowerShell Gallery. Use the following command to install:  

    install-module PSWarranty

# Supported Platforms

The following platforms are supported. You can also use a CSV file as a source to generate reports. This won't sync back to the source though. Some APIs don't allow write-back, so those are only available to create reports.

An unchecked box means development for this is underway

## PSA    
- [x] Autotask
- [x] Connectwise Manage
- [X] Halo PSA / ITSM / Service Desk

## Documentation Tools
- [x] IT-Glue
- [X] Hudu

## RMM
- [x] Solarwinds N-Able (Reporting only)
- [ ] Solarwinds RMM
- [ ] NinjaRMM (Reporting only)
- [ ] SyncroRMM
- [x] DattoRMM
- [ ] Connectwise Automate
- [x] BluetraitIO

# Supported Manufacturers
- [x] Dell (Requires API key)
- [x] Microsoft
- [x] HP (Spotty API. New one coming soon.)
- [x] Lenovo
- [x] Toshiba
- [x] Apple (Estimated dates)

# Apple Note
Due to a change in how Apple generates serial numbers it is no longer possible to accurately determine the warranty expiry for newer devices. As such any new Apple devices could return a completely inaccurate expiry date. To account for this you can add the -ExcludeApple switch to the the Update-Warrantyinfo.ps1 script to skip updating any apple devices and ensure you do not update with inaccurate data.

# HP Note
HP has taken down their warranty API for retooling until further notice. The re-release of which has been postponed and no ETA is currently available. As a result, HP device requests will timeout and return an error. While there are no adverse data affects as a result, a high number of HP devices in the source can lead to long execution times. To account for this you can add the -ExcludeHP switch to the the Update-Warrantyinfo.ps1 script to skip updating any HP devices.

# Usage
## Autotask
To execute an update of all devices in Autotask use:

    $Creds = get-credential  
    update-warrantyinfo -Autotask -AutotaskCredentials $creds -AutotaskAPIKey 'APIINTEGRATIONKEY' -SyncWithSource -OverwriteWarranty

This script will then update all devices in Autotask with the date found in the APIs. If they date returned is invalid it will not update. If the date is already filled in, it will overwrite this.

To only generate HTML reports use

    $Creds = get-credential  
    update-warrantyinfo -Autotask -AutotaskCredentials $creds -AutotaskAPIKey 'APIINTEGRATIONKEY' -GenerateReports

This will generate the reports in C:\Temp. To set the path yourself use

       update-warrantyinfo -Autotask -AutotaskCredentials $creds -AutotaskAPIKey 'APIINTEGRATIONKEY' -GenerateReports -ReportsLocation "C:\OtherFolder"

## Hudu
To update Hudu, first edit the asset layout of the devices you wish to update to add a new field. If you wish to use expirations and not have apple devices, choose a date field and enable add to expirations. If you wish to have apple devices you will need to make this a string field and not use expirations. Make a note of the name of the field and the name of the asset layout then call the script:

        update-warrantyinfo -Hudu -HuduAPIKey "YourAPIKey" -HuduBaseURL "YourHuduDomain" -HuduDeviceAssetLayout "Desktops / Laptops" -HuduWarrantyField "Warranty Expiry" -SyncWithSource -OverwriteWarranty -ExcludeApple 

## Halo PSA / ITSM / Service Desk
To update Halo you will first need to determine which field name you are using to store you serial numbers. This could be a base field such as "inventory_number", an asset field such as "Serial Number" or a custom field such as "CFSerialNumber". You can find the name by using the Halo Powershell Module and using Get-HaloAsset -FullObjects and inpecting the returned objects:

        update-warrantyinfo -Halo -HaloClientID "YourAPIAppClientID" -HaloClientSecret "YourHaloAPIAppSecret" -HaloURL "YourHaloURL" -HaloSerialField "Halo Serial Field Name" -SyncWithSource -OverwriteWarranty -ExcludeApple

## BluetraitIO
To execute an update of all devices in BluetraitIO use:

    update-warrantyinfo -BluetraitIO -BTAPIKEY "your_api_key" -BTAPIURL "https://example.bluetrait.io/api/" -SyncWithSource -OverwriteWarranty

# Contributions

Feel free to send pull requests or fill out issues when you encounter them. I'm also completely open to adding direct maintainers/contributors and working together! :) I'd love if we could make this into a huge help for our entire community.

# Future plans

Version 0.8 includes all things I required for myself, if you need a feature, shoot me a feature request :)
