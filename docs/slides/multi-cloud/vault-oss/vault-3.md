name: Chapter-3
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Chapter 3
## Running a Production Vault Server

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???

Chapter 3 focuses on running a production Vault server

---
layout: true

.footer[
- Copyright © 2021 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: vault-production-serves
# Running a Production Vault Server
* Running a Vault server in "Prod" mode involves multiple steps:
  * Specify configuration in a config file.
  * Start the server.
  * Initialize the server to get unseal keys and an initial root token.
  * Unseal the Vault server with the unseal keys.

???
* Describe the steps to run a production Vault server.
* We should be running a cluster for high availability resilience to node failures, so we'll do this on multiple servers.
* We'll probably use something like systemd on Linux to run Vault as a service that starts up when the system is booted.

---
name: configuring-vault
# Configuring Vault Servers
* Vault configuration files can be specified in [HCL](https://github.com/hashicorp/hcl) or JSON.
* Common configuration settings include:
  * listener
  * storage
  * seal
  * log_level
  * ui
  * api_addr
  * cluster_addr

???
* Discuss Vault configuration files and common settings.
storage and listener probably your minimum

---
name: running-vault
# Starting a Production Vault Server
* You use the `vault server` command to start a Vault Production server.
* But, you do not use the `-dev` option.

???
* Describe the command to run a Vault production server.
use the -config to specify path to vault config or folder containing it

---
name: initializing-vault
# Initializing Vault Clusters
* Recall that a Vault cluster runs multiple Vault servers.
* Each Vault cluster must be initialized once.
* This is done with the `vault operator init` command.
* The number of key shares and the key threshold can be specified with the `-key-shares` and `key-threshold` options.
* The command returns the unseal keys and the initial root token for the cluster.

???
* Describe Vault's `init` command
* This command creates the cryptographic barrier. Can't be precreated because it must be unique to the system.

Academically how this all works is very interesting, though in production it's a giant pain. Because what we're doing is we're trying to see how can I generate trust in a place where I have no trust, I don't trust the system i'm running on. and I don't trust the people that i'm working with. now how do I have a system that's a security system that's about trust when I don't have any of that.

We use what's called Shamir's secret sharing in order to solve that which I think is academically super interesting. Basically, what we do is we take points off of a polynomial. so by default we're using five points with a threshold of three. We think about a line segment. for me to solve the equation of that line I need any two points in that line.

But for me to solve the equation of a parabola (hello school algebra) I need any three points on that parabola. so basically what we're doing is taking an arbitrary parabola, and we're picking five points off that parabola, and then we're going to give one point each to five five different people.

and if any three of those come back and enter in their shard, or key share, we're able to solve for the original equation of the parabola and regenerate the master key which allows us to decrypt the encryption key.

---
name: unsealing-vault
# Unsealing Vault Servers
* Each Vault server must be unsealed each time it is started.
* You cannot use the server until you unseal it.
* This is done with the `vault operator unseal` command, using the unseal keys returned when you initialized the cluster.

???
* Describe Vault's `unseal` command.
because we can't write the master key anywhere, that could compromise all of vault.
so that's why we split it up to multiple people that i'm going to trust
one person might be a bad actor, but it's much less likely that three different people are going to behave badly at the same time in collaboration with each other.
Not a perfect system but certainly better than trusting a single person.
Of course, the problem is, if the server restarts now it's two in the morning and I not only need to go find one person to go get us fix and I need to go find three people so I can get the thing back up and running.
And if i'm running multiple servers in that cluster I need to do this for every single one of those servers because i can't share the master key or the encryption key between the servers, it needs to be kept secure.

---
name: vault-status-command
# Determining the Status of a Vault Server
* Use the `vault status` command to get the status of a Vault server.
* It will tell you if your Vault server is sealed or unsealed.
* It will also tell you the following:
  * the number of key shares and the key threshold
  * whether HA mode (clustering) is enabled
  * whether the server is running as a performance standby.

???
Describe the `vault status` command

---
name: chapter-3-review-questions
# 📝 Chapter 3 Review

* What is used to configure a "Prod" mode Vault server?
* What Vault command needs to be run once against a new Vault cluster?
* What Vault command has to be run each time a Vault server is started?

???
* Let's review what we learned in this chapter.

---
name: chapter-3-review-answers
# 📝 Chapter 3 Review

* What is used to configure a "Prod" mode Vault server?
  * A configuration file
* What Vault command needs to be run once against a new Vault cluster?
  * `vault operator init`
* What Vault command has to be run each time a Vault server is started?
  * `vault operator unseal`

???
* Here are the answers to the review questions.
