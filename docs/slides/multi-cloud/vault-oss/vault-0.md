name: vault-title-slide
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Vault Workshop
## Modern Security with Vault for any Cloud

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???
Welcome to Intro to Vault! My name is Andy Assareh, I am a Solutions Engineer at HashiCorp, and I will be your primary guide through the workshop today.

We also have a fantastic group of Hashicorporeals and Dadgarians here that will be assisting as your lab TAs as well so let’s all give a fist pump for that.

Well we have a lot to cover today, so let's get right down to it.
---
layout: true

.footer[
- Copyright © 2021 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: Link-to-Slide-Deck
# The Slide Deck
<br><br>
### Follow along on your own computer at this link:

https://hashicorp.github.io/field-workshops-vault/slides/multi-cloud/vault-oss/index.html

???

The first question we usually hear during these workshops is "Can we have the slide deck?" The answer is YES, and we've made it really easy to download. In fact, it's just a website. Open this link and you can follow along on your own laptop. Just use the arrow keys to navigate.

---
name: Introductions
# Introductions

* Your Name
* Job Title
* Secrets Management Experience
* Favorite Text Editor

???
**Let's start with introductions. Give us your name, job title, any secrets management experience, and finally your favorite text editor. There are no wrong answers here. Except for Notepad. Notepad++ is ok though.**

If your audience is less than 25 people you can quickly go through the room and ask everyone to introduce themselves. The text editor question is a fun ice breaker, and it also gives you a quick barometer on how technical your students are.

**Rest assured you don't need to be an expert at vi to use Vault.**

Now introduce yourself, tell a story, give the audience something to think about.

---
name: Table-of-Contents
# Table of Contents

1. HashiCorp Vault Overview
1. Interacting with Vault
1. Running a Production Server
1. Vault Secrets Engines
1. Vault Authentication Methods
1. Vault Policies
1. Dynamic Database Secrets
1. Encryption as a Service

???
Here is our agenda for today's training. The format is simple, you'll hear a lecture and view slides on each topic, then participate in a hands-on lab about that topic. We'll alternate between lecture and lab to keep things interesting. We'll start with an introduction to Vault, then work our way through some key concepts, ending with some more advanced use cases. We will have time for questions at the end. The workshop should take roughly three hours to complete. We will have a short break after the first lab, which should be around 11.

---
name: instruqt-tracks
# Lab Environment Used
* This workshop uses [Instruqt](https://instruqt.com) for hands-on labs.
* Instruqt labs are run in "tracks" that are divided into "challenges".
* This workshop uses the following tracks:
    1. [Vault Basics](https://play.instruqt.com/hashicorp/invite/qfwncq62zsxu)
    1. [Vault Dynamic Database Credentials](https://play.instruqt.com/hashicorp/invite/sryhqfdm6sgx)
    1. [Vault Encryption as a Service](https://play.instruqt.com/hashicorp/invite/qleasfx1dszc)
* We'll cover chapters 1-6 and then do the first lab.
* We'll then cover chapter 7 with the second lab.
* We'll finish with chapter 8 and the third lab.

???
All you'll need to participate in the hands-on labs is a web browser.

One thing to note regarding corporate VPNs- we have had reports of issues with the labs when connected to a corporate VPN. If you are on a VPN and do experience issues, please try disconnecting from the VPN.
