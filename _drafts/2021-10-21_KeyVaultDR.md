---
layout: post
title:  "Azure Key Vault DR Strategy Woes"
date:   2021-10-21 23:00:00 -0500
categories: Azure KeyVault DR HSM Certificates Encryption-Keys
---

# Azure Key Vault DR Strategy Woes
Imagine a scenario where you want to control your own destiny (failover) in the event of an Azure regional failure.  It is notable that Microsoft guarantees that the contents of your vault are [replicated to a secondary region > 150 miles away](https://docs.microsoft.com/en-us/azure/key-vault/general/disaster-recovery-guidance) so rest assured your data will be backed up. Aside from the obvious limitations that Microsoft lists for you, have you considered  other factors that may further impact your DR strategy. Do you have full control over the decision to declare a disaster in your own infrastructure and perform your own application failover to another region?

# Questions you may ask yourself further
## What constitutes a failover event in the Azure Key Vault?
In the event of a true disaster microsoft will initiate the regional failover. What they neglect to tell you is the indicators at which this decision is made.  How long of an outage in one region constitutes a failover event? Is the decision manual?  Will you even be alerted that it is occurring or just notice your failover is causing requests to fail. 

## What constitutes a disaster event FOR YOU?
We have seen Azure outages go on for hours without initiating a failover (or maybe the failover infrastructure was impacted too) [`TODO - source`].  It may be that your tolerance for an outage is much less then the [`"up to 20 minutes"`](https://docs.microsoft.com/en-us/azure/key-vault/general/disaster-recovery-guidance) it will take for Private Link to re-establish!  

## What control do you have over the way the failover is handled?
If Microsoft has to make a game-time decision(which may include accepting data loss for the greater good)... will you be OK with that? [`TODO - source DB regional data loss `]

# Design Considerations

If the above thoughts provoked some concern, fear not - we have some options to go forward. 

## Azure Key Vault (non pooled)

In the basic Azure Key Vault model, we can control our destiny in the event of a disaster by explicitly building a secondary vault in the region of our choosing and automating the replication of data between our primary and secondary.  The PAAS cost model for Key Vault is extraordinary - you do not pay per vault, only a small cost per operation.  Unfortunately there is no command to backup/restore the entire vault, so this must be done secret by secret.  As such we can stand up a secondary vault and automate the replication of our secrets to it.  Kicking off that automation may become challenging if your application has functionality to update the vault during runtime.  Otherwise, a simple script to replicate data from the primary to the secondary vaults can be created to meet your application replication and secret naming needs (see also [marketplace tasks to do this natively in a DevOps Pipeline](https://marketplace.visualstudio.com/items?itemName=cboroson.cboroson-KeyVault-Replication#:~:text=Azure%20does%20not%20yet%20offer%20a%20way%20to,part%20of%20the%20recovery%20of%20an%20entire%20region.)).

### CAVEATS
Ah yes, the wisdom piece of this article - if you made this far you might want to consider some "gotchas".  Namely -  you make use of HSM backed keys or certificates ... here be dragons.  If you follow best practice and enable soft delete and purge protection  the above strategy will not work out well for you.  HSM backed keys can only be restored a single time, subsequent restores (in the event of a rotation will fail).  This scenario is explained in more detail here [`TODO - link to other article`]

Also, as mentioned above this mechanism introduces complexity as  the backup and restore must be done secret by secret.

## Azure Key Vault Managed HSM

The Managed HSM has a much more robust set of DR options.  Namely you can backup the entire contents + metadata + rbac to blob storage and perform a restore.

### CAVEATS
Restores are ["destructive and disruptive"](https://docs.microsoft.com/en-us/azure/key-vault/managed-hsm/backup-restore#full-restore)