# Audit Your Org For "Privileged Users"

This repo contains a minimalistic audit config and a quick guide to audit your Salesforce Orgs for "privileged" users that will require phishing-resistant MFA.

It helps you to identify all users that will be affected by this update.

## Background

Salesforce will enforce additional requirements for "privileged users", as they are described [here](https://help.salesforce.com/s/articleView?id=005321563&type=1).

In a nutshell: users that have `AuthorApex`, `CustomizeApplication`, `ModifyAllData` **OR** `ViewAllData`, will require additional steps to ensure, that their MFA is phishing resistant.

## How It Works

The audit config uses two minimalistic roles:

- `Admin`: Assign this role to all users that should **not** appear in the report because you already know they will require the new MFA.
- `Non Privileged User`: The default role for everyone else. Users that have one of the four permissions, will get flagged in the audit result.

The config simply checks, if a user who is not supposed to have one of the permissions, has it from their profile or one of the permission sets.

The audit results list all sources, so you know exactly where to look.

## Audit Your Org

1. Make sure you have the [Security Audit Engine](securityauditengine.org) installed.

```bash
sf plugins install @j-schreiber/sf-cli-security-audit
```

2. Customize the [inventory](privileged_perms/inventory/users.yml) and grant the "Admin" role to all users who should not be flagged. All other users are automatically set as "Non Privileged User" based on the user policy option.

3. Run the audit with `--verbose` to get a full list of all violations.

```bash
sf org audit run -o MyProdAlias -d privileged_perms --verbose
```