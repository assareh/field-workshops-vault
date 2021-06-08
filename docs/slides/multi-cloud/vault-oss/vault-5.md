name: chapter-5
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Chapter 5
## Vault Authentication Methods

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???

* Chapter 5 introduces Vault authentication methods
* It focuses on the Userpass method.

---
layout: true

.footer[
- Copyright © 2021 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: vault-auth-methods
# Vault Authentication Methods
.center[![:scale 45%](images/vault_auth_methods.png)]
.center[Vault supports many different authentication methods.]

???
Vault supports many different authentication methods just like we support many different secrets engines.

I'm really trying to find what is the best possible way for me to authenticate this workload, and usually it's what's going to give me the most confidence that this workload is who it says it is, or this user is who they say they are. the user ones are pretty straightforward right it's wherever your directory service is that you use to authenticate people and we'll usually just tie that into vault.

ideally maybe something like okta that has two factor authentication out of the box, we can tie it all together. If you use AD we can use LDAP, or if you want to make everyone use their github account, we can do that, whatever it is.

* Auth methods are how your apps and users verify their identity.
* In the same way you might present some kind of valid ID at the hotel check-in desk, users and apps provide some kind of credential or token to authenticate.
* You can enable multiple auth methods and multiple instances of the same auth method.

---
name:vault-auth-methods-2
# Some Important Vault Auth Methods

<div style="float: left; width: 50%;">
<u>Methods for Users</u>
<ul>
<li>Userpass</li>
<li>GitHub</li>
<li>LDAP</li>
<li>JWT/OIDC</li>
<li>Okta</li>
</ul>
</div>
<div style="float: right; width: 50%;">
<u>Methods for Applications</u>
<ul>
<li>AppRole</li>
<li>AWS</li>
<li>Azure</li>
<li>Google Cloud</li>
<li>Kubernetes</li>
</ul>
</div>

???
the application methods are usually the ones that are more interesting to think about. cloud, k8s
if we're on prem we may not have those, but we have another solution called approle which can solve this as well.
in general though we'll want to use the mechanism native to that platform if one is available.

approle is really cool in that again we're thinking about how can we generate security when we don't actually trust any one particular thing. and so with approle we created a construct where like hey maybe you're using VRA provisioning and then you're using chef configuration management, well, let me pass an encrypted APP ID through VRA. and then i'll send the role ID via chef. i'm gonna assume that somebody is not going to simultaneously compromise both of those systems in order to compromise approle.

we're able to combine these two things together, generate one time tokens so that I can then create a long lived token to authenticate to vault in a secure fashion, where neither of the orchestration methods that actually got me those credentials were able to compromise unless somebody could compromise the whole system together.

* Userpass - Allows users to authenticate with username and password managed by Vault
* GitHub - Allows users to authenticate with their GitHub personal access tokens
* LDAP - Allows users to authenticate against an LDAP server with their username and password managed by that server.
* JWT/OIDC - Allows users to authenticate against an external OpenID Connect provider or with JSON Web Tokens (JWTs)
* Okta - Allows users to authenticate using Okta single sign-on.
* AppRole - Allows applications to authenticate in automated workflows using a role and a role ID.
* AWS - Allows applications on AWS EC2 instances and Lambda functions to authenticate with IAM credentials or EC2 metadata.
* Azure - Allows applications associated with Azure Managed Service Identities to authenticate using Azure Active Directory credentials.
* Google Cloud - Allows applications in GCP to authenticate using Google Cloud IAM service accounts or Google Compute Engine (GCE) metadata.
* Kubernetes - Allows Kubernetes pods to authenticate with JWT tokens.

---
name: enabling-auth-methods
# Enabling Authentication Methods

* Most Vault auth methods need to be explicitly enabled.
* This is done with the `vault auth enable` command.
* Each auth method has a default path.
* Alternate paths can be specified to enable multiple instances:<br> `vault auth enable -path=aws-east aws`
* Custom paths must be specified in CLI commands and API calls:<br>
`vault write aws-east/config/root`<br>
instead of<br>
`vault write aws/config/root`

???

* Talk about enabling auth methods.
* Talk about default and custom paths
* Explain the examples

---
name: userpass-0
# Vault's Userpass Auth Method
.center[![:scale 30%](images/userpass_login.png)]
* The Userpass method authenticates users with usernames and passwords managed by Vault.

???
* The Userpass method allows users to authenticate with username and password managed by Vault.
* It is not recommended for production, but it's fine for development and lab environments.
* In the real world you'd probably have Vault use your Active Directory, LDAP, GitHub, or other system of record for authentication by users.

---
name: chapter-5-review-questions
# 📝 Chapter 5 Review
* What types of entities can Vault authenticate?
* What system manages credentials for the Userpass auth method?
* Can a user that is not assigned any policies other than the default policy access any secrets?

???
* Let's review what we learned in this chapter.

---
name: chapter-5-review-answers
# 📝 Chapter 5 Review

* What types of entities can Vault authenticate?
  * Users and applications
* What system manages credentials for the Userpass auth method?
  * Vault
* Can a user that is not assigned any policies other than the default policy access any secrets?
  * No

???
* Here are the answers to the review questions.
