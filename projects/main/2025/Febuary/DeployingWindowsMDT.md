---
layout: default  
title: Using MDT to automate Windows installation over the network
---
[Home](../../../../index.md) | [Projects](../../../..//projects/index.md) | [Troubleshooting](../../../../troubleshooting/index.md)

# Why MDT?
I chose MDT (Microsoft Deployment Toolkit) to set up and deploy multiple versions of Windows with different configurations over the network. Using MDT is easier than creating multiple unattended.xml files and using WDS.

# Configuring MDT
The latest version of Windows Automated Installation Kit (WAIK) does not include a 32-bit Windows PE image. I had to uncheck "x86" in the deployment share's properties, or else updating the deployment share would fail.

After doing that, I then clicked on `Operating Systems` on the left. I went through the import operating system wizard in which I chose the directory that contained the Windows installation files and imported them for use with MDT. Once the setup files had been imported, I created a task sequence with default settings to ensure I had set everything up correctly. I then updated the deployment share which generated a WIM file that I can add to Windows Deployment Services for networking booting.

## Testing Initial MDT Setup
Once the WIM file had been added to Windows Deployment Services, I created a new VM and booted it over the network using the WIM file created by MDT. Once I had booted into the setup environment, I then followed the on-screen prompts and inserted the server's Administrator username, password, and the server's domain. Then I chose a task sequence and kept clicking next until setup began installing Windows.

## Issue encountered while configuring initial MDT setup

### No valid setup files
MDT would not detect my Windows installation files as valid. I checked the sources folder where the main setup files are stored. Instead of finding install.wim, the Media Creation Tool (MCT) had created install.esd. Thankfully this was an easy fix.

1. Copied the setup files from the ISO that MCT created
2. Opened CMD
3. Ran the following command to get the image index of `Windows 11 Pro`:
   ```batch
   dism /get-wiminfo /wimfile:C:\Users\Administrator\Desktop\ISO\sources\install.esd
   ```
4. Converted the ESD to a WIM file by running 
    ```batch
    dism /export-image /sourceimagefile:C:\Users\Administrator\Desktop\ISO\sources\install.esd /SourceIndex:6 /DestinationImageFile:C:\Users\Administrator\Desktop\ISO\sources\install.wim /Compress:Max /CheckIntegrity
    ```

## Creating Task Sequences
A task sequence in the Microsoft Deployment Toolkit allows administrators to customize parts of the Windows Setup, such as what optional features are installed, whether Windows checks for updates and installs any additional programs.

## Windows 11/10
To spin up a fully updated Windows 10 or 11 virtual machine, these are the task sequences I have chosen to add/enable.

### Tasks
* Windows Update (Pre-Application Installation)
    * This searches for, and installs all Windows Updates before moving on to installing applications.
* Install Applications
    * VMware Tools has been added to this part of the task sequence installing with silent flags. VMware tools enhances the virtual machine's performance.
* Install Roles and Features
    * By default, this task is not added so using the Task Sequencer I manually added it under "Custom Tasks". I have enabled `.NET Framework 3.5` as some apps that I test require the use of older .NET frameworks.

## Windows 7
Without additional tweaking, Windows 7 cannot access the Windows Update servers. As a result, the task sequence is almost identical to Windows 10/11 but without Windows Update being ran