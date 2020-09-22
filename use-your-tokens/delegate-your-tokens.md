# Delegate/Stake Tokens

In this example we will put 208 tokens \(i.e. 208 \* 10^9 base units\) to our own escrow account.

First, let's generate an escrow transaction and store it to `tx_escrow.json`:

```bash
oasis-node stake account gen_escrow \
  $TX_FLAGS \
  --stake.amount 208000000000 \
  --stake.escrow.account oasis1qr6swa6gsp2ukfjcdmka8wrkrwz294t7ev39nrw6 \
  --transaction.file tx_escrow.json \
  --transaction.nonce 8 \
  --transaction.fee.gas 1000 \
  --transaction.fee.amount 2000
```

To submit the generated transaction, we need to copy `tx_escrow.json` to the online Oasis node \(i.e. the `server`\) and submit it from there:

```bash
oasis-node consensus submit_tx \
  -a $ADDR \
  --transaction.file tx_escrow.json
```

Let's [check our account's info](accounts/get-info.md):

```javascript
{
    "general": {
        "balance": "223492488765",
        "nonce": 9
    },
    "escrow": {
        "active": {
            "balance": "11450384816640",
            "total_shares": "10185014125910"
        },
        "debonding": {
            "balance": "0",
            "total_shares": "0"
        },
        ...
    }
}
```

We can observe that:

* Our general balance decreased for 208 tokens and 2000 base units. The latter

  corresponds to the fee that we specified we will pay for this transaction.

* Our account's nonce increased to `9`.
* Our escrow account's active balance increased for 208 tokens.
* The total number of shares in our escrow account's active balance

  increased from 10000000000000 to 10185014125910.

When a delegator delegates some amount of tokens to a staking account, the delegator receives the number of shares proportional to the current _share price_ \(in base units\) calculated from the total number of base units delegated to a staking account so far and the number of shares issued so far:

```text
shares_per_base_unit = account_issued_shares / account_delegated_base_units
```

In our case, the current share price \(i.e. `shares_per_base_unit`\) is 10000000000000 / 11242384816640 which is 0.8894909899542729.

For 208 tokens, the amount of newly issued shares is thus 208 \* 10^9 \* 0.8894909899542729 which is 185014125910.48877 \(rounded to 185014125910\).

