# Contribution Guidelines

{% hint style="info" %}
This document outlines our guidelines for contributing to the Oasis Network's codebase and documentation. If you are interested in learning more about the Oasis Network's governance model, including the processes for submitting feature requests and bug fixes, [please see our governance overview here](https://docs.oasis.dev/operators/governance.html).
{% endhint %}

### Building

Our development environment is documented in our [README](https://github.com/oasisprotocol/oasis-core/blob/master/README.md).

### Contributing Code

* **File issues:** Please make sure to first file an issue \(i.e. feature request, bug report\) before you actually start work on something.
* **Create branches:** If you have write permissions to the repository, you can create user-id prefixed branches \(e.g. user/feature/foobar\) in the main repository. Otherwise, fork the main repository and create your branches there.
  * Good habit: regularly rebase to the `HEAD` of `master` branch of the main repository to make sure you prevent nasty conflicts:

    ```text
    git rebase <main-repo>/master
    ```

  * Push your branch to GitHub regularly so others can see what you are working on:

    ```text
    git push -u <main-repo-or-your-fork> <branch-name>
    ```

    _Note that you are allowed to force push into your development branches._
* **Use draft pull requests for work-in-progress:**
  * The draft state signals that the code is not ready for review, but still gives a nice URL to track the ongoing work.
* _master_ branch is protected and will require at least 1 code review approval from a code owner before it can be merged.
* When coding, please follow these standard practices:
  * **Write tests:** Especially when fixing bugs, make a test so we know that we’ve fixed the bug and prevent it from reappearing in the future.
  * **Logging:** Please follow the logging conventions in the rest of the code base.
  * **Instrumentation:** Please following the instrumentation conventions in the rest of the code.
    * Try to instrument anything that would be relevant to an operational network.
* **Change Log:** Please write a [Change Log fragment](https://github.com/oasisprotocol/old-docs/blob/0f46da8900d0d1f0ad7388c773845c1ee27a1cc8/src/operators/.changelog/README.md) that will be included in the next section of the [Change Log](https://github.com/oasisprotocol/old-docs/blob/0f46da8900d0d1f0ad7388c773845c1ee27a1cc8/src/operators/CHANGELOG.md) once a new version is released.
* **Documentation:** Please write documentation in the code as you go. If possible also consider updating/augmenting the [developer documentation](https://github.com/oasisprotocol/old-docs/blob/0f46da8900d0d1f0ad7388c773845c1ee27a1cc8/src/operators/docs/index.md).
* **Check CI:** Don’t break the build!
  * Make sure all tests pass before submitting your pull request for review.
* **Signal PR review:**
  * Mark the draft pull request as _Ready for review_.
  * Please include good high-level descriptions of what the pull request does.
  * The description should include references to all GitHub issues addressed by the pull request. Include the status \("done", "partially done", etc\).
  * Provide some details on how the code was tested.
  * After you are nearing review \(and definitely before merge\) **squash commits into logical parts** \(avoid squashing over merged commits, use rebase first!\). Use proper commit messages which explain what was changed in the given commit and why.
* **Get a code review:**
  * Code owners will be automatically assigned to review based on the files that were changed.
  * You can generally look up the last few people to edit the file to get the best person to review.
  * When addressing the review: Make sure to address all comments, and respond to them so that the reviewer knows what has happened \(e.g. "done" or "acknowledged" or "I don't think so because ..."\).
* **Merge:** Once approved, the creator of the pull request should merge the branch, close the pull request, and delete the branch. If the creator does not have write access to the repository, one of the committers should do so instead.
* **Signal to close issues:** Let the person who filed the issue close it. Ping them in a comment \(e.g. @user\) making sure you’ve commented how an issue was addressed.
  * Anyone else with write permissions should be able to close the issue if not addressed within a week.

### Contributing Documentation

Documentation is always welcome! Documentation comes in several forms:

* Code-level documentation, following language specifications.
* Developer and system documentation, which lives in `docs/`, describes commonly used components, protocols and testing procedures.

### Style Guides

#### Git Commit Messages

A quick summary:

* Separate subject from body with a blank line.
* Limit the subject line to 72 characters.
* Capitalize the subject line.
* Do not end the subject line with a period.
* Use the present tense \("Add feature" not "Added feature"\).
* Use the imperative mood \("Move component to..." not "Moves component to..."\).
* Wrap the body at 80 characters.
* Use the body to explain _what_ and _why_ vs. _how_.

A detailed post on Git commit messages: [How To Write a Git Commit Message](https://chris.beams.io/posts/git-commit/).

#### Go Style Guide

Go code should use the standard `gofmt` formatting style. Be sure to run `gofmt` before pushing any code.

`gofmt` also does not check import order/grouping, so please ensure that you use the following convention:

```text
package foo

import (
  // Standard library imports.
  "context"
  "errors"

  // External imports.
  "github.com/opentracing/opentracing-go"

  // Internal imports.
  "github.com/oasisprotocol/oasis-core/go/common/crypto/hash"
)
```

#### Rust Style Guide

Rust code should use the style provided in the `.rustfmt.toml` in the top-level directory of the repository. Be sure to run `cargo fmt`before pushing any code.

Similar as above for Go, `rustfmt` does not check import order/grouping, so please ensure that you use the following convention:

```text
// External crates.
extern crate foo;

// Local modules.
mod bar;

// Standard library imports.
use std::{mem, box::Box};

// External imports.
use foo::baz;

// Internal imports.
use bar::quux;
```

Additionally, as `rustfmt` does not check `Cargo.toml`, please manually verify that changes to `Cargo.toml` follow the [best practices](https://github.com/rust-lang-nursery/fmt-rfcs/blob/master/guide/cargo.md) \(the most important ones are sorting dependencies and splitting long lines\).

