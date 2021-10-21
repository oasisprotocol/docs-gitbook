---
description: This page describes how to run a ParaTime node on the Oasis Network.
---

# Run a ParaTime Node

{% hint style="info" %}
These instructions are for setting up a _ParaTime_ node. If you want to run a _validator_ node instead, see the [instructions for running a validator node](run-validator.md). Similarly, if you want to run a non-validator node instead, see the [instructions for running a non-validator node](run-non-validator.md).
{% endhint %}

{% hint style="warning" %}
For a production setup, we recommend running the ParaTime compute/storage node separately from the validator node (if you run one).

Running ParaTime and validator nodes as separate Oasis nodes will prevent configuration mistakes and/or (security) issues affecting one node type from affecting the other one.
{% endhint %}

{% hint style="success" %}
If you are looking for some concrete ParaTimes that you can run, see [the list of ParaTimes and their parameters](../../contribute-to-the-network/run-a-paratime-node.md).
{% endhint %}

{% hint style="success" %}
Oasis Core refers to ParaTimes as runtimes internally, so all configuration options will have runtime in their name.
{% endhint %}

This guide will cover setting up your ParaTime compute node for the Oasis Network. This guide assumes some basic knowledge on the use of command line tools.

## Prerequisites

Before following this guide, make sure you've followed the [Prerequisites](../prerequisites/) and [Run a Non-validator Node](run-non-validator.md) sections and have:

* Oasis Node binary installed and configured on your system.
* The chosen top-level `/node/` working directory prepared. In addition to `etc` and `data` directories, also prepare the following directories:
  * `bin`: This will store binaries needed by Oasis Node for running the ParaTimes.
  * `runtimes`: This will store the ParaTime binaries and their corresponding signatures (if they are running in a Trusted Execution Environment).

{% hint style="success" %}
Feel free to name your working directory as you wish, e.g. `/srv/oasis/`.

Just make sure to use the correct working directory path in the instructions below.
{% endhint %}

* Genesis file copied to `/node/etc/genesis.json`.

{% hint style="success" %}
Reading the rest of the [validator node setup instructions](run-validator.md) may also be useful.
{% endhint %}

