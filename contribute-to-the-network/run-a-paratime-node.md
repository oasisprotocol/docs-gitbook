---
description: >-
  Overview of the requirements to become a compute node on a ParaTime connected
  to the Oasis Network.
---

# Run a ParaTime Node

The Oasis Network has two main components, the Consensus Layer and the ParaTime Layer.

1. The **Consensus Layer** is a scalable, high-throughput, secure, proof-of-stake consensus run by a decentralized set of validator nodes.
2. The **ParaTime Layer** hosts many parallel runtimes \(ParaTimes\), each representing a replicated compute environment with shared state.

![](../.gitbook/assets/image%20%281%29%20%282%29%20%281%29.png)

  
Operating a ParaTime requires the participation of node operators, who contribute nodes to the committee in exchange for rewards. ParaTimes can be operated by anyone, and have their own reward system, participation requirements, and structure.

As a node operator you can participate in any number of ParaTimes. While there are a number of ParaTimes under development, below are a few key ParaTimes that you can get involved in today. For operational documentation on running a ParaTime, please see the section on [running a ParaTime node for node operators](../run-a-node/set-up-your-node/run-a-paratime-node.md).

{% tabs %}
{% tab title="Cipher ParaTime" %}
## Cipher ParaTime

An Oasis Foundation developed ParaTime that will enable a bridge to Ethereum and Confidential Smart Contract support.

### Overview 

* **Leading Developer:** [Oasis Protocol Foundation](http://oasisprotocol.org/)
* **Status:** Planning incentivized testnet 
* **Tesnet Launch Date:** June 2021
* **Mainnet Launch Date:** TBA
* **Sign Up:** [here](https://oasisfoundation.typeform.com/to/I3wQ1CFJ)
* **Slack Channel:** [**\#**cipher-paratime](www.oasisprotocol.org/slack)
* **Requires SGX**: Yes
* **Parameters:**
  * [Testnet](../foundation/testnet/#cipher-paratime)

### Features

* Secure and stable 2-way token bridge between the Oasis Network and Ethereum with full support for the transfer of ROSE tokens from the Oasis Network to Ethereum 
* User-friendly web UI for swapping tokens
* Confidential Smart Contract support
* Fully decentralized with node operators distributed across the world
* Oasis **ROSE** tokens will be the native token used in the ParaTime for gas fees
* Support for WebAssembly smart contracts
* Support for confidential compute

### Rewards

* The ParaTime will release tokens on-chain to reward nodes for participation.
* The reward program is 2 years long, and tokens will be released per epoch.
* The reward will be 10~20 ROSE tokens per entity per epoch, where the Oasis Protocol Foundation will make the final decision on the reward size upon the testnet launch.
* Incentivized testnet with ROSE tokens delegated by the Oasis Protocol Foundation to testnet participants based on performance \(rewards can be found [here](https://oasis-foundation.medium.com/oasis-cipher-paratime-c9f40ae64946)\).
{% endtab %}

{% tab title="Oasis-Eth ParaTime" %}
## Oasis-Eth ParaTime

A self-contained EVM compatible ParaTime developed by Second State that allows developers to quickly build DApps on the Oasis Network. The Oasis-Eth ParaTime has support for the entire solidity toolchain, but currently is not connected to the Ethereum Network

### Overview 

* **Leading Developer:** [Second State](http://oasiseth.org/)
* **Status:** Open for Enrollment
* **Tesnet Launch Date:** January 2021 \(Complete\)
* **Mainnet Launch Date:** Late February 2021
* **Sign up:** [here](https://github.com/second-state/oasis-ssvm-runtime/wiki/Deploy-OasisEth-Paratime-Beta-on-the-oasis-mainnet)
* **Slack Channel:** [**\#**oasiseth-paratime](https://join.slack.com/t/oasiscommunity/shared_invite/enQtNjQ5MTA3NTgyOTkzLWIxNTg1ZWZmOTIwNmQ2MTg1YmU0MzgyMzk3OWM2ZWQ4NTQ0ZDJkNTBmMTdlM2JhODllYjg5YmJkODc2NzgwNTg)
* **Requires SGX:** No

### Features

* Self-contained, full-featured Ethereum development environment built on top of the Oasis Network to take advantage of scalability, affordability, and privacy benefits
* Full support for all current Ethereum 1.0 smart contracts, DApps, developer tools, and libraries via EVM runtime and tooling
* Full support for all next-gen Ethereum 2.0 smart contracts, DApps, developer tools, and libraries via Ewasm runtime and tooling
* Full support for Solidity
* Gas costs orders of magnitude lower than Ethereum
* Confidential smart contract support launching soon
* Additional features TBA

### Rewards

* OETH native gas tokens distributed as rewards for Oasis Ethereum ParaTime node operators
* There is a fixed total supply of 21 million OETH. It is deflationary as the network burns gas. There is no token sale, nor investors, nor pre-mine. All tokens are awarded over time to node operators, developers, and the community
* Additional rewards TBA

### Hardware Requirements

* 2.0 GHz x86-64 CPU
* CPU must have AES-NI support
* 2 GB ECC RAM \(4GB recommended\)
* 500 GB High Speed Storage \(SSD recommended\)
{% endtab %}
{% endtabs %}

Oasis Lab's Parcel ParaTime is also under development. If you're a developer and would like to start building applications on the Oasis Network with the Parcel SDK please sign up [here](https://www.oasislabs.com/parcelsdk).

