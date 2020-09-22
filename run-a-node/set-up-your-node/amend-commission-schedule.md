# Amend Commission Schedule

We can configure our account to take a commission on staking rewards given to our node\(s\). The commission rate must be within bounds, which we can also configure.

Let's generate a transaction to:

* tell everyone that our bounds allow us to set any rate \(0% - 100%\), and
* we'll take 50%.

We're not allowed to change the commission bounds too close in near future, so we'd have to make changes a number of epochs in the future.

The commission schedule rules are specified by the `staking.params.commission_schedule_rules` consensus parameter.

To obtain its value from the genesis file, run:

```bash
cat /localhostdir/genesis.json | \
  python3 -c 'import sys, json; \
  rules = json.load(sys.stdin)["staking"]["params"]["commission_schedule_rules"]; \
  print(json.dumps(rules, indent=4))'
```

For our example network this returns:

```javascript
{
    "rate_change_interval": 1,
    "rate_bound_lead": 14,
    "max_rate_steps": 21,
    "max_bound_steps": 21
}
```

This means that we must submit a commission rate bound at least `14` epochs in advance \(`rate_bound_lead`\) and that we can change it on every epoch \(`rate_change_interval`\).

The `max_rate_steps` and `max_bound_steps` determine the maximum number of commission rate steps and rate bound steps, respectively.

In the example, we're setting the bounds to start on epoch 16. An account's default bounds are 0% maximum, so we have to wait until our new bounds go into effect to raise our rate to 50%. Because of that, we'll specify that our rate also starts on epoch 16.

```bash
oasis-node stake account gen_amend_commission_schedule \
  $TX_FLAGS \
  --stake.commission_schedule.bounds 16/0/100000 \
  --stake.commission_schedule.rates 16/50000 \
  --transaction.file tx_amend_commission_schedule.json \
  --transaction.nonce 11 \
  --transaction.fee.gas 1000 \
  --transaction.fee.amount 2000
```

Rates and minimum/maximum rates are in units of 1/100,000, so `0`, `50000`, and `100000` come out to 0%, 50%, and 100%, respectively.

To submit the generated transaction, we need to copy `tx_amend_commission_schedule.json` to the online Oasis node \(i.e. the `server`\) and submit it from there:

```bash
oasis-node consensus submit_tx \
  -a $ADDR \
  --transaction.file tx_amend_commission_schedule.json
```

After that, we can [check our account's info](), and we should see something like this:

```javascript
{
    "general": {
        ...
        "nonce": 11
    },
    "escrow": {
        ...
        "commission_schedule": {
            "rates": [
                "start": 16,
                "rate": "50000",
            ],
            "bounds": [
                "start": 16,
                "rate_min": "0",
                "rate_max": "100000"
            ]
        }
    }
}
```

Node operators collect commissions when their node earns a staking reward for delegators. A validator node earns a staking reward for participating in the consensus protocol each epoch. The commission rate is a fraction of the staking reward.

For example, if our node earns a reward of 0.007 tokens, 0.0035 tokens are added to the escrow pool \(increasing the value of our escrow pool shares uniformly\), and 0.0035 tokens are given to us \(issuing us new shares as if we manually deposited them\).

{% hint style="info" %}
It is also possible to set multiple commission rate steps and rate bound steps by passing the `--stake.commission_schedule.rates` and `--stake.commission_schedule.bounds` CLI flags multiple times.

For example, setting multiple commission rate steps and rate bound steps with:

```bash
oasis-node stake account gen_amend_commission_schedule \
  ...
  --stake.commission_schedule.bounds 32/10000/50000 \
  --stake.commission_schedule.bounds 64/10000/30000 \
  --stake.commission_schedule.rates 32/50000 \
  --stake.commission_schedule.rates 40/40000 \
  --stake.commission_schedule.rates 48/30000 \
  --stake.commission_schedule.rates 56/25000 \
  --stake.commission_schedule.rates 64/20000 \
  ...
```

would result in the following commission schedule when [checking an account's info]():

```javascript
{
    "general": {
        ...
    },
    "escrow": {
        ...
        "commission_schedule": {
            "rates": [
                {
                    "start": 32,
                    "rate": "50000"
                },
                {
                    "start": 40,
                    "rate": "40000"
                },
                {
                    "start": 48,
                    "rate": "30000"
                },
                {
                    "start": 56,
                    "rate": "25000"
                },
                {
                    "start": 64,
                    "rate": "20000"
                }
            ],
            "bounds": [
                {
                    "start": 32,
                    "rate_min": "10000",
                    "rate_max": "50000"
                },
                {
                    "start": 64,
                    "rate_min": "10000",
                    "rate_max": "30000"
                }
            ]
        },
        ...
    }
}
```
{% endhint %}

{% hint style="info" %}
To troubleshoot an amendment that's rejected, consult our [compendium of 23 common ways for a commission schedule amendment to fail](https://github.com/oasisprotocol/oasis-core/blob/0dee03d75b3e8cfb36293fbf8ecaaec6f45dd3a5/go/staking/api/commission_test.go#L61-L610).
{% endhint %}

