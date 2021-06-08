name: chapter-6
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Chapter 6
## Vault Policies

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???

* Chapter 6 introduces Vault Policies

---
layout: true

.footer[
- Copyright © 2021 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: vault-policies
# Vault Policies
* Vault Policies restrict the secrets users and applications have access to.
* Vault follows the practice of least privilege, *denying* access by default.
* Vault administrators must explicity grant users and applications access to specific paths with policy statements.
* In addition to specifying paths, policies also specify a set of capabilities for those paths.
* Policies are written in HashiCorp Configuration Language (HCL).

???
if you're familiar with terraform it's a similar syntax

---
name: vault-policy-example
# A Vault Policy Example
* Here is an example of a Vault policy:
```hcl
# Allow tokens to look up their own properties
path "auth/token/lookup-self" {
    capabilities = ["read"]
}
```
* Note that this policy does not allow tokens to change their own properties.
???
* This policy allows tokens to look up their own properties

allows you to see who you are, or at least who vault thinks you are mua ha ha ha evil

---
name: vault-policy-paths-capabilities
# Policy Paths and Capabilities
* The path of a policy maps to a Vault API path.
* The most common capabilities granted are: `create`, `read`, `update`, `delete`, and `list` which correspond to HTTP verbs like POST and GET.
* Two other capabilities do not correspond to HTTP verbs:
  * `sudo` allows access to paths that are root-protected.
  * `deny` denies access to a path and takes precedence over other capabilities.

???
* Explain policy paths and capabilities

explicit deny is there if you want to be certain that somebody can't write a policy that's going to accidentally grant access to something they shouldn't

sudo is another interesting one, when we first configure vault we have a root token which we authenticate with. but that root token is not something that we want to use long term. it's too powerful and must be destroyed.. So we're going to use something like user pass or some other method to grant access to users who are not root users, but I still need to have administrators in here. And so we can have these people who don't have access to the root policy gain access to things that generally only the root policy would do like granting sudo permissions, and so basically just allows them to get access to the special areas.

---
name: policies-for-lobs
# Configuring Policies for LOBs
* Many organizations organize Vault secrets by line of business (LOB) and department.
* Here's an example policy for line of business A, department 1:

```hcl
path "lob_a/dept_1/*" {
    capabilities = ["read", "list", "create", "delete", "update"]
}
```

* This policy grants all standard capabilities to all secrets mounted under `lob_a/dept_1/` by using the glob character (`*`).

???
* Talk about how many organizations organize Vault secrets by line of business and department.
* Explain the policy including the glob character and that it can only be used at the end of a path.

You can use HEREDOC format to pass in the policy directly without saving it to a file first, but generally in production, what i'd recommend is keeping all these policies in version control because you're going to want to ultimately be able to recreate and preferably version them as they change over time.

and i've seen where people have used pipelines in order to push all these policies from a git repository into vault so that they know exactly who made changes and when they made them in git in addition to the fact that you'll see this in the vault audit logs as well.

similarly there's a terraform provider for configuring vault that is becoming an inceasingly popular way to do this as well, because then you have the configuration captured as code.

---
name: vault-policy-commands
# Vault Policy CLI Commands
* Vault policies can be added to a Vault server using Vault's CLI, UI, or API.
* The command to add a policy with the CLI is `vault policy write`.
* Here is a command that creates a policy called "lob-A-dept-1" from the HCL file "lob-A-dept-1-policy.hcl":<br>
`vault policy write lob-A-dept-1 lob-A-dept-1-policy.hcl`
* Here is a command that associates this policy with a Userpass user:<br>
`vault write auth/userpass/users/joe/policies policies=lob-A-dept-1`

???
Describe the most important Vault CLI commands for policies.

---
name: chapter-6-review-questions
# 📝 Chapter 6 Review
* Does Vault grant access to secrets by default?
* What are the policy capabilities that correspond to HTTP verbs?
* What CLI command can be used to add a policy to Vault?

???
* Let's review what we learned in this chapter.

---
name: chapter-6-review-answers
# 📝 Chapter 6 Review

* Does Vault grant access to secrets by default?
  * No
* What are the policy capabilities that correspond to HTTP verbs?
  * `create`, `read`, `update`, `delete`, and `list`
* What CLI command can be used to add a policy to Vault?
  * `vault policy write`

???
* Here are the answers to the review questions.
