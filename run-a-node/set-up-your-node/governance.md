# Governance

{% hint style="info" %}
This section describes functionality which is currently only available on the Testnet and will only be enabled on Mainnet as part of the Cobalt upgrade.
{% endhint %}

{% hint style="info" %}
This example assumes you have read and followed the instructions in the [Prerequisites](../../manage-tokens/oasis-cli-tools/prerequisites.md) and [Setup](../../manage-tokens/oasis-cli-tools/setup.md) sections of the _Use Your Tokens_ docs.
{% endhint %}

## Listing Active Proposals

In order to list all active governance proposals, you can use the following command:

```bash
oasis-node governance list_proposals -a $ADDR
```

In case there are currently any active proposals this should return a list of them similar to the following:

```javascript
[{
    "id":1,
    "submitter":"oasis1qrs2dl6nz6fcxxr3tq37laxlz6hxk6kuscnr6rxj",
    "state":"active",
    "deposit":"10000000000000",
    "content":{
        "upgrade":{
            "v":1,
            "handler":"1304_testnet_upgrade",
            "target":{
                "runtime_host_protocol":{"major":2},
                "runtime_committee_protocol":{"major":2},
                "consensus_protocol":{"major":4}
            },
            "epoch":5662
        }
    },
    "created_at":5633,
    "closes_at":5645
}]
```

##  Voting for a Proposal

{% hint style="info" %}
At this time only entities which have active validator nodes scheduled in the validator set are eligible to vote for governance proposals.
{% endhint %}

If you want to vote for an active proposal, you can use the following command to generate a suitable transaction:

```bash
oasis-node governance gen_cast_vote \
  "${TX_FLAGS[@]}" \
  --vote.proposal.id 1 \
  --vote yes \
  --transaction.file tx_cast_vote.json \
  --transaction.nonce 1 \
  --transaction.fee.gas 2000 \
  --transaction.fee.amount 2000
```

This will output a preview of the generated transaction:

```bash
You are about to sign the following transaction:
  Method: governance.CastVote
  Body:
    Proposal ID: 1
    Vote:        yes
  Nonce:  1
  Fee:
    Amount: 0.000002 ROSE
    Gas limit: 2000
    (gas price: 0.000000001 ROSE per gas unit)
Other info:
  Genesis document's hash: 9ce956ef5999024e148f0c21f1e8a05ab4fc98a44c4696b289770705aeb1dd77
```

and ask you for confirmation.

## Submit the Transaction

To submit the generated transaction, we need to copy `tx_cast_vote.json` to the online Oasis node \(i.e. the `server`\) and submit it from there:

```bash
oasis-node consensus submit_tx \
  -a $ADDR \
  --transaction.file tx_cast_vote.json
```