{% hint style="info" %}
To speed up bootstraping your new node, we recommend [copying node's state from your existing node](../advanced/copy-state-from-one-node-to-the-other.md) or [syncing it using state sync](../advanced/sync-node-using-state-sync.md).
{% endhint %}

### Stake Requirements

To be able to register as a ParaTime node on the Oasis Network, you need to have enough tokens staked in your entity's escrow account.

To see the staking requirements for different node roles, use the Oasis CLI tools as described in [Common Staking Info](../../manage-tokens/oasis-cli-tools/common-staking-info.md).

{% hint style="success" %}
Currently, both the Mainnet and the Testnet require 100 ROSE/TEST for each role type:

```
Staking threshold (entity): ROSE 100.0
Staking threshold (node-validator): ROSE 100.0
Staking threshold (node-compute): ROSE 100.0
Staking threshold (node-storage): ROSE 100.0
Staking threshold (node-keymanager): ROSE 100.0
Staking threshold (runtime-compute): ROSE 100.0
Staking threshold (runtime-keymanager): ROSE 100.0
```
{% endhint %}

To register a node that is both a validator and a ParaTime node, the entity for which the node is registered would need to satisfy the following:

* Entity registration staking threshold (currently 100 tokens),
* Validator node staking threshold (currently 100 tokens),
* Compute node staking threshold (currently 100 tokens),
* Storage node staking threshold (currently 100 tokens).

All together, there would need to be at least 400 tokens staked in your entity's escrow account.

To stake the tokens, use our [Oasis CLI tools](../../manage-tokens/oasis-cli-tools/delegate-tokens.md).

If everything was set up correctly, you should see something like below when running [Oasis Node Stake Account Info](../../manage-tokens/oasis-cli-tools/get-account-info.md) command for your entity's account (this is an example for the Testnet):

```bash
Balance:
  Total: 0.0 TEST
  Available: 0.0 TEST

Active Delegations to this Account:
  Total: 20000.0 TEST (20000000000000 shares)
  Delegations:
    - From:   oasis1qrwdwxutpyr9d2m84zh55rzcf99aw0hkts7myvv9
      Amount: 20000.0 TEST (20000000000000 shares)

Stake Accumulator:
  Claims:
    - Name: registry.RegisterEntity
      Staking Thresholds:
        - Global: entity
    - Name: registry.RegisterNode.HG5TB3dbY8gtYBBw/R/cHfPaOpe0vT7W1wu/2rtpk/A=
      Staking Thresholds:
        - Global: node-compute
      Staking Thresholds:
        - Global: node-storage

Nonce: 1
```

{% hint style="info" %}
The stake requirements may differ from ParaTime to ParaTime and are subject to change in the future.
{% endhint %}

### Register a New Entity or Update Your Entity Registration

If you don't have an entity yet, create a new one by following the [Creating Your Entity](run-validator.md#creating-your-entity) instructions.

{% hint style="danger" %}
Everything in this section should be done on the `localhost` as there are sensitive items that will be created.
{% endhint %}

If you will be running the ParaTime on a new Oasis Node, initialize a new node by following the [Initializing a Node](run-validator.md#initializing-a-node) instructions.

Then update your entity descriptor by enumerating paths to all your node's descriptors (existing and new ones) in the `--entity.node.descriptor` flag. For example:

```bash
oasis-node registry entity update \
    ... various signer flags ... \
    --entity.node.descriptor /localhost/node1/node_genesis.json,/localhost/node2/node_genesis.json
```

{% hint style="info" %}
To confirm all nodes are added to your entity descriptor, run:

```bash
cat <PATH-TO-entity.json> | jq
```

and ensure you see all your nodes' IDs under the `"nodes"` list.

For example:

```bash
{
  "v": 2,
  "id": "QTg+ZjubD/36egwByAIGC6lMVBKgqo7xnZPgHVoIKzc=",
  "nodes": [
    "yT1h8/eN0VInQxn3YKcHxvSgGcsjsTSYmdTLZZMBTWI=",
    "HG5TB3dbY8gtYBBw/R/cHfPaOpe0vT7W1wu/2rtpk/A="
  ]
}
```
{% endhint %}

Then generate and submit the new/updated entity descriptor via the entity registration transaction by following the [Generating Entity Registration Transaction](run-validator.md#generating-entity-registration-transaction) instructions.

{% hint style="warning" %}
Make sure your entity descriptor (i.e. `entity.json`) is copied to your online server and saved as `/node/entity/entity.json` to ensure the [node's configuration](run-a-paratime-node.md#configuration) will find it.
{% endhint %}

{% hint style="success" %}
You will [configure the node](run-a-paratime-node.md#configuration) to automatically register for the roles it has enabled (i.e. storage and compute roles) via the `worker.registration.entity` configuration flag.

No manual node registration is necessary.
{% endhint %}

### The ParaTime Identifier and Binary

In order to run a ParaTime node you need to obtain the following pieces of information first, both of these need to come from a trusted source:

* ****[**The ParaTime Identifier**](https://docs.oasis.dev/oasis-core/high-level-components/index-1/identifiers) is a 256-bit unique identifier of a ParaTime on the Oasis Network. It provides a unique identity to the ParaTime and together with the [genesis document's hash](https://docs.oasis.dev/oasis-core/high-level-components/index/genesis#genesis-documents-hash) serves as a domain separation context for ParaTime transaction and cryptographic commitments.\
  \
  It is usually represented in hexadecimal form, for example:\
  `8000000000000000000000000000000000000000000000000000000000000000`\

* **The ParaTime Binary** contains the executable code that implements the ParaTime itself. It is executed in a sandboxed environment by Oasis Node and its format depends on whether the ParaTime is running in a Trusted Execution Environment (TEE) or not.\
  \
  In the non-TEE case this will be a regular Linux executable (an [ELF binary](https://en.wikipedia.org/wiki/Executable\_and\_Linkable\_Format), usually without an extension) and in the TEE case this will be an [SGXS binary](https://github.com/fortanix/rust-sgx/blob/master/doc/SGXS.md) (usually with a `.sgxs` extension) that describes a secure enclave together with a detached signature of the binary (usually with a `.sig`extension).

{% hint style="danger" %}
Like the genesis document, make sure you obtain these from a trusted source.
{% endhint %}

{% hint style="warning" %}
#### **Compiling the ParaTime Binary from Source Code**

In case you decide to build the ParaTime binary from source yourself, make sure that you follow our [guidelines for deterministic compilation](https://docs.oasis.dev/oasis-sdk/guide/reproducing) to ensure that you receive the exact same binary.

When the ParaTime is running in a TEE, a different binary to what is registered in the consensus layer will not work and will be rejected by the network.
{% endhint %}

### Install Oasis Core Runtime Loader

For ParaTimes running inside [Intel SGX Trusted Execution Environment](run-a-paratime-node.md#setting-up-trusted-execution-environment-tee), you will need to install the Oasis Core Runtime Loader.

The Oasis Core Runtime Loader binary (`oasis-core-runtime-loader`) is part of Oasis Core binary releases, so make sure you download the appropriate version specified the [Network Parameters](../../oasis-network/network-parameters.md) page.

Install it to `bin` subdirectory of your node's working directory, e.g. `/node/bin/oasis-core-runtime-loader`.

### Install ParaTime Binary

For each ParaTime, you need to obtain its binary and install it to `runtimes` subdirectory of your node's working directory.

For ParaTimes running inside a Trusted Execution Environment, you also need to obtain and install the binary's detached signature to this directory.

{% hint style="info" %}
For example, for the [Cipher ParaTime](../../foundation/testnet/#cipher-paratime), you would have to obtain the `cipher-paratime.sgxs` binary and the `cipher-paratime.sig` detached signature and install them to `/node/runtimes/cipher-paratime.sgxs`and `/node/runtimes/cipher-paratime.sig`.
{% endhint %}

### Install Bubblewrap Sandbox (at least version 0.3.3)

ParaTime compute nodes execute ParaTime binaries inside a sandboxed environment provided by [Bubblewrap](https://github.com/containers/bubblewrap). In order to install it, please follow these instructions, depending on your distribution:

{% tabs %}
{% tab title="Ubuntu 18.10+" %}
```bash
sudo apt install bubblewrap
```
{% endtab %}

{% tab title="Fedora" %}
```bash
sudo dnf install bubblewrap
```
{% endtab %}

{% tab title="Other Distributions" %}
On other systems, you can download the latest [source release provided by the Bubblewrap project](https://github.com/containers/bubblewrap/releases) and build it yourself.

Make sure you have the necessary development tools installed on your system and the `libcap` development headers. On Ubuntu, you can install them with:

```bash
sudo apt install build-essential libcap-dev
```

After obtaining the Bubblewrap source tarball, e.g. [bubblewrap-0.4.1.tar.xz](https://github.com/containers/bubblewrap/releases/download/v0.4.1/bubblewrap-0.4.1.tar.xz), run:

```bash
tar -xf bubblewrap-0.4.1.tar.xz
cd bubblewrap-0.4.1
./configure --prefix=/usr
make
sudo make install
```

{% hint style="warning" %}
Note that Oasis Node expects Bubblewrap to be installed under `/usr/bin/bwrap` by default.
{% endhint %}
{% endtab %}
{% endtabs %}

Ensure you have a new enough version by running:

```
bwrap --version
```

{% hint style="warning" %}
Ubuntu 18.04 LTS (and earlier) provide overly-old `bubblewrap`. Follow _Other Distributions_ section on those systems.&#x20;
{% endhint %}

## Setting up Trusted Execution Environment (TEE)

{% hint style="info" %}
In case the ParaTime does not require the use of a TEE (e.g. Intel SGX) you may skip this section and continue with [Configuration](run-a-paratime-node.md#configuration) below.
{% endhint %}

If the ParaTime is configured to run in a TEE (currently only [Intel SGX](https://www.intel.com/content/www/us/en/architecture-and-technology/software-guard-extensions.html)), you must make sure that your system supports running SGX enclaves. This requires that your hardware has SGX support, that SGX support is enabled and that the additional driver and software components are properly installed and running.

### Install SGX Linux Driver

Oasis Core currently only supports the legacy (out-of-tree) [Intel SGX Linux driver](https://github.com/intel/linux-sgx-driver).

{% hint style="info" %}
Support for the new Intel SGX support in mainline Linux kernels since version 5.11 is being tracked in [oasis-core#3651](https://github.com/oasisprotocol/oasis-core/issues/3651).
{% endhint %}

#### Ubuntu 18.04/16.04

A convenient way to install the SGX Linux driver on Ubuntu 18.04/16.04 systems is to use the [Fortanix](https://edp.fortanix.com/docs/installation/guide/)'s APT repository and its [DKMS](https://en.wikipedia.org/wiki/Dynamic\_Kernel\_Module\_Support) package.

First add Fortanix's APT repository to your system:

```bash
echo "deb https://download.fortanix.com/linux/apt xenial main" | sudo tee /etc/apt/sources.list.d/fortanix.list >/dev/null
curl -sSL "https://download.fortanix.com/linux/apt/fortanix.gpg" | sudo -E apt-key add -
```

And then install the `intel-sgx-dkms` package:

```bash
sudo apt update
sudo apt install intel-sgx-dkms
```

{% hint style="warning" %}
Some [Azure Confidential Computing instances](https://docs.microsoft.com/en-us/azure/confidential-computing/quick-create-portal) have the [Intel SGX DCAP driver](https://github.com/intel/SGXDataCenterAttestationPrimitives/tree/master/driver/linux) pre-installed.

To determine that, run `dmesg | grep -i sgx` and observe if a line like the following is shown:

```
[    4.991649] sgx: intel_sgx: Intel SGX DCAP Driver v1.33
```

If that is the case, you need to blacklist the Intel SGX DCAP driver's module by running:

```
echo "blacklist intel_sgx" | sudo tee -a /etc/modprobe.d/blacklist-intel_sgx.conf >/dev/null
```
{% endhint %}

#### Fedora 34/33

A convenient way to install the SGX Linux driver on Fedora 34/33 systems is to use the Oasis-provided [Fedora Package for the Legacy Intel SGX Linux Driver](https://github.com/oasisprotocol/sgx-driver-kmod).

#### Other Distributions

Go to [Intel SGX Downloads](https://01.org/intel-software-guard-extensions/downloads) page and find the latest "Intel SGX Linux Release" (_not_ "Intel SGX DCAP Release") and download the "Intel (R) SGX Installers" for your distribution. The package will have `driver` in the name (e.g., `sgx_linux_x64_driver_2.11.0_2d2b795.bin`).

#### Verification

After installing the driver and restarting your system, make sure that the `/dev/isgx` device exists.

### Ensure `/dev` is NOT Mounted with the `noexec` Option

Newer Linux distributions usually mount `/dev` with the `noexec` mount option. If that is the case, it will prevent the enclave loader from mapping executable pages.

Ensure your `/dev` (i.e. `devtmpfs`) is not mounted with the `noexec` option. To check that, use:

```
cat /proc/mounts | grep devtmpfs
```

To temporarily remove the `noexec` mount option for `/dev`, run:

```
sudo mount -o remount,exec /dev
```

To permanently remove the `noexec` mount option for `/dev`, add the following to the system's `/etc/fstab` file:

```
devtmpfs        /dev        devtmpfs    defaults,exec 0 0
```

{% hint style="info" %}
This is the recommended way to modify mount options for virtual (i.e. API) file system as described in [systemd's API File Systems](https://www.freedesktop.org/wiki/Software/systemd/APIFileSystems/) documentation.
{% endhint %}

### Install AESM Service

To allow execution of SGX enclaves, several **Architectural Enclaves (AE)** are involved (i.e. Launch Enclave, Provisioning Enclave, Provisioning Certificate Enclave, Quoting Enclave, Platform Services Enclaves).

Communication between application-spawned SGX enclaves and Intel-provided Architectural Enclaves is through **Application Enclave Service Manager (AESM)**. AESM runs as a daemon and provides a socket through which applications can facilitate various SGX services such as launch approval, remote attestation quote signing, etc.

#### Ubuntu 20.04/18.04/16.04

A convenient way to install the AESM service on Ubuntu 20.04/18.04/16.04 systems is to use the Intel's [official Intel SGX APT repository](https://download.01.org/intel-sgx/sgx\_repo/).

First add Intel SGX APT repository to your system:

```bash
echo "deb https://download.01.org/intel-sgx/sgx_repo/ubuntu $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/intel-sgx.list >/dev/null
curl -sSL "https://download.01.org/intel-sgx/sgx_repo/ubuntu/intel-sgx-deb.key" | sudo -E apt-key add -
```

And then install the `sgx-aesm-service`,  `libsgx-aesm-launch-plugin` and `libsgx-aesm-epid-plugin` packages:

```bash
sudo apt update
sudo apt install sgx-aesm-service libsgx-aesm-launch-plugin libsgx-aesm-epid-plugin
```

The AESM service should be up and running. To confirm that, use:

```bash
sudo systemctl status aesmd.service
```

#### Docker-enabled System

An easy way to install and run the AESM service on a [Docker](https://docs.docker.com/engine/)-enabled system is to use [Fortanix's AESM container image](https://hub.docker.com/r/fortanix/aesmd/).

Executing the following command should (always) pull the latest version of Fortanix's AESM Docker container, map the `/dev/isgx` device and `/var/run/aesmd` directory and ensure AESM is running in the background (also automatically started on boot):

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

#### Podman-enabled System

Similarly to Docker-enabled systems, an easy way to install and run the AESM service on a [Podman](https://podman.io)-enabled system is to use [Fortanix's AESM container image](https://hub.docker.com/r/fortanix/aesmd/).

First, create the container with:

```bash
sudo podman create \
  --pull always \
  --device /dev/isgx \
  --volume /var/run/aesmd:/var/run/aesmd:Z \
  --name aesmd \
  docker.io/fortanix/aesmd
```

Then generate the `container-aesmd.service` systemd unit file for it with:

```bash
sudo podman generate systemd --restart-policy=always --time 10 --name aesmd | \
  sed "/\[Service\]/a RuntimeDirectory=aesmd" | \
  sudo tee /etc/systemd/system/container-aesmd.service
```

Finally, enable and start the `container-aesmd.service` with:

```bash
sudo systemctl enable container-aesmd.service
sudo systemctl start container-aesmd.service
```

The AESM service should be up and running. To confirm that, use:

```bash
sudo systemctl status container-aesmd.service
```

To see the logs of the AESM service, use:

```
sudo podman logs -t -f aesmd
```

### Check SGX Setup

In order to make sure that your SGX setup is working, you can use the `sgx-detect` tool from the [sgxs-tools](https://lib.rs/crates/sgxs-tools) Rust package.

There are no pre-built packages for it, so you will need to compile it yourself.

{% hint style="info" %}
sgxs-tools must be compiled with a nightly version of the Rust toolchain since they use the `#![feature]` macro.
{% endhint %}

#### Install Dependencies

Make sure you have the following installed on your system:

* [GCC](http://gcc.gnu.org).
* [Protobuf](https://github.com/protocolbuffers/protobuf) compiler.
* [pkg-config](https://www.freedesktop.org/wiki/Software/pkg-config).
* [OpenSSL](https://www.openssl.org) development package.

On Fedora, you can install all the above with:

```
sudo dnf install gcc protobuf-compiler pkg-config openssl-devel
```

On Ubuntu, you can install all the above with:

```
sudo apt install gcc protobuf-compiler pkg-config libssl-dev
```

#### Install [Rust](https://www.rust-lang.org) Nightly

We follow [Rust upstream's recommendation](https://www.rust-lang.org/tools/install) on using [rustup](https://rustup.rs) to install and manage Rust versions.

{% hint style="warning" %}
rustup cannot be installed alongside a distribution packaged Rust version. You will need to remove it (if it's present) before you can start using rustup.
{% endhint %}

Install rustup by running:

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

{% hint style="success" %}
If you want to avoid directly executing a shell script fetched the internet, you can also [download `rustup-init` executable for your platform](https://rust-lang.github.io/rustup/installation/other.html) and run it manually. This will run `rustup-init` which will download and install the latest stable version of Rust on your system.
{% endhint %}

Install Rust nightly with:

```
rustup install nightly
```

#### Build and Install sgxs-tools

```bash
cargo +nightly install sgxs-tools
```

#### Run `sgx-detect` Tool

After the installation completes, run `sgx-detect` to make sure that everything is set up correctly.

When everything works, you should get output similar to the following (some things depend on hardware features so your output may differ):

```
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

The important part is the checkbox under _Able to launch enclaves_ in both _Debug mode_ and _Production mode (Intel whitelisted)_.

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
    "{{ runtime_id }}": {{ runtime_bin_path }}
  
  # The following section is required for ParaTimes which are running inside the
  # Intel SGX Trusted Execution Environment.
  sgx:
    loader: /node/bin/oasis-core-runtime-loader
    signatures:
      # Paths to ParaTime signatures.
      "{{ runtime_id }}": {{ runtime_sig_path }}

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
  * You can find the current Oasis Seed Node address in the [Network Parameters](../../oasis-network/network-parameters.md).
* `{{ runtime_id }}`: The [ParaTime identifier](run-a-paratime-node.md#the-paratime-identifier-and-binary).
  * You can find the current Oasis-supported ParaTime identifiers in the [Network Paramers](../../foundation/testnet/#paratimes).
* `{{ runtime_bin_path }}`: Path to the [ParaTime binary](run-a-paratime-node.md#the-paratime-identifier-and-binary) of the form `/node/runtimes/foo-paratime.sgxs`.
* `{{ runtime_sig_path }}`: Path to the [ParaTime detached signature](run-a-paratime-node.md#the-paratime-identifier-and-binary) (for ParaTimes that require Intel SGX) of the form `/node/runtimes/foo-paratime.sig`.
* `{{ ias_proxy_address }}`: The IAS proxy address in the form `ID@HOST:port`.
  * You can find the current Oasis IAS proxy address in the [Network Parameters](../../oasis-network/network-parameters.md).
  * If you want, you can also [run your own IAS proxy](run-an-ias-proxy.md).

{% hint style="warning" %}
Make sure the`worker.client.port` (default: `9100`) and `worker.p2p.port` (default: `9200`) ports are exposed and publicly accessible on the internet (for`TCP`traffic).
{% endhint %}

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

## Troubleshooting

See  [the general troubleshooting section](../troubleshooting.md), before proceeding with ParaTime node-specific troubleshooting.

### Too Old Bubblewrap Version

Double check your installed `bubblewrap` version, and ensure is at least of version **0.3.3**. Previous versions of this guide did not explicitly mention the required version. For details see the [Install Bubblewrap Sandbox](run-a-paratime-node.md#install-bubblewrap-sandbox-at-least-version-0-3-3) section.

### Missing `libsgx-aesm-epid-plugin`

If you are encountering the following error message in your node's logs:

```
failed to initialize TEE: error while getting quote info from AESMD: aesm: error 30
```

Ensure you have all required SGX driver libraries installed as listed in [Install SGX Linux Driver section](run-a-paratime-node.md#install-sgx-linux-driver). Previous versions of this guide were missing the `libsgx-aesm-epid-plugin`.

### Stake Requirement

Double check your node entity satisfies the staking requirements for a ParaTime node. For details see the [Stake Requirements](run-a-paratime-node.md#stake-requirements) section.

### Runtime Worker Ports

Make sure the `worker.client.port` (default: `30001`) and `worker.p2p.port` (default: `30002`) ports are exposed and publicly accessible on the internet (for `TCP` traffic). Previous versions of this guide did not explicitly mention this requirement.

### Unable to Launch Enclaves

If running `sgx-detect --verbose` reports:

```
🕮  SGX system software > Able to launch enclaves > Debug mode
The enclave could not be launched.

debug: failed to load report enclave
debug: cause: failed to load report enclave
debug: cause: Failed to map enclave into memory.
debug: cause: Operation not permitted (os error 1)
```

Ensure your system's [`/dev` is NOT mounted with the `noexec` mount option](run-a-paratime-node.md#ensure-dev-is-not-mounted-with-the-noexec-option).
