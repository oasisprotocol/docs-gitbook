---
description: >-
  For each upgrade of the Testnet network, we will track important changes for
  node operators' deployments.
---

# Upgrade Log

## 2021-04-13 Upgrade

* **Upgrade height** upgrade is scheduled to happen at epoch **5662.**

{% hint style="info" %}
We expect the Testnet network to reach this epoch at around 2021-04-13 12:00 UTC.
{% endhint %}

### Instructions

* Runtime operators see [Before upgrade](upgrade-log.md#before-upgrade) section for required steps to be done before upgrade.
* \(optional\) Vote for the upgrade. On 2021-04-12 an upgrade proposal will be proposed which \(if accepted\) will schedule a network shutdown on epoch **5662.** See the [Governance documentation](../../run-a-node/set-up-your-node/governance.md) for details on voting for proposals.

{% hint style="warning" %}
The upgrade proposal contains a non-existing upgrade handler and will be used to coordinate the network shutdown, the rest of the upgrade is manual.
{% endhint %}

Following steps should be performed only after the network has reached the upgrade network and has halted:

* Download the Testnet genesis file published in the [Testnet 2021-04-13 release](https://github.com/oasisprotocol/testnet-artifacts/releases/tag/2021-04-13).

{% hint style="info" %}
Testnet state at epoch **5662** will be exported and migrated to a 21.1.x compatible genesis file. Upgrade genesis file will be published on the above link soon after reaching the upgrade epoch.
{% endhint %}

* Replace the old genesis file with the new Testnet genesis file.
* Replace the old version of Oasis Node with version [21.1](https://github.com/oasisprotocol/oasis-core/releases/tag/v21.1).
* [Wipe state](../../run-a-node/maintenance-guides/wiping-node-state.md#state-wipe-and-keep-node-identity).
* Start your node.

### Before upgrade

**Runtime operators**

This upgrade requires a runtime storage node migration to be performed **before the upgrade genesis is published**. This can be done before the upgrade epoch is reached by stopping all runtime nodes and running the migration.

{% hint style="danger" %}
**Backup your node's data directory**

To prevent irrecoverable runtime storage data corruption/loss in case of a failed storage migration, backup your node's data directory.

For example, to backup the `/serverdir/node` directory using the rsync tool, run:

```text
rsync -a /serverdir/node/ /serverdir/node-BACKUP/
```
{% endhint %}

The storage database on all storage nodes needs to be migrated with the following command \(using the [21.1](https://github.com/oasisprotocol/oasis-core/releases/tag/v21.1) binary\):

```text
oasis-node storage migrate \
  --datadir <NODE-DATADIR> \
  --runtime.supported <RUNTIME-ID>
```

After the migration to v5 completes, you will see an output similar to:

```text
...
- migrating from v4 to v5...
- migrating version 24468...
- migrated root state-root:195cf7a9a103e7300b2bb4e537cb9935cbebd83e448e67aa55433861a6ad7426 -> state-root:cea105a5d701deab935b94af9e8e0c5af5dcdb61c242bf434da9f11aa8d110ba
- migrated root io-root:0850c5a33ee7f45aa92724b7d5f28c9ac9ae8799b88cc5be9773e8aba9526ca7 -> io-root:19713a2b44e1bf868ebee43c36872baa3058870bb890a5e25d1c4cea2622be77
- migrated root io-root:477391131f60ac2c22bce9167c7e3783a13d4fb81fddd2d388b4ead6a586fe52 -> io-root:f29f86d491303c5fd7b3572e97cbd65b7487b6b4ac519623afd161cc2e4678b7
```

Take note of the displayed `state-root` and report it to the Foundation, as it needs to be included in the upgrade's new genesis file. Keep the runtime nodes stopped until the upgrade epoch is reached. At upgrade epoch, upgrade the nodes by following the remaining steps above.

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
rsync -a /serverdir/node/ /serverdir/node-BACKUP/
```
{% endhint %}

For this upgrade, the runtime node operators need to perform an additional migration of the storage nodes. **Before starting the upgraded node and before wiping state**, the storage database on all storage nodes needs to be migrated with the following command \(using the 21.0.1 binary\):

```text
oasis-node storage migrate \
  --datadir <NODE-DATADIR> \
  --runtime.supported <RUNTIME-ID>
```

{% hint style="warning" %}
**Storage access policy changes**

Due to the changes in the default access policy on storage nodes, at least one of the storage nodes should be configured with the`worker.storage.public_rpc.enabled` flag set to `true`.

Otherwise, external runtime clients wont be able to connect to any storage nodes.
{% endhint %}

