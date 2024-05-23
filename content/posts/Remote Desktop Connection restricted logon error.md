+++
title = "Windows 10 Remote Desktop Connection Fix: The system administrator has restricted the types of logon (network or interactive) that you may use"
date = "2024-05-23"
author = "Jude Sudbury"
cover = "/20240523/rdp-fail.webp"
description = "Windows 10 Remote Desktop Connection Fix: The system administrator has restricted the types of logon (network or interactive) that you may use"
+++

A client of mine was attempting to access a Windows 10 Enterprise workstation from a laptop using RDP but encountered this error:

"The system administrator has restricted the types of logon (network or interactive) that you may use. For assistance, contact your system administrator or technical support"

![Screen shot of this error: The system administrator has restricted the types of logon (network or interactive) that you may use. For assistance, contact your system administrator or technical support](/20240523/rdp-error.png)

I checked the usual:

- Remote access is enabled
- The user account used for remote access is local is a member of the local administrators group
- He had turned off NLA and the firewall (for troubleshooting purposes)

All settings were correct. I eventually started digging around the local security policy (run secpol.msc or search for 'local security policy' in the windows menu) with the aim of checking that the 'Allow log on through Remote Desktop Services' had the administrators group listed. It did. While there, however, I noticed that under '**Deny** log on through Remote Desktop Services' there was an entry for 'local account'

**Find/ open local security policy app**

![Find/ open local security policy app.](/20240523/search-for-local-security-policy.png)

**Find Deny log on through Remote Desktop Services**

![Find Deny log on through Remote Desktop Services.](/20240523/local-sec-policy.png)

**Remove "Local account"**

![Remove "Local account".](/20240523/local-sec-policy-2.png)

That was it, problem solved! This security policy appears to be a default for the Enterprise edition of Windows, most likely because Microsoft expect the computer to be domain joined (this particular workstation was standalone). Note also that "Deny access to this computer from the network" also contained the Local account object.

