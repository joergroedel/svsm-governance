# Meeting Minutes: SVSM Development Call (May 20th, 2026)

## Topics:

### TSC Meeting Update

* **Recent Fixes Merged:** Jörg said the TSC reviewed the status of several recently reported issues and noted that multiple related fix PRs have already been posted and merged.
* **Security and Embargo Policy Still Open:** The TSC continued discussing internal security and embargo handling, but no final policy decision was made yet.
* **CI Instability Investigation:** Jörg reported intermittent CI failures in the native-platform QEMU tests after the move to KVM-backed execution, saying additional debugging PRs were merged so future failures should provide more actionable data.
* **Private Security Advisory Workflow Trial:** The TSC also experimented with GitHub's private security advisory workflow and wants more experience with it before using it as the default path for handling security issues.

### Virtio Driver Sync Strategy

* **Current Sync Pain:** Stefano raised that fixing a recent virtio driver issue required landing the fix upstream first and then backporting it into the copied code in the COCONUT tree, which risks missing other upstream fixes.
* **Why the Code Was Inlined:** Jörg and Oliver recalled that the original upstream crate needed local integration patches, especially around transport and shared-buffer allocation hooks, and that the project also removed code it did not need.
* **Interest in Re-Syncing With Upstream:** Oliver said the gap between the local copy and upstream may now be smaller and worth re-evaluating because upstream changes could be beneficial.
* **Preferred Direction:** The group agreed that moving to a downstream fork, either as a submodule or Cargo dependency, would make rebasing easier while still carrying the project-specific patches temporarily.
* **Upstream-First Goal:** Stefano and Oliver agreed the needed patches should be proposed upstream where possible so the downstream fork can eventually be reduced or removed again.
* **Ownership:** Oliver volunteered to take the lead on the downstream fork evaluation, with Stefano coordinating setup and follow-up.

### Page Table Refactoring and Reuse

* **Missing Dirty Bit Likely a Bug:** Ziqiao asked why writable parent page-table entries set the accessed bit but not the dirty bit, and Jörg said that omission was likely just missed rather than intentional.
* **Refactor Toward Reuse:** Ziqiao said the ongoing page-table refactor aims to make the implementation more layered, easier to verify, and reusable by other projects.
* **Allocator Abstraction Added:** Ziqiao explained that the refactor introduces a trait-based way to provide page-table frame allocation and deallocation, which is one of the key requirements for making the code more generic.
* **Possible External Consumer:** Ziqiao mentioned that another Microsoft project may want to reuse the page-table implementation if it becomes general enough and remains well-suited for confidential-VM use cases.
* **Crates.io Publication Is Possible:** Jörg said he is open to publishing reusable crates from the project, including on `crates.io`, as long as the required generalization does not significantly hurt the COCONUT codebase or workflow.
* **Next Step:** Jörg asked for concrete requirements from the other interested project so the team can judge whether the current design should be shaped toward broader reuse.

### Device Tree Support Rollout

* **Testing Requires Matching QEMU Changes:** Luigi said testing the posted device-tree support requires his corresponding QEMU branch in addition to the SVSM PR.
* **Breaking Transition Needs Staging:** The device-tree work introduces a new IGVM directive and QEMU command-line parameters, so Luigi and Jörg agreed the QEMU changes need to merge first, followed by a notice for developers to update before the SVSM-side changes merge.
* **Launch Script and Version Handling:** The discussion noted that rollout may need some QEMU-version or capability detection in the launch flow, although the main incompatibility comes from the new IGVM builder directive.
* **IRQs Deferred:** Luigi said the current device-tree support intentionally skips IRQ description, which Jörg said is acceptable for now because SVSM does not yet use runtime IRQ support.
* **Immediate Follow-Up:** Jörg said he plans to test the device-tree work early the following week.
