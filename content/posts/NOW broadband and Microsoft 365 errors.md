+++
title = "NOW / Sky broadband and Microsoft 365 connectivity errors"
date = "2024-02-21"
author = "Jude Sudbury"
cover = "/20240221/character-sitting-in-front-of-a-computer.webp"
description = "NOW / Sky broadband and Microsoft 365 connectivity errors"
+++

Recently I've had three people from different households have weird errors while working from home. All involved connecting to OneDrive or attempting to save an Office document back to Sharepoint or OneDrive. The error thrown by (Excel in this example) is rather enigmatic:

"The server you are trying to access is using an authentication protocol not supported by this version of Office."

![The server you are trying to access is using an authentication protocol not supported by this version of Office.](/20240221/error_message.webp)

And when asked to sign out and back in to the Windows OneDrive app, this happened:

![OneDrive signing in hangs.](/20240221/one_drive_error.png)

One Drive refused to sign back in, just hanging forever with the words 'signing in' with a spinning circle.

All three reported no issues when back at the office. What did each of these people have in common? They are all using NOW broadband (Sky). As part of my troubleshooting, I ran IPconfig to check IP details, in particular to see what DNS servers they were using. However, the local IP returned for each was an [IPv6 address](https://en.wikipedia.org/wiki/IPv6). It's nice to see Sky advancing internet technology, however Microsoft doesn't appear to like it. I disabled IPv6 on each computer and all the errors disappeared immediatley and service was restored. Not even a reboot was required.

Since I wasn't able to log into any of my clients' routers to see whether it was possible to disable IPv6 at the source, I had to disable it via the Windows control panel:

- **Press the Windows button on the left side of the keyboard**
- Type: **ncpa.cpl** and press return
- Right click the network adapter you are using (this is wired, but you may be using a wireless adapater):

![Ethernet adapter in Windows](/20240221/step01.png)

- Click **properties** and untick Internet Protocol Version 6:

![Windows IPv6](/20240221/step02.png)

- Click **OK** and you're done.

Just remember you've done this because at some point in the future, the future is IPv6 😉