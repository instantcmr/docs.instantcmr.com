

# Anti-Spam requirements for email contacts

InstantCMR takes the enforcement of anti-spam laws very seriously. Spam can lead to negative impacts for all our users, so it’s important that your emails comply with applicable laws. As outlined in our Terms of Use, we take many protective measures beyond what is required by law to help prevent spam and protect our platform.

InstantCMR only sends emails (e.g. user invitation, inbound document notification or password reset emails) to email addresses provided and authorized by you and verified by our customer service.

Amongst other channels, these email addresses can be provided to instantCMR via the instantCMR API described in this document. In order to avoid accidental misuse of our API, we require that you pre-authorize each distinct email address sent to us via the instantCMR API.

Any API requests that contain email contact entities with email addresses that are not yet authorized will be rejected with a **400 BadRequest** HTTP response as shown in the following sample

```http
HTTP/1.1 400 {field}: unauthorized to send email to {email}  
Date: Thu, 23 Jan 2020 16:29:47 GMT  
Server: Jetty(9.2.1.v20140609)  
```

{% table %}
---
* **field**
* The name of the field containing unauthorized email contact.
---
* **email**
* The email address that failed the email authorization check.
{% /table %}


## How can I pre-authorize an email address?
In order to pre-authorize an email address or a range of email addresses please contact our customer support at **[help@instantcmr.com](mailto:help@instantcmr.com)** when setting up your account with us. You can preauthorize

*   All email accounts on your domain name (e.g. **\*@logisticsgmbh.de**). You will need to provide proof that your company owns the domain name.
*   A single email account on any domain name (e.g. **bertram.friedrich@emails.com**). You will need to provide proof that you own the email account.

