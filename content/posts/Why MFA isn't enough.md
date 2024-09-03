+++
title = "Why MFA Isn’t Enough: The Need for Stronger Security Measures in Microsoft 365"
date = "2024-09-03"
author = "Jude Sudbury"
cover = "20240903/computer_hacker.webp"
description = "Why MFA Isn’t Enough: The Need for Stronger Security Measures in Microsoft 365"
+++

### Why MFA Isn’t Enough: The Need for Stronger Security Measures in Microsoft 365

Multi-Factor Authentication (MFA) is a robust solution for protecting online accounts, including those within Microsoft 365. It adds an extra layer of security beyond just a password, typically requiring a second factor like a text message code or an app-based approval. However, relying solely on MFA can lead to a false sense of security. While MFA is a significant improvement over password-only protection, it has vulnerabilities that can be exploited by determined attackers using a blend of social engineering and readily available (and easy-to-use) software.

### Evilginx

Evilginx is a phishing framework that can steal credentials and session cookies even when MFA is in place. Evilginx acts as a man-in-the-middle, intercepting traffic between the user and the legitimate website. This allows attackers to capture not only the username and password but also the MFA token. An MFA token, or session cookie, is a file stored on a user's device after successful authentication that prevents the need to re-enter MFA credentials repeatedly during the same session. Once in possession of a target's login credentials and session cookie, the hacker can log in to the compromised account. Once inside, they can add their own MFA device to the hacked account, ensuring easy access in the future. Evilginx’s [quick start guide](https://help.evilginx.com/docs/getting-started/quick-start) promises to teach would-be hackers (ethical or otherwise) how to use the tool in just 5 minutes.

### The Role of Social Engineering

Social engineering remains a powerful weapon in the attacker’s arsenal. Even the most secure systems can be compromised if a user is tricked into giving away their credentials or clicking on a malicious link. Phishing attacks, as they are known, have become increasingly sophisticated, often mimicking legitimate communications so well that even trained and/or cautious users can be fooled.

### 6 Steps to Hacking

#### 1. **Setting Up Evilginx:**
   - The attacker sets up a server running Evilginx. This server will act as a reverse proxy, sitting between the victim and the legitimate website (e.g., Google, Microsoft, etc.).
   - The attacker configures Evilginx to impersonate the login page of the target website by cloning the legitimate website's HTML, CSS, and JavaScript content.

#### 2. **Creating the Phishing Link:**
   - Once Evilginx is set up, the attacker generates a phishing URL that points to the Evilginx server. This URL will look like it belongs to the legitimate website but will actually direct the victim to the attacker's server.
   - To make the URL less suspicious, attackers often use techniques like typosquatting (e.g., "g00gle.com" instead of "google.com"), URL shortening services, or even legitimate-looking domain names with subtle changes.

#### 3. **Sending the Phishing Email:**
   - The attacker crafts an email that looks legitimate, possibly impersonating a trusted entity (e.g., a colleague, IT department, or known service provider).
   - In the email, they include a link to the 'important document' or other urgent content, which is actually the phishing URL pointing to the Evilginx server.

#### 4. **Victim Clicks the Link:**
   - When the victim clicks on the link, they are directed to the Evilginx server. Evilginx, acting as a reverse proxy, forwards the victim's traffic to the legitimate website in real time.
   - The victim sees the actual login page of the legitimate website, making the phishing attempt harder to detect.

#### 5. **Capturing Credentials and Session Cookies:**
   - When the victim enters their login credentials on the spoofed page, Evilginx captures these details before passing them on to the legitimate website.
   - If the victim uses two-factor authentication (2FA)/multi-factor authentication (MFA), Evilginx captures the MFA token as well.
   - The legitimate website then creates a session for the user (e.g., setting cookies), which is also captured by Evilginx. This allows the attacker to hijack the session and access the victim's account without needing the login credentials again.

#### 6. **Redirecting the Victim:**
   - After capturing the credentials and session data, Evilginx usually redirects the victim to the legitimate website or a legitimate document, so the victim may not realise they've been phished.

### Why Do Hackers Do It?

In the earlier days of hacking and computer viruses, the motivations of hackers and virus creators were often different from those seen today. Early hackers were driven by a mix of curiosity, challenge, experimentation, and, in some cases, a desire for recognition or notoriety. Modern cybercriminals frequently seek financial gain through ransomware, data theft, and other malicious activities.

Once a hacker gains access to a victim's account, their primary focus is often on the victim's email. Typically, the hacker will create inbox rules to filter or hide specific incoming emails from the legitimate user, ensuring that their activities go unnoticed. They may also delete sent emails to cover their tracks. The hacker’s goal is to remain undetected for as long as possible, potentially engaging in a "long game" by interacting with third parties known to the victim. Their ultimate aim might be to extort money by redirecting payments for an outstanding invoice to a bank account they control. This type of stealthy and insidious behaviour can result in significant financial loss.

On the other hand, some hackers are less cautious and use the compromised account to send thousands of spam or phishing emails. This more blatant misuse often leads to a quicker discovery, as third parties frequently report the suspicious activity back to the victim, often through non-email channels, alerting them to the breach.

### User Training: Important but Not Sufficient

Many organisations emphasise user training as the first line of defence against these kinds of attacks; it’s not possible to use Evilginx to simply hack an account—the hacker must trick the target into visiting the fake website and entering their credentials. While educating users on recognising phishing attempts and maintaining good security hygiene is crucial, it’s arguably not enough. Humans are fallible, and even well-trained individuals can make mistakes. Social engineering attacks are designed to exploit this very weakness. Therefore, relying solely on user training as a defence mechanism is risky.

### The Case for Hard Controls: Conditional Access and Device Compliance

Given the limitations of MFA and the susceptibility of users to social engineering, it’s clear that stronger, more technical controls are necessary. One of the most effective strategies is implementing Conditional Access policies that restrict access to Microsoft 365 resources based on device compliance.

**What does this mean?**

It means that only devices that meet certain security criteria—such as being enrolled in Intune, Microsoft’s device management platform—are allowed to access corporate resources. Enrolling devices into Intune ensures that they are compliant with your organisation’s security policies, such as having up-to-date antivirus software, operating systems, encryption, and other protections. This approach significantly reduces the risk of unauthorised access, as attackers would need to compromise a compliant device (i.e., gain remote control or have physical access to it) to gain entry, which is far more challenging than bypassing MFA.

**Corporate-Owned vs. Personal Devices**

While it’s ideal for all devices accessing corporate resources to be company-owned and tightly controlled, the reality is that many employees prefer to use their personal mobile devices for work, especially for checking email. To accommodate this, organisations can offer users the option to enrol their personal devices in Intune. This allows IT to enforce security policies on personal devices without compromising the user’s privacy for non-work-related activities. However, users should be aware that enrolling their personal devices means the organisation can enforce security requirements, such as mandatory updates and the ability to remotely wipe the device if it’s lost or stolen. The remote wipe of personally owned, enrolled devices is normally selective, i.e., only corporate data is removed.

### Conclusion: The Need for a Multi-Layered Approach

While MFA is a crucial component of a secure Microsoft 365 environment, it should not be relied on in isolation. The combination of easily accessible hacking tools like Evilginx, the prevalence of social engineering attacks, and the fallibility of human users means that organisations must implement stronger controls. Conditional Access policies that ensure only compliant devices can access corporate resources offer a more reliable defence against unauthorised access.

By enrolling devices into Intune, organisations can ensure that security policies are consistently enforced, reducing the risk of a breach. In today’s threat landscape, a multi-layered approach that includes hard technical controls is essential to safeguarding your Microsoft 365 environment.