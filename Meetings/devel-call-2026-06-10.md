# Meeting Minutes: SVSM Development Call (June 10th, 2026)

## Topics:

### TSC Membership Update

* **Carlos Joined the TSC:** Jörg announced that Carlos López has joined the Technical Steering Committee, bringing the TSC to five members.

### TSC Meeting Update

* **CI Failure Fix Identified:** Jörg said the intermittent CI failure appears to be a KVM issue. Carlos said he prepared a small patch, and Stefano and Oliver tested it successfully in their environments.
* **Upstream and Stable Needed:** Jörg asked Carlos to make sure the fix gets upstream and into stable, so it can eventually reach the Azure Linux kernel used by CI.
* **Issue Tracking:** Stefano said he would update the issue to track the current progress and thanked Carlos for the patch.
* **AI-Generated PR Handling Reconfirmed:** Jörg said the TSC again discussed PRs that look AI-generated. The policy remains that AI-generated work is acceptable only when a human is responsible, signs off, and submits the work.
* **Human Authorship Checks:** Jörg said that when the project suspects a PR does not meet that requirement, the current approach is to ask for proof that a real person submitted it, usually by inviting the contributor to the community call or to a private call.
* **Planes and PR Status Discussed:** Jörg said the TSC also discussed the planes work and reviewed the status of several open PRs.

### KVM Planes Transition

* **Planes Patches Posted:** Jörg said the KVM planes patches are now based on Linux 7.1 and have been posted to the mailing list. There are still review and static-analysis issues to fix, but they should not block updating the SVSM downstream trees.
* **Downstream Tree Update Planned:** Jörg plans to update the downstream Linux and QEMU trees within the next two weeks so the default SVSM host stack uses a Linux 7.1 planes kernel and a newer QEMU.
* **Direct VMSA Still Blocks the Update:** Jörg said the biggest remaining blocker is direct VMSA support. He has written QEMU and kernel-side patches but still needs to integrate them into the planes series and verify that launch measurements match again.
* **More Frequent Host Stack Updates Expected:** Once the downstream trees are updated, Jörg expects to keep updating them more frequently as new versions of the planes work become available.

### SVSM Runtime IRQ Support and Breaking Changes

* **SVSM Gets Its Own APIC:** Jörg said the 7.1 planes patches give the SVSM its own APIC, allowing it to continue using APIC and IRQ support after launching firmware.
* **Create/Delete vCPU Protocol First Use Case:** Jörg said one likely first user is the SVSM core protocol handling for creating and deleting vCPUs, which currently has race conditions because it cannot use IPIs.
* **Older Host Stacks Will Stop Working:** Jörg said using these new runtime IRQ capabilities will introduce breaking changes, so future SVSM versions will stop working on older downstream host stacks such as the current 6.11 kernel branch.
* **Existing Users Asked for Feedback:** Adam said his deployment is already applying the new planes patches, so the expected breakage should be acceptable for his use case.
* **Backports May Work:** Adam asked whether applying the current planes patch set to older KVM series should work. Jörg said the posted version should work, and he does not currently see a reason why backports would not, although he could not guarantee them.
* **Compatibility Policy After Upstreaming:** Jörg said the project can still make breaking changes now, but once the host stack is upstream and the first stable SVSM release is made, SVSM should remain backward compatible.
* **Announcement Planned:** James suggested announcing the planned breakage and required host-stack updates. Jörg agreed and said the project will give users some transition time and announce the required component updates on the mailing list.
* **SVSM Has Never Run on Fully Upstream KVM:** Adam confirmed that SVSM has so far only worked on downstream KVM patch stacks, since the required multi-VMPL support has not yet been merged upstream.

### QEMU Device-Tree Support With Planes

* **Device-Tree Support May Be Folded In:** Luigi asked whether device-tree support could be merged into the new QEMU release with planes to avoid another breaking host-side update later.
* **Planes QEMU Branch Can Take the PR:** Jörg agreed that this would simplify things and asked Luigi to point him to the changes so he can apply them to his planes QEMU branch.
* **Conflict Check Planned:** Luigi said the PR is already open on the fork and should likely apply cleanly, although he still needs to test for conflicts.
* **Direct VMSA Patch Not Yet Pushed:** Jörg said his direct VMSA QEMU changes are still only local. They force vCPU index 0 for the VMSA and disallow non-zero vCPU indexes, but he and Luigi expected little overlap with the device-tree changes.

### Page-Table Crate Work

* **Three Page-Table PRs Open:** Ziqiao said he opened three PRs, identified during the call as 1062, 1088, and 1100, to move page-table code into a more general location outside the kernel folder.
* **AI Assisted but Human Reviewed:** Ziqiao said AI helped with some of the mechanical migration, but the commits were hand-reviewed and also contain manual changes.
* **Review Planned:** Jörg said the PRs had not been reviewed yet due to time constraints, but he plans to review them later this week.
* **Shared Crate Goal:** Ziqiao said the goal is for COCONUT-SVSM and another project, Lightbox, to use the same page-table implementation from a paging crate, with the possibility of publishing it on crates.io later.
* **Follow-Up Unsafe-Code Changes Expected:** Ziqiao said later versions may need small changes related to compiler behavior and Rust undefined behavior caused by improper unsafe code. Jörg suggested handling those changes on top after the current migration is reviewed.

### Cross-Compilation Status

* **One PR Still Missing:** Jörg asked whether the recently merged cross-compilation PRs make SVSM ready for cross-compilation. Stefano said one remaining Rust-related PR is still needed.
* **Documentation PR Pending:** Stefano said there is also a small documentation PR that he plans to merge soon.
