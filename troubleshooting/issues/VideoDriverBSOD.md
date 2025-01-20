---
layout: default  
title: Attempting to fix a Blue Screen of Death (BSOD) when updating graphics drivers
---
[Troubleshooting](../index.md)

# Attempting to fix a Blue Screen of Death (BSOD) when updating graphics drivers

## Issue
When using recent Windows Insider builds, the system will crash when installing/updating the graphics drivers.

## Investigation
### Initial
When the system crashes an error code of `WIN32K_CRITICAL_FAILURE` would appear and the system would automatically reboot. When the system rebooted, I copied the dump file from `C:\windows\` to my desktop for analysis. After opening the dump file in `WinDBG`, I analysed the memory dump by running `analyse -v`. Eventually I came across `win32kfull!DDCIFreeMemory` in the stack trace which help me isolate what feature may be causing the issue.

#### What is DDCCI (DDC/CI)?
DDC/CI allows a computer to communicate to a monitor and lets the computer adjust various parts of the monitor, mainly volume and brightness.

## Fix / Workaround
1. Closed `Twinkle Tray` which is a program that lets me adjust the volume and brightness of my monitors
2. Reattempted to update the graphic drivers again
3. Reported the issue to Microsoft after confirming that the issue can be reproduced