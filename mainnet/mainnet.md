# Mainnet Overview

This document provides an overview of the proposed criteria and changes to upgrade from Mainnet Beta to Mainnet. This has been [reviewed and approved by community members and validators of the Oasis Network](https://github.com/oasisprotocol/community-forum/issues/1) and is being reproduced and summarized here for easy access.

{% hint style="warning" %}
As proposed by the community, Mainnet will launch on November 18, 2020 at 16:00 UTC.
{% endhint %}

## Criteria for Mainnet

In order to transition from Mainnet Beta to Mainnet, community members have collectively suggested the following criteria be met. This is a collection of community feedback.

* [x]  Validators representing more than 2/3 of stake in the initial consensus committee successfully get online to launch Mainnet Beta.
* [x]  Beta network runs successfully for at least 10 days.
* [x]  No major security risks on the Beta Network have been discovered or otherwise unremediated and untested in the past 10 days.
* [x]  At least 50 validators run on the Network.
  * _Throughout Mainnet Beta there have been between 75 and 77 active validators on the network._
* [x]  There are NO Oasis Protocol Foundation or Oasis Labs nodes serving as validators.
* [x]  At least one block explorer exists to track network stability, transactions, and validator activity.
  * _There is much more. See_ [_Block Explorers & Validator Leaderboards_](https://docs.oasis.dev/general/community-resources/community-made-resources#block-explorers-validator-leaderboards) _part of our docs._
* [x]  At least one qualified custodian supports the native ROSE token.
  * _Currently, Anchorage and Finoa support the ROSE token. See_ [_Custody Providers_](https://docs.oasis.dev/general/use-your-tokens/holding-tokens/custody-providers) _part of our docs._
* [x]  At least one web wallet or hardware wallet supports native ROSE token.
  * _Currently, Bitpie mobile wallet and RockX Ledger-backed web wallet are available and support ROSE token transfers. Support for staking and delegation is in development. See_ [_Mobile Wallets_](https://docs.oasis.dev/general/use-your-tokens/mobile-wallets) _and_ [_Web Wallets_](https://docs.oasis.dev/general/use-your-tokens/web-wallets) _parts of our docs._

## Mechanics of Upgrading to Mainnet

Upgrading from Mainnet Beta to Mainnet will require a coordinated upgrade of the Network. All nodes will need to configure a new genesis file that they can generate or verify independently and reset/archive any state from Mainnet Beta. Once enough \(representing 2/3+ of stake\) nodes have taken this step, the network will start.

## Proposed Changes From Mainnet Beta to Mainnet

The Mainnet genesis file is intended to be as close as possible to the state of the Mainnet Beta network at the time of upgrade. That includes retaining validator token balances, retaining genesis file wallet allocations, and block height at time of the snapshot.

In addition, after receiving additional feedback from the community, the Oasis Protocol Foundation has proposed to increase the staking rewards model. In the new proposed model staking rewards will start at 20% \(annualized\) and range from 20% to 2% over the first 4 years of the network \(see more in updated [Token Metrics and Distribution](https://docs.oasis.dev/oasis-network-primer/token-metrics-and-distribution) doc\).

{% hint style="info" %}
The following parts of the Genesis File will be updated:

* **`height`** will be updated to be the block height at the time of the snapshot on Mainnet Beta.
* **`genesis_time`** will be updated with the time of the Mainnet launch.
* **`chain_id`** will be set to `oasis-1`.
* **`halt_epoch`** will be set to `9940` \(approximately 1 year from Mainnet launch\).
* **`staking.params.disable_transfers`** will be set to **`false`** \(or omitted\) to enable transfers.
* **`staking.params.reward_schedule`** will be updated to reflect the updated reward schedule as mentioned above.
* Transfer from the Community and Ecosystem Wallet of 450M ROSE to the Common Pool to fund increased staking rewards.
{% endhint %}

See the updated [Network Parameters](../oasis-network/network-parameters.md) for the published Mainnet genesis file.

Mainnet will use [**Oasis Core 20.12.1**](https://github.com/oasisprotocol/oasis-core/releases/tag/v20.12.1).

## Mainnet Launch Support

The Oasis team will be offering live video support during the launch of Mainnet. Video call link and calendar details will be shared with node operators via email and Slack.

For any additional support, please reach out via the [**\#nodeoperators** Oasis Community Slack channel](../oasis-network/connect-with-us.md) with your questions, comments, and feedback related to Mainnet Beta.

To follow the network, please use one of the many community block explorers including [oasisscan.com](https://www.oasisscan.com/). 

