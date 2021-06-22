---
description: This page describes how to run a ParaTime node on the Oasis Network.
---

# Run a ParaTime Node

{% hint style="info" %}
These instructions are for setting up a _ParaTime_ node. If you want to run a _validator_ node instead, see the [instructions for running a validator node](run-validator.md). Similarly, if you want to run a non-validator node instead, see the [instructions for running a non-validator node](run-non-validator.md).
{% endhint %}

{% hint style="success" %}
If you are looking for some concrete ParaTimes that you can run, see [the list of ParaTimes and their parameters](../../contribute-to-the-network/run-a-paratime-node.md).
{% endhint %}

This guide will cover setting up your ParaTime compute node for the Oasis Network. This guide assumes some basic knowledge on the use of command line tools.

## Prerequisites

Before following this guide, make sure you've followed the [Prerequisites](../prerequisites/) and [Run a Non-validator Node](run-non-validator.md) sections and have the Oasis Node binary installed and configured on your systems. In addition to the basic non-validator configuration you will also need to [create and register your own entity](run-validator.md#creating-your-entity). Reading the rest of the [validator node setup instructions](run-validator.md) may also be useful.

### Stake Requirements

To be able to register as a ParaTime node on the Oasis Network, you need to have enough tokens staked in your escrow account. For more details, see the [Stake requirements](../../contribute-to-the-network/run-validator.md#stake-requirements) section of [Run a Validator Node](../../contribute-to-the-network/run-validator.md) doc. Note that stake requirements may differ from ParaTime to ParaTime.

### The ParaTime Identifier and Binary

In order to run a ParaTime node you need to obtain the following pieces of information first, both of these need to come from a trusted source:

* \*\*\*\*[**The ParaTime Identifier**](https://docs.oasis.dev/oasis-core/high-level-components/index-1/identifiers) is a 256-bit unique identifier of a ParaTime on the Oasis Network. It provides a unique identity to the ParaTime and together with the [genesis document's hash](https://docs.oasis.dev/oasis-core/high-level-components/index/genesis#genesis-documents-hash) serves as a domain separation context for ParaTime transaction and cryptographic commitments.  It is usually represented in hexadecimal form, for example: `8000000000000000000000000000000000000000000000000000000000000000` 
* **The ParaTime Binary** contains the executable code that implements the ParaTime itself. It is executed in a sandboxed environment by Oasis Node and its format depends on whether the ParaTime is running in a Trusted Execution Environment \(TEE\) or not.  In the non-TEE case this will be a regular Linux executable \(an [ELF binary](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format), usually without an extension\) and in the TEE case this will be an [SGXS binary](https://github.com/fortanix/rust-sgx/blob/master/doc/SGXS.md) \(usually with a `.sgxs` extension\) that describes a secure enclave together with a detached signature of the binary \(usually with a `.sig`extension\).  The rest of this guide assumes that the binary is available at `/node/bin/paratime.sgxs` and that the signature is available at `/node/bin/paratime.sig`.

{% hint style="danger" %}
Like the genesis document, make sure you obtain these from a trusted source.
{% endhint %}

{% hint style="warning" %}
#### **Compiling the ParaTime Binary from Source Code**

In case you decide to build the ParaTime binary from source yourself, make sure that you follow our guidelines for deterministic compilation to ensure that you receive the exact same binary.

When the ParaTime is running in a TEE, a different binary to what is registered in the consensus layer will not work and will be rejected by the network.
{% endhint %}

### Trusted Execution Environment \(TEE\)

If the ParaTime is configured to run in a TEE \(currently only [Intel SGX](https://www.intel.com/content/www/us/en/architecture-and-technology/software-guard-extensions.html)\), you must make sure that your system supports running SGX enclaves. This requires that your hardware has SGX support, that SGX support is enabled and that the additional driver and software components are properly installed and running.

#### Install SGX Driver

On [Intel's website](https://01.org/intel-software-guard-extensions/downloads), find the latest "Intel SGX Linux Release" \(_not_ "Intel SGX DCAP Linux Release"\) and download the "Intel \(R\) SGX Installers" for your platform. The package will have `driver` in the name.

After installing the driver and restarting your system, make sure that the `/dev/isgx` device exists.

{% hint style="warning" %}
Make sure that `/dev` is _not_ mounted with the `noexec` option as otherwise the enclave loader will be unable to map executable pages. You can use `sudo mount -o remount,exec /dev` to temporarily change the mount flag.
{% endhint %}

#### Install AESM Service

The easiest way to install and run the AESM service is by using a Docker container provided by Fortanix as follows \(this will keep the container running and it will be automatically started on boot\):

```bash
docker run \
  --pull always \
  --detach \
  --restart always \
  --device /dev/isgx \
  --volume /var/run/aesmd:/var/run/aesmd \
  --name aesmd \
  fortanix/aesmd
```

#### Check SGX Setup

In order to make sure that your SGX setup is working, you can use the `sgx-detect` tool from the [sgxs-tools](https://lib.rs/crates/sgxs-tools) Rust package.

Make sure you have Rust nightly, pkg-config and OpenSSL development package installed. For more details, see [Oasis Core's Build Environment Setup and Building, Prerequisites doc](https://docs.oasis.dev/oasis-core/development-setup/build-environment-setup-and-building/prerequisites).

Then build and install sgxs-tools by running:

```bash
cargo +nightly install sgxs-tools
```

_NOTE: These utilities must be compiled with a nightly version of the Rust toolchain since they use the `#![feature]` macro._

After the installation completes run `sgx-detect` to make sure that everything is set up correctly. When everything works, you should get output similar to the following \(some things depend on hardware features so your output may differ\):

```text
Detecting SGX, this may take a minute...
✔  SGX instruction set
  ✔  CPU support
  ✔  CPU configuration
  ✔  Enclave attributes
  ✔  Enclave Page Cache
  SGX features
    ✔  SGX2  ✔  EXINFO  ✔  ENCLV  ✔  OVERSUB  ✔  KSS  
    Total EPC size: 92.8MiB
✘  Flexible launch control
  ✔  CPU support
  ？ CPU configuration
  ✘  Able to launch production mode enclave
✔  SGX system software
  ✔  SGX kernel device (/dev/isgx)
  ✘  libsgx_enclave_common
  ✔  AESM service
  ✔  Able to launch enclaves
    ✔  Debug mode
    ✘  Production mode
    ✔  Production mode (Intel whitelisted)
```

The important part is the checkbox under _Able to launch enclaves_ in both _Debug mode_ and _Production mode \(Intel whitelisted\)_.

In case you encounter errors, see the [list of common SGX installation issues](https://edp.fortanix.com/docs/installation/help/) for help.

## Configuration

In order to configure the node create the `/node/etc/config.yml` file with the following content:

```yaml
datadir: /node/data

log:
  level:
    default: info
    tendermint: info
    tendermint/context: error
  format: JSON

genesis:
  file: /node/etc/genesis.json

consensus:
  tendermint:
    core:
      listen_address: tcp://0.0.0.0:26656

      # The external IP that is used when registering this node to the network.
      # NOTE: If you are using the Sentry node setup, this option should be
      # omitted.
      external_address: tcp://{{ external_address }}:26656
    
    p2p:
      # List of seed nodes to connect to.
      # NOTE: You can add additional seed nodes to this list if you want.
      seed:
        - "{{ seed_node_address }}"

runtime:
  supported:
    # List of ParaTimes that the node should support.
    - "{{ runtime_id }}"

  paths:
    # Paths to ParaTime binaries for all of the supported ParaTimes.
    "{{ runtime_id }}": /node/bin/paratime.sgxs
  
  # The following section is required for ParaTimes which are running inside the
  # Intel SGX Trusted Execution Environment.
  sgx:
    loader: /node/bin/oasis-core-runtime-loader
    signatures:
      # Paths to ParaTime signatures.
      "{{ runtime_id }}": /node/bin/paratime.sig

worker:
  registration:
    # In order for the node to register itself, the entity.json of the entity
    # used to provision the node must be available on the node.
    entity: /node/entity/entity.json
  
  storage:
    enabled: true
  
  compute:
    enabled: true
  
  client:
    # External gRPC configuration.
    port: 30001
    addresses:
      # The external IP that is used when registering this node to the network.
      - "{{ external_address }}:30001"
  
  p2p:
    # External P2P configuration.
    enabled: true
    port: 30002
    addresses:
      # The external IP that is used when registering this node to the network.
      - "{{ external_address }}:30002"

# The following section is required for ParaTimes which are running inside the
# Intel SGX Trusted Execution Environment.
ias:
  proxy:
    address:
      # List of IAS proxies to connect to.
      # NOTE: You can add additional IAS proxies to this list if you want.
      - "{{ ias_proxy_address }}"
```

Before using this configuration you should collect the following information to replace the  variables present in the configuration file:

* `{{ external_address }}`: The external IP you used when registering this node.
* `{{ seed_node_address }}`: The seed node address in the form `ID@IP:port`.

  You can find the current Oasis Seed Node address in the [Network Parameters](../../oasis-network/network-parameters.md).

* `{{ runtime_id }}`: The Runtime identifier.
* `{{ ias_proxy_address }}`: The IAS proxy address in the form `ID@HOST:port`. You can find the current Oasis IAS proxy address in the [Network Parameters](../../oasis-network/network-parameters.md). If you want you can also [run your own IAS proxy](run-an-ias-proxy.md).

## Starting the Oasis Node

You can start the node by running the following command:

```bash
oasis-node --config /node/etc/config.yml
```

## Checking Node Status

To ensure that your node is properly connected with the network, you can run the following command after the node has started:

```bash
oasis-node control status -a unix:/node/data/internal.sock
```

