---
description: >-
  These are the current parameters for the Testnet, a test-only network for
  testing out upcoming features and changes to the protocol.
---

# The Testnet

{% hint style="danger" %}
**The Testnet may be subject to frequent version upgrades and state resets.**
{% endhint %}

{% hint style="info" %}
On the Testnet, TEST tokens are in use -- if you need some to test your clients, nodes or paratimes, feel free to request them on [**\#testnet** in the Community Slack](../oasis-network/connect-with-us.md). Note that these are test-only tokens and account balances, as any other state, may be frequently reset.
{% endhint %}

This page is meant to be kept up to date with the information from the currently released Testnet. Use the information here to deploy or upgrade your node on the Testnet.

* Latest Testnet version: **2021-02-03**
* [Genesis file](https://github.com/oasisprotocol/testnet-artifacts/releases/download/2021-02-03/genesis.json):
  * SHA1: `0af9dc476719c1294ceed7b0c271ae2adc8860f6`
  * SHA256: `703a18218f9aa1d268cc79a63fde9255984f5f8433c5ae4cdb1a1f1cf7ee76c1`
* Genesis document's hash \([explanation](../oasis-network/genesis-doc.md#genesis-file-vs-genesis-document)\):
  * `643fb06848be7e970af3b5b2d772eb8cfb30499c8162bc18ac03df2f5e22520e`
* Oasis seed node address:
  * `05EAC99BB37F6DAAD4B13386FF5E087ACBDDC450@34.86.165.6:26656`

{% hint style="success" %}
Feel free to use other seed nodes besides the one provided here.
{% endhint %}

* [Oasis Core](https://github.com/oasisprotocol/oasis-core) version:
  * [20.12.5](https://github.com/oasisprotocol/oasis-core/releases/tag/v20.12.5)

{% hint style="info" %}
The Oasis Node is part of the Oasis Core release.
{% endhint %}

## 2021-03-24 Upgrade

* **Upgrade height** upgrade is scheduled to happen at epoch **5128.**

{% hint style="info" %}
We expect the Testnet network to reach this epoch at around 2021-03-24 11:30 UTC.
{% endhint %}

#### Instructions

* \(optional\) To ensure your node will stop at epoch **5128** [submit the following upgrade descriptor](../run-a-node/maintenance-guides/handling-network-upgrades.md#stop-the-node-at-specific-epoch) at any time before the upgrade: 

  ```text
  {"name":"testnet-upgrade-2021-03-24","method":"internal","identifier":"testnet-upgrade-2021-03-24","epoch":5128}
  ```

* Download [Oasis Node](../run-a-node/prerequisites/) version [21.0.1](https://github.com/oasisprotocol/oasis-core/releases/tag/v21.0.1), while continuing to run version 20.12.x.
* Download the Testnet genesis file published in the [Testnet 2021-03-24 release](https://github.com/oasisprotocol/testnet-artifacts/releases/tag/2021-03-24).

{% hint style="info" %}
Testnet state at epoch **5128** will be exported and migrated to a 21.0.x compatible genesis file. Upgrade genesis file will be published on the above link soon after reaching the upgrade epoch.
{% endhint %}

* \(optional\) Verify the provided Testnet genesis file by comparing it to network state dump. See instructions in the [Handling Network Upgrades](../run-a-node/maintenance-guides/handling-network-upgrades.md#download-and-verify-the-provided-genesis-file) guide.
* Replace the old genesis file with the new Testnet genesis file.
* Stop your node \(if you haven't stopped it already by submitting the upgrade descriptor\).
* Remove the old 20.12.x version of Oasis Node.
* [Wipe state](../run-a-node/maintenance-guides/wiping-node-state.md#state-wipe-and-keep-node-identity).
* Update your node's configuration or perform any additional needed steps as per [Additional Steps](testnet.md#additional-steps) below.
* Start your node.

{% hint style="info" %}
For more detailed instructions, see the [Handling Network Upgrades](../run-a-node/maintenance-guides/handling-network-upgrades.md) guide.
{% endhint %}

#### Additional steps

Examine the [Changelog](https://github.com/oasisprotocol/oasis-core/blob/v21.0.1/CHANGELOG.md#210-2021-03-18) of the 21.0 release.

**Runtime operators**

In addition to some [configuration changes](https://github.com/oasisprotocol/oasis-core/blob/v21.0.1/CHANGELOG.md#configuration-changes), this upgrade contains breaking runtime API changes. Make sure any runtime code is updated and compatible with the 21.0.x runtime API version.

For this upgrade, the runtime node operators need to perform an additional migration of the storage nodes. Before starting the upgraded node, the storage database on all storage nodes needs to be migrated with the following command \(using the 21.0.1 binary\):

```text
oasis-node storage migrate \
  --datadir <NODE-DATADIR> \
  --runtime.supported <RUNTIME-ID>
```

{% hint style="info" %}
Depending on the runtime storage state size, the migration can take a long time.
{% endhint %}

{% hint style="warning" %}
**Storage access policy changes**

Due to the changes in the default access policy on storage nodes, at least one of the storage nodes should be configured with the`worker.storage.public_rpc.enabled` flag set to `true`.

Otherwise, external runtime clients wont be able to connect to any storage nodes.
{% endhint %}

