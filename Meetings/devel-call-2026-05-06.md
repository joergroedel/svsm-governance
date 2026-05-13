# Meeting Minutes: SVSM Development Call (May 6th, 2026)

## Topics:

### TSC Meeting Update

* **Memory Ordering PR:** Jörg reported that the memory-ordering discussion around `PR1051` was resolved, with the relevant swap operation needing acquire-release semantics rather than release-only semantics.
* **CCC Project Update:** The Confidential Computing Consortium reacted positively to COCONUT's growth in contributors, merged PRs, and improved security posture, including CI and OpenSSF scorecard progress.
* **Broader Workload Direction:** The CCC discussion focused on COCONUT's expanded scope beyond an SVSM toward running smaller confidential workloads and basic Linux binaries directly.
* **Possible Gramine Collaboration:** The overlap with Gramine's goal of running unmodified Linux applications prompted discussion about possible collaboration, though the Rust-versus-C language split was noted as a practical barrier.
* **Security Reports:** The TSC reviewed newly received security reports and is working on mitigations, with more details expected later.
* **GSoC Acceptance:** Both COCONUT-related Google Summer of Code projects were accepted, students have started engaging with the community, and project work is expected to begin on May 25.
* **AI-Generated PR Policy:** The group discussed how to handle AI-generated or AI-assisted pull requests after `PR1049`, reaffirming that AI-assisted code is allowed but pull requests must still be submitted by a human to satisfy DCO requirements.

### PRs and Issue Triage

* **Bootloader Changes Merged:** Jon's bootloader series was considered ready and has already been merged, with any remaining review feedback to be handled as follow-up fixes.
* **PR Dependency Review:** The TSC also reviewed several other open pull requests to clarify status and dependencies between them.
* **VGA Boot Failure:** The group revisited the issue where attaching a VGA device in QEMU causes SVSM boot failures, likely in the accept-pages path, and agreed that more investigation is still needed.
* **Secrets Zeroing Issue:** Another issue discussed was the report about secrets not being cleared correctly; Stefano said he would continue the follow-up in the issue tracker.
* **TSC Context Shared:** Jörg explained for newer participants that the TSC is a four-person shared maintainership group that meets weekly to handle governance, merges, issue tracking, and security follow-up.

### vTPM Attestation Manifest Discussion

* **Manifest v1 Questions:** Jacob Xu raised questions about implementing the updated vTPM service manifest, which now expects a list of signing and storage keys rather than the single-key behavior in the earlier version.
* **Interactive vs. Fixed Lists:** The main design discussion compared an interactive protocol where the requester provides a template against a fixed or vendor-specific list of attested templates or keys.
* **Protocol Size Constraint:** James Bottomley noted that the current attestation request format likely does not have enough input space to carry arbitrary template data, which may explain why the interactive approach was rejected previously.
* **Names vs. Public Areas:** The group also discussed attesting TPM names instead of full public key areas to reduce size, with the caveat that some approaches may require regenerating keys.
* **Next Step:** Jörg suggested moving the open design questions to the mailing list first and opening a GitHub issue later once the direction is clearer and implementation work can be tracked concretely.

### Verification and Refactoring

* **Page Table Refactor:** Ziqiaozhou asked about splitting generic x86 page-table code from COCONUT-specific code to support formal verification and possible reuse by other projects; Jörg said that direction was fine as long as it does not conflict with future multi-architecture cleanup.
* **Verification CI Cost:** Ziqiaozhou also reported that the verification CI currently installs both Z3 and Verus, with Z3 taking the most time.
* **Caching Follow-Up:** Jörg said the current CI cost is acceptable for now, but caching Verus artifacts similar to the QEMU workflow would be a useful optimization later.
* **Local Setup Convenience:** Stefano asked whether local developers could still use prebuilt tooling, and Ziqiaozhou said that keeping a path for local prebuilt installs would be helpful.
