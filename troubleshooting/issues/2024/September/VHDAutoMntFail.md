---
layout: default  
title:  
---
[Troubleshooting](../../../index.md)

# Deleted VHD causing an error upon boot

## Issue
When booting my system, Windows said that it was unable to mount a VHD (Virtual Hard Disk) that I had previously deleted.

## Investigation
My first idea was to recreate the VHD using the same settings as the first VHD. However, after a reboot, the error was replaced with a new error saying that `The specified VHD has an unexpected virtual identifier.`. I then deleted the newly created file before checking the event log to see if I could gather any more information, but found no useful information. I decided to check the registry to see if there were any references to the VHD I had deleted. After using a third party tool to search through the registry, I came across two registry values that mentioned `AutoAttach` alongside the filepath.

![Registry values showing AutoAttach References](../../images/2024/September//VHDRegistryValues.jpg)
*Registry values showing AutoAttach references that caused the error*

## Fix
Deleting the two registry values listed in the image above and restarting the PC, the error message no longer appeared on boot.