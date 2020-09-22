# Transfer Tokens

{% hint style="info" %}
This example assumes you have read and followed the [Prerequisites](prerequisites.md) and [Setup](setup.md) sections.
{% endhint %}

Let's assume we have a staking account with address `oasis1qr6swa6gsp2ukfjcdmka8wrkrwz294t7ev39nrw6` and we want to transfer tokens to another staking account, e.g. `oasis1qr3jc2yfhszpyy0daha2l9xjlkrxnzas0uaje4t3`.

{% hint style="info" %}
To convert your entity's ID to a staking account address, see the [Obtain Account Address From Entity's ID](accounts/address.md#obtain-account-address-from-entitys-id) section.
{% endhint %}

## Query Our Account's Info

To query our staking account's information, use the following command:

```bash
oasis-node stake account info \
  -a $ADDR \
  --stake.account.address oasis1qr6swa6gsp2ukfjcdmka8wrkrwz294t7ev39nrw6 \
  | python3 -m json.tool
```

{% hint style="info" %}
For a detailed explanation on querying account information, see [Account Info]() section.
{% endhint %}

At the beginning, this outputs:

```javascript
{
    "general": {
        "balance": "601492492765",
        "nonce": 7
    },
    "escrow": {
        "active": {
            "balance": "11242384816640",
            "total_shares": "10000000000000"
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

* Account's general balance is ~601 tokens.
* Account's nonce is `7`.
* ~11242 tokens are actively bounded to the escrow account.
* The amount of tokens that are currently debonding is 0.

## Query Destination Account's Info

To query the destination account's information, use the following command:

Initially, this outputs:

```javascript
{
    "general": {
        "balance": "0",
        "nonce": 1030
    },
    "escrow": {
        "active": {
            "balance": "0",
            "total_shares": "0"
        },
        "debonding": {
            "balance": "0",
            "total_shares": "0"
        },
        ...
    }
}
```

We can observe that the chosen destination account currently has a general balance of 0 tokens.

## Generate a Transfer Transaction

Let's generate a transfer transaction of 170 tokens, \(i.e. 170 \* 10^9 base units\), from our account to the chosen destination account:

```bash
oasis-node stake account gen_transfer \
  $TX_FLAGS \
  --stake.amount 170000000000 \
  --stake.transfer.destination oasis1qr3jc2yfhszpyy0daha2l9xjlkrxnzas0uaje4t3 \
  --transaction.file tx_transfer.json \
  --transaction.nonce 7 \
  --transaction.fee.gas 1000 \
  --transaction.fee.amount 2000
```

## Submit the Transaction

To submit the generated transaction, we need to copy `tx_transfer.json` to the online Oasis node \(i.e. the `server`\) and submit it from there:

```bash
oasis-node consensus submit_tx \
  -a $ADDR \
  --transaction.file tx_transfer.json
```

## Query Both Accounts' Info

Let's [check both accounts' info]() \(first ours and then the destination's\):

```javascript
{
    "general": {
        "balance": "431492490765",
        "nonce": 8
    },
    "escrow": {
        "active": {
            "balance": "11242384816640",
            "total_shares": "10000000000000"
        },
        "debonding": {
            "balance": "0",
            "total_shares": "0"
        },
        ...
    }
}
```

```javascript
{
    "general": {
        "balance": "170000000000",
        "nonce": 1030
    },
    "escrow": {
        "active": {
            "balance": "0",
            "total_shares": "0"
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

* Our general balance decreased for 170 tokens and 2000 base units. The latter

  corresponds to the fee that we specified we will pay for this transaction.

* Our account's nonce increased to `8`.
* Destination account's general balance increased for 170 tokens.

