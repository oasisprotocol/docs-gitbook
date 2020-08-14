# Ledger

[Ledger](https://www.ledger.com/) is a popular hardware wallet that is supported on the Oasis Network.

{% hint style="warning" %}
**This documentation section is out of date and needs to be updated to work with Oasis Core 20.9 and later.**
{% endhint %}

## Prerequisites

Before following this guide, make sure you've followed the [Operator Docs' Prerequisites Guide](../operator-docs/prerequisites-guide.md#installing-using-oasis-node) to install the `oasis-node` binary.

## Installation

To use the Oasis Ledger App, install it via [Ledger Live](https://www.ledger.com/ledger-live/)'s Manager. You will have to set developer mode to _on_ in order to find it.

{% hint style="warning" %}
The [Oasis Ledger App](https://github.com/Zondax/ledger-oasis) will not be available for installation via Ledger Live's Manager until after it has been approved by Ledger.
{% endhint %}

Once installed, the application will use the generated mnemonic stored on the Ledger device to generate a pair of private and public keys.

## Identifying the Ledger device

To identify your Ledger device for use with the oasis-node CLI, unlock your device and make sure you have the Oasis Ledger App open.

Run the following command to identify your device by the generated address:

```bash
oasis-node signer ledger list_devices
```

If your device is properly connected, you should see an output similar to the one below:

```text
============ Device found
Oasis App Version : 302e31322e33
Oasis App Address : oasis19hpt4y2reqwyfqcd53asjchdqf468chr673y6jn07xjp36w32jlscf0me
```

From now on, use the `Oasis App Address` to identify the Ledger device you want to use for signing.

For convenience, set the `OASIS_APP_ADDRESS` environment value to its value.

## Exporting the Public Key to Entity

Before you can sign anything, you need to export the public key from the device and use it to generate an entity. This will also be used later as another check to ensure that you are signing a transaction with the correct key.

Using the address you retrieved in the previous section , run the following:

```bash
oasis-node signer export \
  --signer.backend ledger \
  --signer.dir /localhostdir/entity/ \
  --signer.ledger.address $OASIS_APP_ADDRESS \
  --signer.ledger.index 1
```

{% hint style="info" %}
You can omit the `--signer.ledger.address` flag and `oasis-node` will try to connect to any available Ledger device.
{% endhint %}

This will create an `entity.json` file in `/localhostdir/entity/` that contains the public key generated on the device associated with `--signer.ledger.address`, derived from a path with an account index of `--signer.ledger.index`. The `--signer.backend ledger` flag is important here as it specifies use of the Ledger backend.

This command must be run anytime a new account index is to be used, with a new `/localhostdir/entity/` provided.

## Generating and Signing Transactions

As described in the [Operator Docs' Stake Management Guide](../operator-docs/stake-management.md#generating-and-submitting-transactions), you need to set the appropriate **base flags** and **signer flags** for each transaction you want to generate.

For convenience, you can set the `TX_FLAGS` environment variable like below \(replacing Ledger device's address index appropriately\):

```bash
TX_FLAGS="--genesis.file /localhostdir/genesis.json \
  --signer ledger \
  --signer.ledger.address $OASIS_APP_ADDRESS \
  --signer.ledger.index 1 \
  --signer.dir /localhostdir/entity/"
```

Then, you can generate and sign a transaction, e.g. a transfer transaction, by running:

```bash
oasis-node stake account gen_transfer \
  $TX_FLAGS \
  --stake.amount <AMOUNT-TO-TRANSFER> \
  --stake.transfer.destination <DESTINATION-ACCOUNT-ADDRESS> \
  --transaction.file tx_transfer.json \
  --transaction.nonce 1 \
  --transaction.fee.gas 1000 \
  --transaction.fee.amount 2000
```

where `<AMOUNT-TO-TRANSFER>` and `<DESTINATION-ACCOUNT-ADDRESS>` are replaced with the amount of tokens to transfer and the address of the transfer's destination account, respectively.

{% hint style="info" %}
For a more detailed explanation of the transaction flags that were set, see [Operator Docs' Stake Management Guide](../operator-docs/stake-management.md#common-transaction-flags).
{% endhint %}

Next, unlock your Ledger device and verify the transaction's fields on your device's screen.

After you've confirmed the transaction's fields are correct, sign the transaction on your Ledger device by double-pressing the _Sign transaction_ screen.

