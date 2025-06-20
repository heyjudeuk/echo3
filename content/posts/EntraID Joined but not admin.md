+++
title = "Entra ID Joined, But Not an Admin? Fixing the “SID with a Question Mark” Issue on Windows 11"
date = "2025-06-20"
author = "Jude Sudbury"
cover = "/20250620/Microsoft-time-waster.webp"
description = "Entra ID Joined, But Not an Admin? Fixing the “SID with a Question Mark” Issue on Windows 11"
+++

If you've recently unboxed a brand-new Windows 11 Pro laptop, gone through the out-of-box experience (OOBE), signed in with a work or school account, and found that the device successfully joined Entra ID — only to discover that the user has no local admin rights — you’re not alone.

Even worse, when checking the local Administrators group, you may see a strange SID beginning with S-1-12-1... and a greyed-out question mark. No name. No permissions. And no way to elevate. In other words: a fully joined but unusable device.

Here’s what’s going on — and how to fix it.

#### 🧠 The Problem
Traditionally, the first user to join a Windows device to Azure AD (now Entra ID) is automatically added to the local Administrators group. But recently, Microsoft changed the default behaviour in new 365 tenants.

The result: the joining user gets a partial profile and a broken admin assignment, seen as a SID with no resolvable name. The OS knows the user exists (they can sign in!), but cannot elevate them — and you can’t install software, update drivers, or even reset Windows without an admin password.


#### 🕵️‍♂️ What Causes It?
The root cause is nearly always Entra ID device settings or Intune policies that block or fail to assign local admin rights. Specifically:

✅ Check This Setting:
Go to Entra ID Admin Centre → Devices → Device Settings

Under "Local administrator settings", ensure 'Registering user is added as local administrator on the device during Microsoft Entra join' is set to All and 'Global administrator role is added as local administrator on the device during Microsoft Entra join' is set to Yes (this is useful).


![Screen shot of EntraID Entra join settings under devices](/20250620/entra-id-join-as-admin.png)

#### 🛠️ How to Fix It
If you already have a device in this broken state — i.e. no admin users — you have a few options.

**Option 1: Reset and Try Again (with Correct Settings)**
If you’re early in deployment and don’t mind starting over:

Use Shift + Restart at the login screen → Troubleshoot → Reset this PC

Ensure internet is available throughout OOBE

Confirm your Entra ID tenant settings (see above) allow the joining user to become a local admin

> 🔐 If BitLocker is enabled, make sure the recovery key is in Entra ID before wiping

**Option 2: Use Intune to Assign Admin Rights Remotely**

If the device is enrolled in Intune, push a script or configuration profile to fix it without wiping.

PowerShell Script via Intune

```powershell
Add-LocalGroupMember -Group "Administrators" -Member "AzureAD\user@domain.com"
```

Deploy this via Endpoint Manager → Devices → Scripts.

#### 🧩 Conclusion
This issue is frustrating, especially when it leaves you with a fully joined but locked-down device. Fortunately, the fix is straightforward once you know where to look.

The key takeaway: Microsoft’s default behaviour has shifted. If you’re managing devices via Entra ID and Intune, double-check your tenant and deployment profile settings to avoid this silent gotcha.



