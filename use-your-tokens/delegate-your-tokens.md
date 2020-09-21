# Delegate Your Tokens

### What is a Delegation

You can _delegate_ your tokens by submitting an _escrow_ transaction that deposits a specific number of tokens into someone else’s escrow account \(as opposed to _staking_ tokens, which usually refers to depositing tokens into your own escrow account\).

In other words, delegating your tokens is equivalent to staking your tokens in someone else's node. Delegating your tokens can give you the opportunity to participate in the Oasis Network's proof-of-stake consensus system and earn rewards via someone else's node.

At this time you need to use the `oasis-node` CLI to perform the delegation. It can use either the file-based signer or a Ledger hardware wallet to sign the escrow transaction. Regardless of which signer you use, the commands that you need to run are similar, but the initial setup process is different.

### Delegation via the `oasis-node` CLI

The `oasis-node` binary offers a command line interface for various staking operations, including the escrow operation needed to delegate tokens.

#### File-based Signer

If you do not have access to or prefer not to use a Ledger device, you can sign your transactions with your entity's private key stored in a file.

{% hint style="danger" %}
When using the file-based signer the use of an [offline/air-gapped machine](https://en.wikipedia.org/wiki/Air_gap_%28networking%29) for this purpose is highly recommended. Gaining access to the entity private key can compromise your tokens.
{% endhint %}

You will need to create your Entity as described in [Running a Node on the Amber Network](../run-a-node/set-up-your-node/running-a-node.md#creating-your-entity) docs and set the following flags:

* `--signer.backend file`: Specifies use of the file signer.

{% hint style="info" %}
Currently, `file` is the default signer so you could also omit this flag.
{% endhint %}

* `--signer.dir`: Path to entity's artifacts directory, e.g. `/localhostdir/entity/`.

An example of an escrow transaction can be found in our [node operator stake management doc](../run-a-node/set-up-your-node/stake-management.md#escrowing-tokens). To delegate your tokens to someone else's node, simply submit an escrow transaction to the escrow account address corresponding to the _entity_ that owns that node.

#### Ledger Hardware Wallet Signer

For any Oasis Network user with access to a [Ledger hardware wallet device](https://www.ledger.com/), the Ledger device can be used to sign transactions.

You will need to set it up as described in our [Ledger docs](https://docs.oasis.dev/oasis-core-ledger/usage/setup) and then [generate](https://docs.oasis.dev/oasis-core-ledger/usage/transactions) and submit an [escrow transaction](../run-a-node/set-up-your-node/stake-management.md#escrowing-tokens).

### Rewards and Slashing

By delegating your tokens to someone else's node, you can earn a portion of the rewards earned by that node through its participation in the Oasis Network. Your delegated tokens can also be slashed if that node gets slashed for double signing. You can learn more about how rewards and slashing impact delegators in our [incentives document]().

