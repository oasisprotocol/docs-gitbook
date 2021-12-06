---
description: >-
  This document provides an overview of how to use the official Oasis Wallets: a
  non-custodial web wallet and a non-custodial browser extension that connect to
  the Oasis Network.
---

# Oasis Wallets

## **What are the official Oasis Wallets?**

The [Oasis Foundation](https://oasisprotocol.org) supports two first-party non-custodial wallets, a [web wallet](https://wallet.oasisprotocol.org) named [Oasis Wallet -Web](https://github.com/oasisprotocol/oasis-wallet-web/) and a browser extension wallet named Oasis Wallet - Browser Extension. Both seamlessly connect to the [Oasis Network](../../oasis-network/overview.md) and makes it easy to hold, send, and receive ROSE tokens.&#x20;

These products are fully open source, built from the ground up by Oasis community developers, with funding from the Oasis Foundation’s [ROSE Bloom Grants Program](https://github.com/oasisprotocol/community/discussions/13). Both have also gone through multiple internal audits from the Oasis Foundation and [Oasis Labs](https://oasislabs.com) teams and are currently going through an external audit as well.

## Where to find the official Oasis Wallets?

The [Oasis Wallet - Web](https://github.com/oasisprotocol/oasis-wallet-web/) can be found at [wallet.oasisprotocol.org](https://wallet.oasisprotocol.org).&#x20;

The [Oasis Wallet - Browser Extension](https://github.com/oasisprotocol/oasis-wallet-ext) can be found in the [Chrome Web Store](https://chrome.google.com/webstore/detail/oasis-wallet/ppdadbejkmjnefldpcdjhnkpbjkikoip).

## **Wallet features**

The Oasis Wallets currently support the following features:

* Creating a new wallet (with a user friendly mnemonic recovery phrase)
* Accessing an existing wallet using a mnemonic recovery phrase, private key, or a [Ledger](https://www.ledger.com) device
* Viewing transaction history
* Submitting new transactions&#x20;
* Managing multiple accounts&#x20;
* Toggling between light mode and dark mode (Web variant)
* Selecting a language for the UI (currently, English and French fort the Web variant, English and Chinese for the Browser Extension variant)
* Staking and receiving staking rewards
* Easily switching between different Oasis Wallets that use the same [ADR 0008](https://github.com/oasisprotocol/oasis-core/blob/master/docs/adr/0008-standard-account-key-generation.md) standard account key generation process

## Frequently Asked Questions

### How can I transfer ROSE tokens from my [BitPie wallet](../holding-rose-tokens/bitpie-wallet.md) to my Oasis Wallet?

{% hint style="warning" %}
BitPie wallet doesn't use the standardized account key generation process specified in [ADR 0008](https://github.com/oasisprotocol/oasis-core/blob/master/docs/adr/0008-standard-account-key-generation.md). Consequently, your **Bitpie wallet's mnemonic phrase will not open the same account in Oasis Wallet**.
{% endhint %}

The **preferred way** is to create a new wallet with an Oasis Wallet and transfer the tokens to this new address using your BitPie wallet. The cost (i.e. transaction fee) should be negligible.

If your tokens are staked/delegated, then you need to debond them first which will take approximately 14 days. Afterwards, you can transfer them to the new Oasis Wallet address and stake/delegate them via an Oasis Wallet again.

**Alternatively** however, if you do not want to transfer the tokens over the network, you can export the private key from your BitPie wallet and import it in Oasis Wallet in the following way by following:

* [How can I export my BitPie wallet's Oasis account private key?](./#how-can-i-export-my-bitpie-wallets-oasis-account-private-key)

### How can I export my [BitPie wallet](../holding-rose-tokens/bitpie-wallet.md)'s Oasis account private key?

On the main BitPie wallet screen, click on the "Receive" button.

![](../../.gitbook/assets/bitpie-mainscreen.png)

The QR code with your ROSE address will appear. Then, in the top right corner, tap on the kebab menu "⋮" and select "Display Private Key"_._

![](../../.gitbook/assets/bitpie-show-private-key.png)

BitPie wallet will now ask you to enter your PIN to access the private key.

Finally, your account's private key will be shown to you encoded in Base64 format (e.g. `YgwGOfrHG1TVWSZBs8WM4w0BUjLmsbk7Gqgd7IGeHfSqdbeQokEhFEJxtc3kVQ4KqkdZTuD0bY7LOlhdEKevaQ==`) which you can [import into Oasis Wallet](web.md#access-an-existing-wallet).

{% content-ref url="web.md" %}
[web.md](web.md)
{% endcontent-ref %}

{% content-ref url="browser-extension.md" %}
[browser-extension.md](browser-extension.md)
{% endcontent-ref %}

