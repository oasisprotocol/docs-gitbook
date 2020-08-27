# Network Governance

{% hint style="info" %}
The proposed governance model is still being finalized. Please check back for updates. If you have a general question on how to use and deploy our software, please read our Operator Docs or join our community Slack. All community members are welcom e and encouraged to commit code to the platform. Contribution guidelines can be found [here](contribution-guidelines.md).
{% endhint %}

### Proposed Governance Model Overview

The Oasis Protocol Foundation proposes a representative democracy governance model based on a combination of off-chain and on-chain processes for the continued development of the Oasis Network. The Oasis Protocol Foundation will be tasked with guiding the long-term development of the platform and coordinating the community of development and network operations, with input collected from community members and changes to the network being voted on by node operators, with voting power based proportionally on staked and delegated tokens. We propose this model because we think it will provide a balanced voice to all engaged community members -- from developers of all sizes, to node owners, to token holders -- while at the same time still facilitating the swift deployment of network updates, new features, critical bug fixes.

In order for the community to balance distributed ownership and participation with speed and quality of platform development, we propose a hybrid model of off- and on-chain mechanisms, organized around the following key components:

1. Minor Feature Requests
2. Major Feature Requests
3. Bug Fixes

### Minor Feature Requests

To request new functionality, there are two primary approaches that will be most effective at receiving input and making progress.

If the feature is small - a change to a single piece of functionality, or an addition that can be expressed clearly and succinctly in a few sentences, then the most appropriate place to [propose it is as a new feature request](https://github.com/oasisprotocol/oasis-core/issues/new?template=feature_request.md) in this repository.

### Major Feature Requests

If the feature is more complicated, involves protocol changes, or has potential safety or performance implications, then consider [proposing an ADR](https://github.com/oasisprotocol/oasis-core/blob/master/docs/adr/index.md) and submit it as a pull request ot this repository. This will allow a structured review and commenting of the proposed changes. You should aim to get the ADR accepted and merged before starting on implementation. Please keep in mind that the project's committers still have the final word on what is accepted into the project.

{% hint style="info" %}
We recommend that major protocol updates including a need to hard fork, and roadmap and feature planning be conducted with recommendations from the Oasis Foundation and its technical advisory committee.
{% endhint %}

Moving forward, our proposed process for reviewing and approving major protocol updates is:

* **Proposals** for features and roadmap updates can come from anyone in the community. These proposals are agreed upon by committee and then shared with the community and open for additional comments for a minimum of 1 month.
* **Review / Comments period** where anyone can provide input, comments, and suggestions. In the case of major roadmap updates, a comment period would be 2 weeks; for major bugs this would be 24 hours.
* **Technical steering committee approval.** If not independently taken by any community member, the technical steering committee can adopt the proposal and, if feasible, assign an engineering team \(see [RFC process](https://github.com/oasislabs/private-rfcs)\).
* **Final vote for approval.** Once built, the community votes to approve each upgrade and the corresponding features that are included in the proposal. This voting process may initially be done off-chain but will eventually become an on-chain process. Nodes in the validator set for the network’s consensus model will vote to approve changes, with each node’s voting power being proportional to their share of tokens staked relative to the total tokens staked in the validator set.
* **Upgrade.** Node operators autonomously upgrade their system to run the new version of the software.

### Urgent Bug Fixes

Urgent bug fixes will primarily be handled off-chain to optimize for speed in addressing any issues that are critical to the immediate health of the network. The Oasis Network community as a whole is collectively responsible for identifying and addressing bugs. As bugs are identified, the Oasis Protocol Foundation can serve as a line of first defense to triage these bugs and coordinate security patches for quick release.

Bugs are a reality for any software project. We can't fix what we don't know about!

If you believe a bug report presents a security risk, please follow [responsible disclosure](https://en.wikipedia.org/wiki/Responsible_disclosure) and report it directly to [security@oasislabs.com](mailto:security@oasislabs.com)instead of filing a public issue or posting it to a public forum.

We will get back to you promptly.

Otherwise, please, first search between [existing issues in our repository](https://github.com/oasisprotocol/oasis-core/issues) and if the issue is not reported yet, [file a new one](https://github.com/oasisprotocol/oasis-core/issues/new?template=bug_report.md).

### Contributing to the Network

If you are interested in contributing to the Oasis Network's codebase or documentation, please [review our contribution guidelines here.](contribution-guidelines.md)

