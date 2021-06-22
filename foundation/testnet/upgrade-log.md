---
description: >-
  For each upgrade of the Testnet network, we will track important changes for
  node operators' deployments.
---

# Upgrade Log

## 2021-06-23 Upgrade

* **Upgrade height:** upgrade is scheduled to happen at epoch **7553.**

{% hint style="info" %}
We expect the Testnet network to reach this epoch at around 2021-06-23 14:30 UTC.
{% endhint %}

### Instructions

* See [Before upgrade](upgrade-log.md#before-upgrade) section for required steps to be done before upgrade.
* \(optional\) Vote for the upgrade. On 2021-06-21 an upgrade proposal will be proposed which \(if accepted\) will schedule the upgrade on epoch **7553.** See the [Governance documentation](../../run-a-node/set-up-your-node/governance.md) for details on voting for proposals.

{% hint style="info" %}
The upgrade proposal contains the `"consensus-max-allowances-16"` upgrade handler whose only purpose is to set the**`staking.params.min_delegation`** consensus parameter to 16 \(default value is 0\) to enable support for beneficiary allowances which are required to transfer tokens into a ParaTime.
{% endhint %}

* Stop your node, replace the old version of Oasis Node with version [21.2.4](https://github.com/oasisprotocol/oasis-core/releases/tag/v21.2.4) and restart your node.

{% hint style="info" %}
Since Oasis Core 21.2.4 is otherwise compatible with the current consensus layer protocol, you may upgrade your Testnet node to this version at any time.
{% endhint %}

{% hint style="warning" %}
For this upgrade, do NOT wipe state.
{% endhint %}

* Once reaching the designated upgrade epoch, your node will stop and needs to be upgraded to Oasis Core [21.2.4](https://github.com/oasisprotocol/oasis-core/releases/tag/v21.2.4).
  * If you upgraded your node to Oasis Core 21.2.4 before the upgrade epoch was reached, you only need to restart your node for the upgrade to proceed.
  * Otherwise, you need to upgrade your node to Oasis Core 21.2.4 first and then restart it.

{% hint style="info" %}
The Testnet's genesis file and the genesis document's hash will remain the same.
{% endhint %}

### Before upgrade

This upgrade will upgrade Oasis Core to version **21.2.x** which includes the new [**BadgerDB**](https://github.com/dgraph-io/badger) **v3**.

Since BadgerDB's on-disk format changed in v3, it requires on-disk state migration. The migration process is done automatically and makes the following steps:

* Upon startup, Oasis Node will start migrating all `<DATA-DIR>/**/*.badger.db` files \(Badger v2 files\) and start writing Badger v3 DB to files with the `.migrate` suffix.
* If the migration fails in the middle, Oasis Node will delete all `<DATA-DIR>/**/*.badger.db.migrate` files the next time it starts and start the migration \(of the remaining `<DATA-DIR>/**/*.badger.db`

  files\) again.

* If the migration succeeds, Oasis Node will append the `.backup` suffix to all `<DATA-DIR>/**/*.badger.db` files \(Badger v2 files\) and remove the `.migrate` suffix from all `<DATA-DIR>/**/*.badger.db.migrate` files \(Badger v3 files\).

#### Extra storage requirements

Your node will thus need to have extra storage space to store both the old and the new BadgerDB files.

To see estimate how much extra space the migration will need, use the `du` tool:

```text
shopt -s globstar
du -h <DATA-DIR>/**/*.badger.db | sort -h -r
```

This is an example output from a Testnet node that uses `/srv/oasis/node` as the `<DATA-DIR>`:

```text
6.3G	/srv/oasis/node/tendermint/data/blockstore.badger.db
2.7G	/srv/oasis/node/tendermint/abci-state/mkvs_storage.badger.db
1.4G	/srv/oasis/node/tendermint/data/state.badger.db
158M	/srv/oasis/node/persistent-store.badger.db
164K	/srv/oasis/node/tendermint/abci-state/mkvs_storage.badger.db/checkpoints
80K	/srv/oasis/node/tendermint/abci-state/mkvs_storage.badger.db/checkpoints/4424334
80K	/srv/oasis/node/tendermint/abci-state/mkvs_storage.badger.db/checkpoints/4423334
76K	/srv/oasis/node/tendermint/abci-state/mkvs_storage.badger.db/checkpoints/4424334/fc815694d8219acb97fc0207a2159601df76df4d96802c147252ad0f2fd8a3f3
76K	/srv/oasis/node/tendermint/abci-state/mkvs_storage.badger.db/checkpoints/4423334/613e734e4ee4999bf71c3a190df13ea9d9b7d65af6a7fd8b2c9a477f2d052313
68K	/srv/oasis/node/tendermint/abci-state/mkvs_storage.badger.db/checkpoints/4424334/fc815694d8219acb97fc0207a2159601df76df4d96802c147252ad0f2fd8a3f3/chunks
68K	/srv/oasis/node/tendermint/abci-state/mkvs_storage.badger.db/checkpoints/4423334/613e734e4ee4999bf71c3a190df13ea9d9b7d65af6a7fd8b2c9a477f2d052313/chunks
20K	/srv/oasis/node/tendermint/data/evidence.badger.db
```

{% hint style="info" %}
After you've confirmed your node is up and running, you can safely delete all the  `<DATA-DIR>/**/*.badger.db.backup` files.
{% endhint %}

#### Extra memory requirements

BadgerDB v2 to v3 migration can use a number of Go routines to migrate different database files in parallel.

However, this comes with a memory cost. For larger database files, it might need up to 4 GB of RAM per database, so we recommend lowering the number of Go routines BadgerDB uses during migration \(`badger.migrate.num_go_routines`\) if your node has less than 8 GB of RAM.

If your node has less than 8 GB of RAM, set the number of Go routines BadgerDB uses during migration to 2 \(default is 8\) by adding the following to your node's `config.yml`:

```text
# BadgerDB configuration.
badger:
  migrate:
    # Set the number of Go routines BadgerDB uses during migration to 2 to lower
    # the memory pressure during migration (at the expense of a longer migration
    # time).
    num_go_routines: 2
```

## 2021-04-13 Upgrade

* **Upgrade height:** upgrade is scheduled to happen at epoch **5662.**

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

* **Upgrade height:** upgrade is scheduled to happen at epoch **5128.**

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

