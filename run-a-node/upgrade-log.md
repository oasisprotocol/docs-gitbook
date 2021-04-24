---
description: >-
  For each upgrade of the network, we will track important changes for node
  operators' deployments.
---

# Upgrade Log

## 2021-04-28 \(16:00 UTC\) - Cobalt Upgrade

* **Upgrade height** upgrade is scheduled to happen at epoch **5046.**

{% hint style="info" %}
We expect the Mainnet network to reach this epoch at around 2021-04-28 12:00 UTC.
{% endhint %}

### Instructions - Before upgrade

* Make sure you are running the latest Mainnet-compatible Oasis Node version: [20.12.7](https://github.com/oasisprotocol/oasis-core/releases/tag/v20.12.7).
  * If you are running a different **20.12.x** Oasis Node version, update to version **20.12.7** before the upgrade.

{% hint style="success" %}
Version **20.12.7** is backwards compatible with other **20.12.x** releases, so upgrade can be performed at any time by stopping the node and replacing the binary.
{% endhint %}

* To ensure your node will stop at epoch **5046** [submit the following upgrade descriptor](maintenance-guides/handling-network-upgrades.md#stop-the-node-at-specific-epoch) at any time before the upgrade: 

  ```text
  {
    "name": "mainnet-upgrade-2021-04-28",
    "method": "internal",
    "identifier": "mainnet-upgrade-2021-04-28",
    "epoch": 5046
  }
  ```

{% hint style="warning" %}
The upgrade descriptor contains a non-existing upgrade handler and will be used to coordinate the network shutdown, the rest of the upgrade is manual.
{% endhint %}

#### **Runtime operators**

Following section is relevant only for runtime operators. 

This upgrade requires a runtime storage node migration to be performed **before the upgrade genesis is published**. This can be done before the upgrade epoch is reached by stopping all runtime nodes and running the migration.

**Backup your node's data directory**

To prevent irrecoverable runtime storage data corruption/loss in case of a failed storage migration, backup your node's data directory.

For example, to backup the `/serverdir/node` directory using the [rsync](https://rsync.samba.org/) tool, run:

```text
rsync -a /serverdir/node/ /serverdir/node-BACKUP/
```

The storage database on all storage nodes needs to be migrated with the following command \(using the [21.1.1](https://github.com/oasisprotocol/oasis-core/releases/tag/v21.1.1) binary\):

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

### Instructions - Upgrade day

Following steps should be performed on **2021-04-28** only after the network has reached the upgrade epoch and has halted:

* Download the genesis file published in the [Cobalt Upgrade release](https://github.com/oasisprotocol/mainnet-artifacts/releases/tag/2021-04-28).

{% hint style="info" %}
Mainnet state at epoch **5046** will be exported and migrated to a 21.1.x compatible genesis file. Upgrade genesis file will be published on the above link soon after reaching the upgrade epoch.
{% endhint %}

* Verify the provided Cobalt upgrade genesis file by comparing it to network state dump. See instructions in the [Handling Network Upgrades](maintenance-guides/handling-network-upgrades.md#download-and-verify-the-provided-genesis-file) guide.
* Replace the old genesis file with the new Cobalt upgrade genesis file.
* Stop your node \(if you haven't stopped it already by submitting the upgrade descriptor\).
* Replace the old version of Oasis Node with version [21.1.1](https://github.com/oasisprotocol/oasis-core/releases/tag/v21.1.1).
* [Wipe state](maintenance-guides/wiping-node-state.md#state-wipe-and-keep-node-identity).
* Start your node.

{% hint style="info" %}
For more detailed instructions, see the [Handling Network Upgrades](maintenance-guides/handling-network-upgrades.md) guide.
{% endhint %}

### Additional notes

Examine the [Change Log](https://github.com/oasisprotocol/oasis-core/blob/v21.1.1/CHANGELOG.md) of the 21.1.1 \(and 21.0\) releases.

**Runtime operators**

Note the following configuration change in the [21.0](https://github.com/oasisprotocol/oasis-core/blob/v21.1.1/CHANGELOG.md#configuration-changes) release.

{% hint style="warning" %}
**Storage access policy changes**

Due to the changes in the default access policy on storage nodes, at least one of the storage nodes should be configured with the`worker.storage.public_rpc.enabled` flag set to `true`.

Otherwise, external runtime clients wont be able to connect to any storage nodes.
{% endhint %}

## 2020-11-18 \(16:00 UTC\) - Mainnet

* **Block height** when Mainnet Beta network stops: **702000.**

{% hint style="info" %}
We expect the Mainnet Beta network to reach this block height at around 2020-11-18 13:30 UTC.
{% endhint %}

* **Upgrade window:**
  * Start: **2020-11-18T16:00:00Z.**
  * End: After nodes representing **2/3+ stake** do the upgrade.

### Instructions

* Download [Oasis Node](prerequisites/oasis-node.md) version [20.12.2](https://github.com/oasisprotocol/oasis-core/releases/tag/v20.12.2), while continuing to run version 20.10.x.
* \(optional\) Use Oasis Node version 20.12.2 to dump network state at the specified block height. It will connect to the running version 20.10.x node.
* Download the Mainnet genesis file published in the [2020-11-18 release](https://github.com/oasisprotocol/mainnet-artifacts/releases/tag/2020-11-18).
* \(optional\) Verify the provided Mainnet genesis file by comparing it to network state dump. See instructions in the [Handling Network Upgrades](maintenance-guides/handling-network-upgrades.md#download-and-verify-the-provided-genesis-file) guide.
* Replace the old Mainnet Beta genesis file with the Mainnet genesis file.
* Stop your node.
* Remove the old 20.10.x version of Oasis Node.
* [Wipe state](maintenance-guides/wiping-node-state.md#state-wipe-and-keep-node-identity).
* Update your node's configuration per instructions in [Configuration changes](upgrade-log.md#configuration-changes) below.
* Start your node.

{% hint style="info" %}
This time, we recommend dumping the network state with the upgraded Oasis Node binary so that the genesis file will be in the [canonical form](https://docs.oasis.dev/oasis-core/high-level-components/index/genesis#canonical-form).

The canonical form will make it easier to compare the obtained genesis file with the one provided by us.
{% endhint %}

{% hint style="info" %}
For more detailed instructions, see the [Handling Network Upgrades](maintenance-guides/handling-network-upgrades.md) guide.
{% endhint %}

### Configuration changes

Since we are upgrading to the Mainnet, we recommend you change your node's configuration and disable pruning of the consensus' state by removing the `consensus.tendermint.abci.prune` key.

For example, this configuration:

```yaml
...

# Consensus backend.
consensus:
  # Setting this to true will mean that the node you're deploying will attempt
  # to register as a validator.
  validator: true

  # Tendermint backend configuration.
  tendermint:
    abci:
      prune:
        strategy: keep_n
        # Keep ~7 days of data since block production is ~1 block every 6 seconds.
        # (7*24*3600/6 = 100800)
        num_kept: 100800
    core:
      listen_address: tcp://0.0.0.0:26656

    ...
```

Becomes:

```yaml
...

# Consensus backend.
consensus:
  # Setting this to true will mean that the node you're deploying will attempt
  # to register as a validator.
  validator: true

  # Tendermint backend configuration.
  tendermint:
    core:
      listen_address: tcp://0.0.0.0:26656

    ...
```

## 2020-10-01 - Mainnet Beta

### Instructions

* Stop your node.
* [Wipe state](maintenance-guides/wiping-node-state.md#state-wipe-and-keep-node-identity).
* Replace the old genesis file with the Mainnet Beta genesis file published in the [2020-10-01 release](https://github.com/oasisprotocol/mainnet-artifacts/releases/tag/2020-10-01).
* Start your node.

{% hint style="info" %}
You should keep using Oasis Core version 20.10.
{% endhint %}

{% hint style="info" %}
For more detailed instructions, see the [Handling Network Upgrades](maintenance-guides/handling-network-upgrades.md) guide.
{% endhint %}

## 2020-09-22 - Mainnet Dry Run

### Instructions

* This is the initial deployment.

