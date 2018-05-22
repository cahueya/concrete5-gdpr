This page serves as an idea dump from the #gdpr channel of concrete5 Slack

To contribute ideas, please join the Slack channel above.
## General

The General data protection regulation is a law that will be active in May 2018 and affect any internet-wide collection, procession or transfer of personal data of EU citizens. This will affect companies and website owner worldwide, if they interact with EU citizens.

To get some details about the Regulation, look here: [https://www.eugdpr.org/](https://www.eugdpr.org/)

## Who is affected by this?
The GDPR aims to protect the personal data of EU citizens and summarize the data protection laws of the EU states so that data within the EU can be shared and processed on equal terms.

### Data subjects
The Data subjects are the people whose data is being used: the customers. Data subjects are natural personas living in the EU. If you are working from the US but dealing with EU citizens, the GDPR does apply to you.


I will try to summarize the relevant points that need to be applied to make a website gdpr-compliant. I am not a lawyer, this is no legal counseling. The intent of this page is to provide orientation for the creation of a GDPR-compliance Package / Documentation for the use with concrete5.


### Outline

The process of being compliant to GDPR can be divided in three separate categories (please educate me if I'm wrong):

1. Behavior as a website owner, your obligations

2. Documentation

3. Technical bits

The goal of this page is the creation a working solution to all three:

1. A guidance of behavior for website owners

2. A list of documentation that you must provide as a website owner

3. A package / code adaptation for concrete5 that fulfills the technical bits

## 1. Behavior as a website owner

* Data reduction. Only collect the data you actually need. If you need the data consistently (i.E. sending Emails to that person), you may retain the data. If you do not use or need the data (i.E. someone contacted you over contact form, does not wish to receive a newsletter and you contact is over), you must dismiss that data if there is no other reason to keep it. 
This also means that your error- and access-logs should be flushed after a reasonable time. Like, if you think you should keep the logs for up to 3 months for error tracking, this is fine but you need to clarify that in your documentation and flush the logs after that time.
* Consent. You need to collect the consent of whoever you are collecting the data from. A checkbox with a link to your privacy statement would serve this purpose. Opt-In is required.
* Secure Data Transport. A SSL/TLS certificate seems unavoidable to mitigate MIM attacks. Either obtain a commercial certificate or read up about the Let's Encrypt Initiative to get a high-quality, free certificate.
* Breach reports. GDPR requires that you inform your Data Protection Officer within 72hrs after you know about a data breach or privacy intrusion to your system.
* Data protection officer. If you are a company with 10 or more people you need to have a data protection offical who is the contact for any means of data security problems.
* Backup Hygiene. If you collect personal data of your users (user profiles), one of them might request deletion one day. You are obligated to remove their data from your production system (live system). If there is user data in your backup files, you must keep these backups reasonable secure (encrypted, safe storage) so that no-one can get access. If you must restore your website from an older backup so that the data becomes "live" again, you must re-perform the deletion of the users that asked for it. 
All you obligations to reduce or delete user data can be overridden by tax laws. So if your country of residents requires you to keep tax records of purchases for 10 years, you are no longer obliged to delete that data even upon request.

## 2. Documentation

* Data Protection Officer. You need to assign a Data protection officer who has knowledge about privacy laws as well as IT security.  Here are some guidelines of how to appoint someone: https://en.wikipedia.org/wiki/General_Data_Protection_Regulation#cite_note-18
* You need to clarify in your privacy statement WHAT data you collect and what the PURPOSE of the collection of this data is. You need to explain with whom the data can be shared and how you treat it and WHEN you are gonna delete it.
* In your Privacy Statement you need to list any external services who get in touch with your customers' traffic. If you use a CDN for Javascript, Google Webfonts or Maps or any Service that is called whenever your site is called, you need to list it.
* Data processing agreements. Whichever service gets in touch with the data you collected from your users (IP address, username) needs to have a data processing agreement with you. Google Analytics -> you need agreement. Mailchimp Mailing Service -> you need an agreement with them. And of course you must mention the use of that service and for which reason and and which data you share with them.


## 3. Technical bits

Some technical challenges come with GDPR. I will try to outline the technical bits and write up some ideas on how these could be achieved. Please contribute with technical knowledge!

* Pseudonymisation. GDPR requires that even in the case of a data breach, the personal data of the website customers (Name, Address, Birthdata) should not be usable to the intruder. This requires encryption of the data.
* An example of pseudonymisation is encryption, which renders the original data unintelligible and the process cannot be reversed without access to the correct decryption key. The GDPR requires that this additional information (such as the decryption key) be kept separately from the pseudonymised data. Pseudonymisation is recommended to reduce the risks to the concerned data subjects and also help controllers and processors to meet their data-protection obligations (Recital 28). Although the GDPR encourages the use of pseudonymisation to "reduce risks to the data subjects," (Recital 28) pseudonymised data is still considered personal data (Recital 26) and therefore remains covered by the GDPR. https://en.wikipedia.org/wiki/General_Data_Protection_Regulation#Pseudonymisation
**Update** from a recent webinar, it seems like a database that is protected with regular measures is ok.
* Right to access. You need to enable the customer access to their personal data. This means: EITHER a user profile is mandatory and needs to be accessible to the user, but the user shall have the possibility to keep their profile private. OR you need to provide a user with the data you have had collected of them within **one month** after they request it.
* Right to erasure. The user must be able to delete the profile. I assume that deleting the user profile includes removing the data from the database. This should either be part of a user profile or you must fulfill the users request for deletion within one month after request.
* Data portability. There must be a possibility to export the personal data so that it can be imported to another system. So you either provide the user data in an easy-to-get format (Copy&Paste-friendly) OR as an export batch OR you need to hand it out to the user within one month after their request.

## Solutions that are already possible

I will try to outline which technical solutions would help comply to GDPR:

### GDPR Package in the Marketplace ###
There are two packages in the concrete5 marketplace that help a lot in being compliant with GDPR:


[https://www.concrete5.org/marketplace/addons/gdpr](https://www.concrete5.org/marketplace/addons/gdpr)

[https://www.concrete5.org/marketplace/addons/extreme-clean1](https://www.concrete5.org/marketplace/addons/extreme-clean1)


### Flushing or Anonymisation of server logs (access logs / error logs that contain IPs)
**This can not be done through concrete5!** Go find a host that does these things or get access to the logs yourself. Whoever is offering the website to the general public is responsible for the fulfilling of the GDPR regulation.

### Registration process
* Double-Opt-In (Email Confirmation)
In http://domain.tld/index.php/dashboard/system/registration/open, choose "Off", if you don't want any registrations or "Validate" if you want registrations to be possible.

* Enable declaration of consent to the privacy statement (required checkbox at the registration form)
In http://domain.tld/index.php/dashboard/users/attributes, choose "Add Attribute" and choose "Checkbox" from the list. Name the attribute handle something like "privacy_check", for the attribute name, formulate something like "I have read and understood the terms of service and the privacy declaration and agree to it" and make it shown and required on User registration. 

* Safe connection. Use Encryption (SSL/TLS) to ensure nobody collects the data. You can use Let's Encrypt easily if you have ssh access to your server. If you are not sure what to do, this Add-On might help:[http://www.concrete5.org/marketplace/addons/lets-encrypt](http://www.concrete5.org/marketplace/addons/lets-encrypt) 

### Documentation
* In your privacy statement, you need to declare WHAT data you are collecting, WHAT you are using it for and HOW you treat it. i.E. "We need to collect your email address to stay in touch with you. We also require your name and address for billing of our services."
* If you are a company above 10 employees or the data you are handling is highly sensitive (banking data, health data), you need to appoint a Data Protection Officer within your company.
* You need to explain the rights of the customer regarding their private data (change, erase, export)

### User profile
* Possibility to switch a public profile to "private" through permissions


## Solutions that could be built as a package for concrete5

### User profile
* Possibility to delete or request deletion through the user profile
* The user data must be really deleted from the database or pseudonymized (this issue has been resolved: [https://github.com/concrete5/concrete5/issues/6283](https://github.com/concrete5/concrete5/issues/6283) .)
* Possibility to export the user data through the user profile (CSV?) OR possibility to display the user data in an easy-to-copy field

### Security Check
Built-in Checking Tool that will look for:
* Working SSL/TLS connection
* Privacy consent checkbox in user registration present?
* Double Opt-In configuration for user profile?
* Checkbox "Do you have a sufficient privacy statement?"

### Legal Sharing / Like buttons
Most of the available Social Media Share/Like plugins will be ILLEGAL according to GDPR because they transmit the users data of visit to the networks without getting any voluntary confirmation. To be GDPR compliant, there is need for a new plugin that needs to be actively enabled to make the connection to the networks like this:
https://github.com/heiseonline/shariff
There is a plugin for concrete5 v.6 in the marketplace:
http://www.concrete5.org/marketplace/addons/social-share-privacy/
