+++
title = "Email delivery failure from Microsoft 365 to Yahoo"
date = "2024-03-12"
author = "Jude Sudbury"
cover = "/email_fail.webp"
description = "Email delivery failure from Microsoft 365 to Yahoo"
+++

I had a customer report today that all emails they were sending from their Microsoft 365 account to all Yahoo email addresses were eventually bouncing as non deliverable.

A message trace reveals this rather unhelpful information:

> 3/10/2024 10:38:41 AM - Server at LO6P123MB7159.GBRP123.PROD.OUTLOOK.COM returned '550 5.4.300 Message expired -> 451 4.4.400 Error communicating with frontend host or destination host. -> 421 4.4.2 Connection dropped due to SocketError'
3/10/2024 10:33:39 AM - Server at mx-eu.mail.am0.yahoodns.net (188.125.72.73) returned '451 4.4.400 Error communicating with frontend host or destination host. -> 421 4.4.2 Connection dropped due to SocketError'

It turns out that Yahoo seem to *require* all sending domains to use DKIM and to have a valid DMARC record.

#### Setting up DKIM on Microsoft 365

[How to use DKIM for email in your custom domain | Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-365/security/office-365-security/email-authentication-dkim-configure?view=o365-worldwide) to For instructions on setting up DKIM.

#### Create a DMARC record

There are several tools online to help you generate a DMARC record which is then published using DNS:
[DMARC record generator](https://dmarcian.com/dmarc-record-wizard/)

#### 3rd party domain spoofing

If you also use 3rd party services, like Mailchimp for example, that will send emails from your domain then ensure they a) support DKIM signing and b) follow their instructions on setting this up; more DNS records will be required.

#### Double check your SPF record

Linked with the above point regarding 3rd party 'spoofers', make sure your SPF record includes all entries for all 3rd parties sending mail on your behalf. This is especially important if you've created a strict DMARC policy that requires SPF and DMARC alignment.

Sorry for the brief post. If none of this means anything and you're having mail delivery issues. Then reach out to me!