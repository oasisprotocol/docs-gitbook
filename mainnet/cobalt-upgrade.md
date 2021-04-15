# Cobalt Upgrade

This document provides an overview of the proposed criteria and changes for the Cobalt Mainnet upgrade. This has been [reviewed and approved by community members and validators of the Oasis Network](https://github.com/oasisprotocol/community/discussions/18) and is being reproduced and summarized here for easy access.

{% hint style="warning" %}
As proposed by the community, the Cobalt upgrade on Mainnet will kick off around April 28, 2021 at 16:00 UTC.
{% endhint %}

## Major Features

All features for the Cobalt upgrade are implemented as part of **Oasis Core 21.1.x** which is a protocol-breaking release. Summary of the major features is as follows:

* **Light Clients and Checkpoint Sync**: In order to make bootstrapping of new network nodes much faster, the upgrade will introduce support for light clients and restoring state from checkpoints provided by other nodes in the network.
* **Random Beacon**: The random beacon is used by the consensus layer for ParaTime committee elections and is a critical component in providing security for ParaTimes with an open admission policy. The improved random beacon implementation is based on SCRAPE and provides unbiased output as long as at least one participant is honest.
* **On-Chain Governance**: The new on-chain governance service provides a simple framework for submitting governance proposals, validators voting on proposals and once an upgrade proposal passes, having a way to perform the upgrade in a controlled manner which minimizes downtime.
* **Support for Beneficiary Allowances**: Each account is able to configure beneficiaries which are allowed to withdraw tokens from it in addition to the account owner.
* **ROSE Transfers Between the Consensus Layer and ParaTimes**: The proposed upgrade introduces a mechanism where ParaTimes can emit messages as part of processing any ParaTime block. These messages can trigger operations in the consensus layer on the ParaTime's behalf. ParaTimes get their own accounts in the consensus layer which can hold ROSE and ParaTimes are able to request them be transferred to other accounts or to withdraw from other accounts when allowed via the allowances mechanism.
* **A Path Towards Self-Governing ParaTimes**: By building on the ParaTime messages mechanism, the proposed upgrade extends ParaTime governance options and enables a path towards ParaTimes that can define their own governance mechanisms.

In addition to the specified additional features we also propose the validator set size to be increased from the current 80 to 100 validators as [suggested earlier by the community](https://github.com/oasisprotocol/community/discussions/5#discussioncomment-282746).

## Mechanics of the Upgrade

{% hint style="info" %}
This section will be updated with more details as we get closer to the upgrade.
{% endhint %}

Upgrading the Mainnet will require a coordinated upgrade of the Network. All nodes will need to configure a new genesis file that they can generate or verify independently and reset/archive any state from Mainnet. Once enough \(representing 2/3+ of stake\) nodes have taken this step, the upgraded network will start.

## Proposed State Changes

{% hint style="info" %}
This section will be updated with more details about the proposed state changes that will be performed at the time of the upgrade.
{% endhint %}

## Launch Support

The Oasis team will be offering live video support during the Cobalt upgrade. Video call link and calendar details will be shared with node operators via email and Slack.

For any additional support, please reach out via the [**\#nodeoperators** Oasis Community Slack channel](../oasis-network/connect-with-us.md) with your questions, comments, and feedback related to Mainnet Beta.

To follow the network, please use one of the many [community block explorers](../community-resources/community-made-resources.md#block-explorers-validator-leaderboards).

