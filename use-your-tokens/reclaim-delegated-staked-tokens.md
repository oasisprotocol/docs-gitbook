# Reclaim Delegated/Staked Tokens

{% hint style="info" %}
This example assumes you have read and followed the instructions in the [Prerequisites](prerequisites.md) and [Setup](setup.md) sections.
{% endhint %}

When we want to reclaim escrowed tokens, we can't do that directly. Instead, we need to specify the number of shares we want to reclaim from an escrow account.

For example, to reclaim 357 billion shares from an escrow account, we need to generate the following reclaim escrow transaction:

```bash
oasis-node stake account gen_reclaim_escrow \
  "${TX_FLAGS[@]}" \
  --stake.shares 357000000000 \
  --stake.escrow.account oasis1qr6swa6gsp2ukfjcdmka8wrkrwz294t7ev39nrw6 \
  --transaction.file tx_reclaim.json \
  --transaction.nonce 9 \
  --transaction.fee.gas 1000 \
  --transaction.fee.amount 2000
```

To submit the generated transaction, we need to copy `tx_reclaim.json` to the online Oasis node \(i.e. the `server`\) and submit it from there:

```bash
oasis-node consensus submit_tx \
  -a $ADDR \
  --transaction.file tx_reclaim.json
```

Let's [check our account's info](accounts/get-info.md):

```javascript
{
    "general": {
        "balance": "223492486765",
        "nonce": 10
    },
    "escrow": {
        "active": {
            "balance": "11049031678686",
            "total_shares": "9828014125910"
        },
        "debonding": {
            "balance": "401353137954",
            "total_shares": "401353137954"
        },
        ...
    }
}
```

We can observe that:

* Our general balance decreased for 2000 base units. This corresponds to the fee

  that we specified we will pay for this transaction.

* Our account's nonce increased to `10`.
* Our escrow account's active number of shares decreased for 357 billion shares

  to 9828014125910.

* Our escrow account's active balance decreased for 401353137954 base units and

  is now 11049031678686 base units.

* Our escrow account's debonding balance increased to 401353137954 base units

  and its number of shares to the same amount.

When a delegator wants to reclaim a certain number of escrowed tokens, the _base unit price_ \(in shares\) must be calculated based on the escrow account's current active balance and the number of issued shares:

```text
base_units_per_share = account_delegated_base_units / account_issued_shares
```

In our case, the current base unit price \(i.e. `base_units_per_share`\) is 11450384816640 / 10185014125910 which is 1.124238481664054 base unit per share.

For 357 billion shares, the amount of base units that will be reclaimed is thus 357 \* 10^9 \* 1.124238481664054 which is 401353137954.06726 \(rounded to 401353137954\).

Hence, the escrow account's active balance decreased for 401353137954 base units and the debonding balance increased for the same amount.

{% hint style="warning" %}
While the number of debonding shares currently equals the number of base units that are currently subject to debonding and hence, the amount of tokens we can except to reclaim after debonding period is over is a little over 401 tokens, there is no guarantee that this stays the same until the end of the debonding period since any slashing could change shares' price.
{% endhint %}

The debonding period is specified by the `staking.params.debonding_interval` consensus parameter and is represented as a number of epochs that need to pass.

To obtain its value from the genesis file, run:

```bash
cat /localhostdir/genesis.json | \
  python3 -c 'import sys, json; \
  print(json.load(sys.stdin)["staking"]["params"]["debonding_interval"])'
```

For our example network, this returns:

```text
10
```

After the debonding period has passed, the network will automatically move our escrow account's active debonding balance into our escrow account's active balance.

