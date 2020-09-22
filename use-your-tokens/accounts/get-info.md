# Get Info



{% hint style="info" %}
This example assumes you have read and followed the [Prerequisites](../prerequisites.md) and [Setup](../setup.md) sections.
{% endhint %}

To get more information about a particular staking account, e.g. `oasis1qrvsa8ukfw3p6kw2vcs0fk9t59mceqq7fyttwqgx`, run:

```bash
oasis-node stake account info \
  -a $ADDR \
  --stake.account.address oasis1qrvsa8ukfw3p6kw2vcs0fk9t59mceqq7fyttwqgx \
  | python3 -m json.tool
```

This will output all staking information about this particular account, e.g.:

```javascript
{
    "general": {
        "balance": "376594833237"
    },
    "escrow": {
        "active": {
            "balance": "10528683450039",
            "total_shares": "10000000000000"
        },
        "debonding": {
            "balance": "0",
            "total_shares": "0"
        },
        "commission_schedule": {},
        "stake_accumulator": {
            "claims": {
                "registry.RegisterEntity": [
                    {
                        "global": "entity"
                    }
                ],
                "registry.RegisterNode.9Epy5pYPGa91IJlJ8Ivb5iby+2ii8APXdfQoMZDEIDc=": [
                    {
                        "global": "node-validator"
                    }
                ]
            }
        }
    }
}
```

We can observe that:

* Account's general balance \(`general.balance`\), the amount of base units that are available to the account owner, is ~377 tokens.
* Account's nonce \(`general.nonce`\), the incremental number that must be unique for each account's transaction, is omitted. That means there haven't been any transactions made with this account as the source. Therefore, the next transaction should have nonce equal to `0`.

Each account can also serve as an escrow account. Escrow accounts are used to keep the funds needed for specific consensus-layer operations \(e.g. registering and running nodes\).

To simplify accounting, each escrow results in the source account being issued shares which can be converted back into staking tokens during the reclaim escrow operation. Reclaiming escrow does not complete immediately, but may be subject to a debonding period during which the tokens still remain escrowed.

We can observe that:

* The amount of tokens that are actively bounded to the escrow account \(`escrow.active.balance`\) is ~10529 tokens.
* The total number of shares for the tokens actively bounded to the escrow account \(`escrow.active.total_shares`\) is 10 trillion.
* The amount of tokens that are currently debonding \(`escrow.debonding.balance`\) is 0.
* The total number of shares for the tokens that are currently debonding \(`escrow.debonding.total_shares`\) is 0.

An entity can also charge commission for tokens that are delegated to it. The commission schedule rate steps would be defined in `escrow.commission_schedule.rates` and the commission rate bound steps would be defined in `escrow.commission_schedule.bounds`. For more details, see the [Amending a commission schedule]() example.

Each escrow account also has a corresponding stake accumulator. It stores stake claims for an escrow account and ensures all claims are satisfied at any given point. Adding a new claim is only possible if all of the existing claims plus the new claim can be satisfied.

We can observe that the stake accumulator currently has two claims:

* The `registry.RegisterEntity` claim is for registering an entity. It needs to satisfy the global threshold \(`global`\) for registering an entity \(`entity`\) which is defined by the staking consensus parameters.

  To see the value of the `entity` global staking threshold, run the `oasis-node stake info` command as described in [Common token Info]() section.

* The `registry.RegisterNode.9Epy5pYPGa91IJlJ8Ivb5iby+2ii8APXdfQoMZDEIDc=` claim is for registering the node with ID `9Epy5pYPGa91IJlJ8Ivb5iby+2ii8APXdfQoMZDEIDc=`.  


  It needs to satisfy the global threshold \(`global`\) for registering a validator node \(`node-validator`\) which is defined by the staking consensus parameters.  


  To see the value of the `node-validator` global staking threshold, run the `oasis-node stake info` command as described in [Common Token Info]() section.  


  In addition to the global thresholds, each runtime the node is registering for may define their own thresholds. In case the node is registering for multiple runtimes, it needs to satisfy the sum of thresholds of all the runtimes it is registering for.  


  For more details, see [Oasis Core Developer Docs on registering a node](https://github.com/oasisprotocol/oasis-core/blob/master/docs/consensus/registry.md#register-node).

