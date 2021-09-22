---
description: >-
  This document provides an overview of how to use the official Oasis Wallets: a
  non-custodial web wallet and a non-custodial browser extension that connect to
  the Oasis Network.
---

# Oasis Wallets

## **What are the official Oasis Wallets?**

The [Oasis Foundation](https://oasisprotocol.org/) supports two first-party noncustodial wallets, a [web wallet](https://wallet.oasisprotocol.org) named [Oasis Wallet -Web](https://github.com/oasisprotocol/oasis-wallet-web/) and a browser extension wallet named Oasis Wallet - Browser Extension. Both seamlessly connect to the [Oasis Network](../../oasis-network/overview.md) and makes it easy to hold, send, and receive ROSE tokens. 

These products are fully open source, built from the ground up by Oasis community developers, with funding from the Oasis Foundation’s [ROSE Bloom Grants Program](https://github.com/oasisprotocol/community/discussions/13). Both have also gone through multiple internal audits from the Oasis Foundation and [Oasis Labs](https://oasislabs.com/) teams and are currently going through an external audit as well.

## Where to find the official Oasis Wallets?

The [Oasis Wallet - Web](https://github.com/oasisprotocol/oasis-wallet-web/) can be found at [wallet.oasisprotocol.org](https://wallet.oasisprotocol.org). 

The [Oasis Wallet - Browser Extension](https://github.com/oasisprotocol/oasis-wallet-ext) can be found in the [Chrome Web Store](https://chrome.google.com/webstore/detail/oasis-wallet/ppdadbejkmjnefldpcdjhnkpbjkikoip).

## **Wallet features**

The Oasis Wallets currently support the following features:

* Creating a new wallet \(with a user friendly mnemonic recovery phrase\)
* Accessing an existing wallet using a mnemonic recovery phrase, private key, or a [Ledger](https://www.ledger.com/) device
* Viewing transaction history
* Submitting new transactions 
* Managing multiple accounts 
* Toggling between light mode and dark mode \(Web variant\)
* Selecting a language for the UI \(currently, English and French fort the Web variant, English and Chinese for the Browser Extension variant\)
* Staking and receiving staking rewards
* Easily switching between different Oasis Wallets that use the same [ADR 0008](https://github.com/oasisprotocol/oasis-core/blob/master/docs/adr/0008-standard-account-key-generation.md) standard account key generation process

## Frequently Asked Questions

### How can I transfer ROSE tokens from my [BitPie wallet](../holding-rose-tokens/mobile-wallets/bitpie-wallet-guide/) to my Oasis Wallet?

The easiest way would be to create a new wallet with an Oasis Wallet and just transfer the tokens from your old Oasis account to a new Oasis account. The cost \(i.e. transaction fee\) should be negligible.

If your tokens are staked/delegated, then you would need to debond them first which will take approximately 14 days. Then you would transfer them to a new account with an Oasis Wallet and stake/delegate them via an Oasis Wallet again.

{% hint style="warning" %}
The BitPie wallet currently doesn't use the standard account key generation process specified in [ADR 0008](https://github.com/oasisprotocol/oasis-core/blob/master/docs/adr/0008-standard-account-key-generation.md), so it is **not possible to switch** your **BitPie wallet Oasis account to** an **Oasis Wallet** just by importing BitPie's mnemonic.
{% endhint %}

{% page-ref page="web.md" %}

{% page-ref page="browser-extension.md" %}



