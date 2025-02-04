---
layout: default  
title: Using MDT to automate Windows installation over the network
---
[Home](../../../../index.md) | [Projects](../../../..//projects/index.md) | [Troubleshooting](../../../../troubleshooting/index.md)

# Why MDT?
I chose MDT (Microsoft Deployment Toolkit) to set up and deploy multiple versions of Windows with different configurations over the network. Using MDT is easier than creating multiple unattended.xml files and using WDS.

# Configuring MDT
The latest version of Windows Automated Installation Kit (WAIK) does not include a 32-bit Windows PE image. I had to uncheck "x86" in the deployment share's properties, or else updating the deployment share would fail.

After doing that, I then clicked on `Operating Systems` on the left. I went through the import operating system wizard in which I chose the directory that contained the Windows installation files and imported them for use with MDT. Once the setup files had been imported, I created a test task sequence with default settings. I then updated the deployment share which generated a WIM file that I can add to Windows Deployment Services for networking booting.

## Testing initial MDT Setup
Once the WIM file had been added Windows Deployment Services. I created a new VM and network booted into the WIM file created by MDT. Once I had booted into the setup environment, I then followed the on-screen prompts and inserted the server's Administrator username, password, and the server's domain. Then I chose a task sequence and kept clicking next until setup began installing Windows.

## Issue encountered while configuring intial MDT setup

### No valid setup files
MDT would not detect my Windows installation files as valid. I checked the sources folder where the main setup files are stored. Instead of finding install.wim, the Media Creation Tool (MCT) had created install.esd. Thankfully this was an easy fix.

1. Copied the setup files from the ISO that MCT created
2. Opened CMD
3. Ran the following command to get the image index of `Windows 11 Pro`:
   ```batch
   dism /get-wiminfo /wimfile:C:\Users\Administrator\Desktop\ISO\sources\install.esd
   ```
4. Converted the esd to a wim file by running 
    ```batch
    dism /export-image /sourceimagefile:C:\Users\Administrator\Desktop\ISO\sources\install.esd /SourceIndex:6 /DestinationImageFile:C:\Users\Administrator\Desktop\ISO\sources\install.wim /Compress:Max /CheckIntegrity
    ```