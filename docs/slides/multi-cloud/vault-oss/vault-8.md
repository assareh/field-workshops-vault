name: chapter-8
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Chapter 8
## Encryption as a Service

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???

* Chapter 8 introduces Vault's Transit secrets engine which functions as Vault's Encryption-as-a-Service (EaaS).

---
layout: true

.footer[
- Copyright © 2021 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: Vault-Transit-Engine

# Vault Transit Engine - Encryption as a Service
.center[![:scale 80%](images/vault-eaas.webp)]

* Vault's Transit Secrets Engine functions as an Encryption-as-a-Service.
* Developers use it to encrypt and decrypt data stored outside of Vault.

???

A really great thing about Vault is that we can use the same tool and methodology to achieve a lot of different use cases and one of the coolest ones is encryption as a service. So once I've authenticated like before, I want to do something entirely different than generate dynamic credentials.

What I need to do now is I happen to have pii information like social security numbers and I need to secure those in some fashion, well if I go talk to Oracle or Microsoft about securing data in my database, they're going to talk to me about transparent database encryption, let's encrypt the entire database.

that's cool if somebody rolls into my data center and walks off with my servers, as long as hopefully the credentials to decrypt it aren't also right there, well then cool, it's encrypted. the problem though, particularly in the cloud is that's unlikely to happen, an attacker is more likely going to compromise one of the applications that already has access to the database. so it doesn't matter if it's encrypted under the hood because that application they're connecting from has access.

So we want to think about really encrypting at a level that's below the entire database, per cell encryption. And so what we can do is we have applications like here let's say a web facing front end like a mortgage application. People will apply for mortgages and i'll include some pii information and that pii information should be encrypted.

And so before the application writes that to the database what's going to do is going to send it through the Vault eaas (transit backend) and then we have keys that stay inside the vault and they never leave the vault server.

It will encrypt that data and then hand the application back that encrypted data, and then it can write that encrypted data into the database. so the database will be a mix of unencrypted cells that didn't need to be encrypted, and encrypted data that needed to be transited through vault.

And now, if that application needs to read that data again it's going to pull that encrypted data up, it's gonna send it back through the eaas and get the decrypted beta back and present it. what makes this really powerful is for us encryption and decryption are different paths which means I can write different policies.

think about again that mortgage application. I absolutely need people to give me their social security number so that I can go put this in the information, so I can have a complete mortgage application. what I don't ever need to do is have my web facing application show somebody a social security number.

So I can create granular permissions that say hey allow the web facing application, the one that's probably going to get hacked, access to read and write to the database and the ability to encrypt with vault, but deny it the ability to decrypt with vault. So that way if it gets compromised, even though they can take the entire database they're not going to be able to decrypt any of the pii information because they don't have access to vault.

Fantastic. now I can also have say an analytics engine going against that database that doesn't need to decrypt anything, has no permissions to vault. it gets hacked, nobody got access to it, and then, finally, I could have say a more secure call Center application which does need to see that information. But I can tie that in with the auditing perspective, so I can know Okay, not only did you know, a  malicious call Center user who did access nine records they were not supposed to.

But using something called derived encryption, I can actually provide cryptographic confidence that they access to these exact nine records.

And so, even if it got to the point where you know some really malicious and smart user might be like ooh I know that if I go access bill gates's information directly, it's going to send up a red flag. So instead i'm going to go find bill gates's information encrypted and i'm going to copy it over john doe's and then read john doe's information.

vault by design with derived encryption can prevent that because that ciphertext is tied to a specific salt or context value that you would have to provide. and each user would have a specific context so we can ensure that my audit log authoritatively says exactly what occurred.

This is a huge boon for people writing applications because if you think about these sorts of problems lots of people have these problems with needing to encrypt data. But the developers are really not well suited to solve these types of problems individually.

i'm not gonna go look at my application developers and building my mortgage application and be like I really need you guys to care about pii and gdpr. and think of all the complex math behind cryptography to make sure that you do this in a secure fashion so there's no way it's going to be compromised.

Like that's really not a good use of their time, it's not core to my business, but it is really important to my business that I handle this well. So, like what's the right way to do that. that's where this whole encryption as a service construct comes in, is let your developers do what they do, build their application and provide them a platform that you know when the auditors show up, are the auditors going to believe that the application developers that are employed by you were not part of what got it compromised or built in a way that's perfect.

hard to make that argument, are they going to be more confident when they're like yeah here's an industry leading solution that we're taking advantage of, and we have a very clear demarcation point about how data comes in and out of this and a complete audit log that shows exactly what's gonna happen. yeah they're gonna like that a lot more, so let us handle the encryption service, let them do what they do best and we can be that black box with the keys inside of it.

---
name: transit-engine-benefits
# Transit Engine Benefits

* Vault's Transit Engine provides developers a well-architected EaaS API so that they don't have to become encryption or cryptography experts.
* It provides centralized key management.
* It ensures that only approved ciphers and algorithms are used.
* It supports automated key rotation and re-wrapping.
* If an attacker manages to get access to the encrypted data, they will only see ciphertext that is useless without Vault.

???
* act as key ring, any key you get a key ring, makes it easy to rotate that key, if you have reqmts to update keys every 90 days, we can do that easily.
* vault retains the previous versions and provides controls over how far back are allowed to decrypt, but requiring the latest key version for all encryption
* we also support a re-wrapping functionality, so if I want to say, like hey I need to make sure that all of my data is not on keys that are over six months old. So I need to decrypt it and then re encrypt it with the new key well that right there is a really potentially dangerous activity that actually run through my entire database and decrypt and re encrypt. We support rewrapping, doing this inside of the black box, send in data encrypted with key version 1, and Vault will send it back encrypted with key version two and nobody ever saw it or has the ability to decrypt all of it, and you know that it was never exposed.

---
name: Vault-Transit-Engine-1
# Vault Transit - Example Application

* In the next lab we'll use a web application that uses the Transit engine to encrypt and decrypt data.
* The app will store its encrypted data in the same MySQL database we used in Chapter 7.
* It will also get MySQL credentials from the Database secrets engine we configured in that chapter's lab.
* We'll first run the web app without Vault: No records are encrypted.
* We'll then run it with Vault enabled and see that new records are encrypted.

???
* Discuss the web app we will be using in this chapter's lab.
* Point out that it will use the same MySQL database from chapter 7.
* Point out that it will get its MySQL credentials from the Database secrets engine students set up in chapter 7.
* Indicate that we will first run without Vault and then with it.

---
name: web-app-screenshot
# The Web App
### Here is a screenshot of the Python web app:

.center[![:scale 70%](images/transit_app.png)]

???
* Show the screenshot of the web app.

---
name: web-app-views
# The Web App's Views
###There are two main sections in the application.
1. **Records View**
  * The Records View displays records in plain text, showing what a logged in user would see after any encrypted data is decrypted.

1. **Database View**
  * The Database View displays the raw records in the database, showing what SQL commands would return:

---
name: records-view
# The Web App's Records View
.center[![:scale 90%](images/records_view.png)]

* As we would expect an authorized user is able to see some of the sensitive data because the app has decrypted any encrypted data.

???
* Show the records view of the web app.

---
name: Vault-Transit-Engine-6
# The Add User Screen
* In the lab, you will add new users to the database.
.center[![:scale 60%](images/add_user.png)]

???
* Describe the Add User screen that students will use to add new records to the database.
* Point out again that when Vault is enabled, the records will be encrypted in the database.

---
name: database-record-without-vault
# Record in Database View Without Vault Enabled
* After adding a record in the lab, you will be instructed to click on the  **Database View** menu.
* You should see the exact same data that you entered.
* This means that Personally Identifiable Data (PII) is being stored in plain text in our database records.
* How can we improve this? Let's enable Vault's Transit engine and see.

---
name: encrypted-record
# A Database Record Encrypted by Vault
#### Here is a record that was encrypted by Vault's Transit engine.
.center[![:scale 80%](images/database_view_with_encrypted_record.png)]
* Note that the birth_date and social_security_number are encrypted.
???
* Show a record from the database encrypted by Vault's Transit engine.
* Point out that the birth_date and social_security_number field are encrypted as indicated by their starting with "vault:v1".
* Point out that the "v1" indicates that the first version of the encryption key was used.

* NEW: **FPE** a lot of applications have a schema that require this SSN for example to be 9 numeric digits. i don't want to have to update or rewrite this app, and retest it and all that.

---
name: encryption-key-rotation
# Rotating Transit Engine Encryption Keys
* The encryption keys of Vaults Transit Engine can be rotated.
* The newest version of the key is used to encrypt new data
* Older versions of the key can still decrypt old data but cannot decrypt new data.
* When we rotate the encryption keys, apps that use the Transit engine are unaware of any changes.
* Data can also be re-encrypted using the `rewrap` endpoint.

---
name: lab-transit-challenge-1
# 👩‍💻 Challenge 1: Enable the Transit Engine
* In this lab challenge, you'll enable the Transit engine.
* You'll do this in the [Vault Encryption as a Service](https://play.instruqt.com/hashicorp/invite/qleasfx1dszc) Instruqt track.
* Instructions:
  * Click the "Enable the Transit Secrets Engine" challenge of the "Vault Encryption as a Service" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* Instruct the students to do the "Enable the Transit Secrets Engine" challenge of the "Vault Encryption as a Service" track.
* This challenge has them enable the Transit secrets engine on the path "lob_a/workshop/transit".

---
name: lab-database-challenge-2
# 👩‍💻 Challenge 2: Create an Encryption Key
* In this lab, you'll create an encryption key for use with the Transit engine you enabled in the previous challenge.
* Instructions:
  * If the track does not do it for you, click the "Create a Key for the Transit Secrets Engine" challenge of the "Vault Encryption as a Service" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* Instruct the students to do the "Create a Key for the Transit Secrets Engine" challenge of the "Vault Encryption as a Service" track.
* This challenge has them create an encryption key for use with the Transit engine they enabled in the previous challenge.

---
name: lab-database-challenge-3
# 👩‍💻 Challenge 3: Use the Web App Without Vault
* In this lab, you'll use the web application without Vault.
* Instructions:
  * If the track does not do it for you, click the "Use the Web App Without Vault" challenge of the "Vault Encryption as a Service" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* Instruct the students to do the "Use the Web App Without Vault" challenge of the "Vault Encryption as a Service" track.
* This challenge has them use the web application without Vault.
* Point out that the new record they add during this challenge will nto be encrypted.

---
name: lab-database-challenge-4
# 👩‍💻 Challenge 4: Use the Web App With Vault
* In this lab, you'll use the web application with Vault.
* You'll also rotate the encryption key.
* Instructions:
  * If the track does not do it for you, click the "Use the Web App With Vault" challenge of the "Vault Encryption as a Service" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* Instruct the students to do the "Use the Web App With Vault" challenge of the "Vault Encryption as a Service" track.
* This challenge has them use the web application with Vault.
* Point out that the new record they add will have sensitive fields encrypted by Vault's Transit engine.

---
name: chapter-8-review-questions
# 📝 Chapter 8 Review
* What is the main advantage of using Vault's Transit secrets engine?
* Where does Vault's Transit Engine store encrypted data?
* Was the application still able to decrypt older encrypted records after you rotated the encryption key?
* Is it possible to tell which version of an encryption key was used?

???
* Let's review what we learned in this chapter.

---
name: chapter-8-review-answers
# 📝 Chapter 8 Review
* What is the main advantage of using Vault's Transit secrets engine?
  * Developers can encrypt data without being experts in cryptography.
* Where does Vault's Transit Engine store encrypted data?
  * Wherever developers want, but outside of Vault
* Was the application still able to decrypt older encrypted records after you rotated the encryption key?
  * Yes
* Is it possible to tell which version of an encryption key was used?
  * Yes. The version is indicated by `v1`, `v2`, etc.

???
* Here are the answers to the review questions.

---
name: conclusion
# Thank You for Participating!
.center[![:scale 40%](images/vault_logo.png)]

### For more information please refer to the following links:
* https://www.vaultproject.io/docs/
* https://www.vaultproject.io/api/
* https://learn.hashicorp.com/vault

???
* Thank the students for their participation
* Share some Vault links

---
name: Feedback-Survey
# Workshop Feedback Survey
* Your feedback is important to us!
* The survey is short, we promise:
  * http://bit.ly/hashiworkshopfeedback

???
* Ask them to fill out the online survey
