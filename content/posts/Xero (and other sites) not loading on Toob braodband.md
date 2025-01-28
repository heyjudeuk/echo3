+++
title = "Fix: Xero and Web Pages Not Loading on Toob Broadband"
date = "2025-01-28"
author = "Jude Sudbury"
cover = "20250128/MTU.webp"
description = "MTU"
+++

**Fix: Xero and Web Pages Not Loading on Toob Broadband**

Recently, a client approached me with a peculiar problem: web apps like Xero, Bankline, Tesco’s website, and even Toob wouldn’t fully load on their Windows 11 laptop using Chrome or Edge. Refreshing the browser sometimes resolved the issue, but it wasn’t reliable. He told me that the problem started after switching his ISP from Virgin to Toob. After troubleshooting—clearing the browser cache, disabling extensions and ad-blockers, and resetting the network stack—I turned to Google and discovered a Reddit thread ([link here](https://www.reddit.com/r/Southampton/comments/19c2vrm/anyone_else_having_connectivity_issues_with_toob/)) that described someone with exactly the same issue. He suggested changing the MTU to 1280; implementing this fix resolved my client’s issue entirely.

**What is MTU and Why Does it Matter?**

MTU (Maximum Transmission Unit) defines the maximum size of a data packet that can be sent over your network. If the MTU size is too large for your internet connection, it can cause packet fragmentation or drops, leading to incomplete data transfer. This can result in web pages failing to load correctly or web apps behaving erratically.

For some networks, especially those with VPNs, certain ISPs (like Toob), or firewalls, an MTU setting that’s too high can cause these connectivity issues. Reducing the MTU is often a simple fix that ensures data packets are small enough to travel without being fragmented. Why 1280? I found subsequent threads on Reddit that claim Toob support have stipulated 1280, which is also the minimum allowed by IPv6. I checked his router interface and found that the MTU for both IPv4 and IPv6 were both set to 'auto'.If the router negotiates an MTU size of 1280, Windows’ default MTU of 1500 can cause packet fragmentation for outgoing traffic. This is generally not an issue for basic browsing if data received from webservers via the router is in smaller packets than Windows is configured to handle. However, for interactive web applications like Xero, which involve bidirectional communication, fragmentation of outbound traffic can interfere with critical processes like HTTPS or TLS handshakes. This can cause issues such as incomplete page loading or erratic app behavior.

---

### **How to Set the MTU to 1280 on Windows**

Follow these steps to adjust the MTU size on a Windows 10 or 11 laptop:

1. **Open Command Prompt as Administrator**

   - Press `Win + S` and type “Command Prompt”.
   - Right-click on **Command Prompt** and select “Run as administrator”.

2. **Find Your Network Adapter Name**

   - In the Command Prompt, type:
     ```
     netsh interface ipv4 show subinterfaces
     ```
   - Press Enter. You’ll see a list of network adapters. Look for the one you’re using (e.g., Wi-Fi or Ethernet).

3. **Set the MTU Size**

   - Use the following command to set the MTU to 1280:
     ```
     netsh interface ipv4 set subinterface "<Adapter Name>" mtu=1280 store=persistent
     ```
   - Replace `<Adapter Name>` with the name of your network adapter. For example:
     ```
     netsh interface ipv4 set subinterface "Wi-Fi" mtu=1280 store=persistent
     ```

4. **Verify the Change**

   - Run the following command to confirm the MTU has been updated:
     ```
     netsh interface ipv4 show subinterfaces
     ```
   - Check that the MTU for your adapter now shows 1280.

5. **Restart Your Computer**

   - Reboot your device to ensure the changes take effect. This is not strictly necessary when changing MTU size, however.

---

### **Why This Fix Works**

By reducing the MTU to 1280, you’re aligning the laptop’s outgoing packet size with the router and ISP’s negotiated limits (assuming the internet's claim that Toob support have given a figure of 1280). This ensures that packets are not fragmented at the router, avoiding issues with outgoing traffic that could interfere with critical processes like HTTPS or TLS handshakes. While incoming data from web servers is generally unaffected by an MTU mismatch, setting the MTU consistently prevents outgoing fragmentation, which is crucial for web apps like Xero that rely on bidirectional communication. This fix resolved my client’s page loading and interaction issues entirely.

