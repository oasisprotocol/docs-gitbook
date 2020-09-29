---
description: >-
  Overview of the technical setup and stake requirements to become a validator
  on the Oasis Network.
---

# Run a Validator Node

[Oasis Network](../oasis-network/overview.md)'s Consensus Layer is a decentralised set of validator nodes that maintain a proof-of-stake blockchain.

Hence, it needs a set of distributed node operators that run different nodes \(including validator nodes\).

## Technical setup

Make sure your system meets the [Hardware](../run-a-node/prerequisites/hardware-recommendations.md) prerequisites and has [Oasis Node](../run-a-node/prerequisites/oasis-node.md) installed.

Then proceed by following the [Run a Validator Node](../run-a-node/set-up-your-node/run-validator.md) guide to:

* Create your entity.
* Initialize and configure your node.
* Register your entity on the network.

## Stake requirements

To become a validator on the Oasis Network, you need to have enough tokens staked in your escrow account.

{% hint style="info" %}
For more information about obtaining information on your entity's account, see the [Account Get Info](../use-your-tokens/account/get-info.md) doc.
{% endhint %}

Currently, you should have:

* 100 ROSE staked for your entity's registration since that is the [current entity's staking threshold](../pre-mainnet/genesis-file.md#node-and-runtime-token-thresholds).
* 100 ROSE staked for your validator node's registration since that is the [current validator node's staking threshold](../pre-mainnet/genesis-file.md#node-and-runtime-token-thresholds).
* Enough ROSE staked to be in the top 80 entities \(by stake\) so your validator will be elected into the consensus committee.

{% hint style="info" %}
The size of the consensus committee \(i.e. the validator set\) is configured by the [**max\_validators** consensus parameter](../pre-mainnet/genesis-file.md#consensus).
{% endhint %}

