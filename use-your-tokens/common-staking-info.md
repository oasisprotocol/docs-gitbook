# Common Staking Info

{% hint style="info" %}
This example assumes you have read and followed the [Prerequisites](prerequisites.md) and [Setup](setup.md) sections.
{% endhint %}

To query an Oasis node for the common staking information, run:

```bash
oasis-node stake info -a $ADDR
```

This will output something like:

```text
Total supply: 10000000000000000000
Common pool: 7999217230119682890
Last block fees: 0
Staking threshold (entity): 100000000000
Staking threshold (node-validator): 100000000000
Staking threshold (node-compute): 100000000000
Staking threshold (node-storage): 100000000000
Staking threshold (node-keymanager): 100000000000
Staking threshold (runtime-compute): 100000000000
Staking threshold (runtime-keymanager): 100000000000
```

The numbers are listed in base units, 1 token corresponds to 10^9 \(i.e. one billion\) base units.

We can observe that the total supply is 10 billion tokens and that almost 8 billion tokens are in the _common pool_.

Finally, the staking thresholds for the entity, all node kinds \(validator, compute, storage\) and all runtime kinds \(compute, key manager\) are 100 tokens.

This means that if you want to register, e.g. an entity with a validator node, you need to escrow \(i.e. stake\) at least 200 tokens.

