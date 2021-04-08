---
description: >-
  For each upgrade of the Testnet network, we will track important changes for
  node operators' deployments.
---

# Upgrade Log

## 2021-03-24 Upgrade

* **Upgrade height** upgrade is scheduled to happen at epoch **5128.**

{% hint style="info" %}
We expect the Testnet network to reach this epoch at around 2021-03-24 11:30 UTC.
{% endhint %}

### Instructions

* \(optional\) To ensure your node will stop at epoch **5128** [submit the following upgrade descriptor](../../run-a-node/maintenance-guides/handling-network-upgrades.md#stop-the-node-at-specific-epoch) at any time before the upgrade: 

  ```text
  {
    "name": "testnet-upgrade-2021-03-24",
    "method": "internal",
    "identifier": "testnet-upgrade-2021-03-24",
    "epoch": 5128
  }
  ```

* Download the Testnet genesis file published in the [Testnet 2021-03-24 release](https://github.com/oasisprotocol/testnet-artifacts/releases/tag/2021-03-24).

{% hint style="info" %}
Testnet state at epoch **5128** will be exported and migrated to a 21.0.x compatible genesis file. Upgrade genesis file will be published on the above link soon after reaching the upgrade epoch.
{% endhint %}

* \(optional\) Verify the provided Testnet genesis file by comparing it to network state dump. See instructions in the [Handling Network Upgrades](../../run-a-node/maintenance-guides/handling-network-upgrades.md#download-and-verify-the-provided-genesis-file) guide.
* Replace the old genesis file with the new Testnet genesis file.
* Stop your node \(if you haven't stopped it already by submitting the upgrade descriptor\).
* Replace the old version of Oasis Node with version [21.0.1](https://github.com/oasisprotocol/oasis-core/releases/tag/v21.0.1).
* Update your node's configuration or perform any additional needed steps as per [Additional Steps](./#additional-steps) below.
* [Wipe state](../../run-a-node/maintenance-guides/wiping-node-state.md#state-wipe-and-keep-node-identity).
* Start your node.

{% hint style="info" %}
For more detailed instructions, see the [Handling Network Upgrades](../../run-a-node/maintenance-guides/handling-network-upgrades.md) guide.
{% endhint %}

### Additional steps

Examine the [Changelog](https://github.com/oasisprotocol/oasis-core/blob/v21.0.1/CHANGELOG.md#210-2021-03-18) of the 21.0 release.

**Runtime operators**

In addition to some [configuration changes](https://github.com/oasisprotocol/oasis-core/blob/v21.0.1/CHANGELOG.md#configuration-changes), this upgrade contains breaking runtime API changes. Make sure any runtime code is updated and compatible with the 21.0.x runtime API version.

{% hint style="danger" %}
**Backup your node's data directory**

To prevent irrecoverable runtime storage data corruption/loss in case of a failed storage migration, backup your node's data directory.

For example, to backup the `/serverdir/node` directory using the rsync tool, run:

```text
rsync /serverdir/node/ /serverdir/node-BACKUP/
```
{% endhint %}

For this upgrade, the runtime node operators need to perform an additional migration of the storage nodes. **Before starting the upgraded node and before wiping state**, the storage database on all storage nodes needs to be migrated with the following command \(using the 21.0.1 binary\):

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

