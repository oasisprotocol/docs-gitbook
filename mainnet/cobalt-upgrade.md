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

For the actual steps that node operators need to make on their nodes, see the [Upgrade Log](../run-a-node/upgrade-log.md#2021-04-28-16-00-utc-cobalt-upgrade).

## Proposed State Changes

The following parts of the genesis file will be updated:

* **`height`** will be set to the height of the Mainnet state dump + 1, i.e.`3027601`.
* **`genesis_time`** will be set to`2021-04-28T16:00:00Z`.
* **`chain_id`** will be set to `oasis-2`.
* **`halt_epoch`** will be set to`13807`\(approximately 1 year from Cobalt upgrade\).
* **`epochtime`**section will be removed \(it will be replaced by the **`beacon`** section\).

#### **Registry backend parameters**	

* **`registry.params.enable_runtime_governance_models`**will be set to:

  ```text
  {
    "entity": true,
    "runtime": true
  }
  ```

* Runtimes will be automatically migrated.
* Some inactive entities/nodes might be removed due to not passing stake claims.

#### **Roothash backend parameters**

* **`roothash.params.max_runtime_messages`**will be set to 256.
* **`roothash.params.max_evidence_age`**will be set to 100.

#### **Staking backend parameters**

* **`staking.params.governance_deposits`**will be set to `0`.
* **`staking.params.allow_escrow_messages`**will be set to`true`.
* **`staking.params.slashing.consensus-light-client-attack.amount`** will be set to `100000000000`.
* **`staking.params.slashing.consensus-light-client-attack.freeze_interval`** will be set to `18446744073709551615`.

#### **Beacon backend parameters**

* **`beacon.base`**will be set to`5047`.
* **`beacon.params.backend`**will be set to`"pvss"`.
* **`beacon.params.pvss_parameters.participants`**will be set to`20`.
* **`beacon.params.pvss_parameters.threshold`**will be set to`10`.
* **`beacon.params.pvss_parameters.commit_interval`**will be set to`399`.
* **`beacon.params.pvss_parameters.reveal_interval`**will be set to`200`.
* **`beacon.params.pvss_parameters.transition_delay`** __will be set to`1`.

#### **Scheduler backend parameters**

* **`scheduler.params.max_validators`**will be set to`100`.

#### **Governance backend parameters**

* **`governance.params.voting_period`**will be set to`168`.
* **`governance.params.upgrade_min_epoch_diff`**will be set to`336`.
* **`governance.params.upgrade_cancel_min_epoch_diff`**will be set to`192`.
* **`governance.params.quorum`**will be set to`75`.
* **`governance.params.threshold`**will be set to `90`.
* **`governance.params.min_proposal_deposit`**will be set to `10000000000000`.

#### **Consensus backend parameters**

* **`consensus.params.max_evidence_num`** will be removed.
* **`consensus.params.max_evidence_size`**will be set to`51200`.
* **`consensus.params.state_checkpoint_interval`**will be set to`10000`.
* **`consensus.params.state_checkpoint_num_kept`**will be set to`2`.
* **`consensus.params.state_checkpoint_chunk_size`**will be set to`8388608`.

#### Other

* **`extra_data`** will be set back to the value in the [Mainnet genesis file](https://github.com/oasisprotocol/mainnet-artifacts/releases/tag/2020-11-18) ****to include the Oasis network's genesis quote: _”_[_Quis custodiet ipsos custodes?_](https://en.wikipedia.org/wiki/Quis_custodiet_ipsos_custodes%3F)_” \[submitted by Oasis Community Member Daniyar Borangaziyev\]:_ 

  ```text
  "extra_data": {
    "quote": "UXVpcyBjdXN0b2RpZXQgaXBzb3MgY3VzdG9kZXM/IFtzdWJtaXR0ZWQgYnkgT2FzaXMgQ29tbXVuaXR5IE1lbWJlciBEYW5peWFyIEJvcmFuZ2F6aXlldl0="
  }
  ```

## Launch Support

The Oasis team will be offering live video support during the Cobalt upgrade. Video call link and calendar details will be shared with node operators via email and Slack.

For any additional support, please reach out via the [**\#nodeoperators** Oasis Community Slack channel](../oasis-network/connect-with-us.md) with your questions, comments, and feedback related to Mainnet Beta.

To follow the network, please use one of the many [community block explorers](../community-resources/community-made-resources.md#block-explorers-validator-leaderboards).

