# Meeting Minutes: SVSM Development Call (April 15th, 2026)

## Topics:

### TSC Meeting Update (April 14)

* **Google Summer of Code (GSoC):**
    * Mentors have selected students for the projects.
    * Final announcements from QEMU or Google are expected by April 30.
* **COCONUT CCC Yearly Update:**
    * An update to the Confidential Computing Consortium is scheduled for April 30.
    * Jörg is preparing a slide deck based on last year's presentation, which will be reviewed by the TSC and shown in this meeting beforehand.
* **PR Reviews & Merges:**
    * Task scheduler race condition fixes have been merged.
    * **PR1030 (Carlos López):** Review was delayed due to overlapping work with a GSoC project regarding CPUID refactoring. 
    * Jörg noted that Carlos's work provides a structured way to define leaves and registers, which the student can build upon.
    * Future work involves writing a code generator for a Rust crate from the x86 CPUID database.
    * **VSOCK PR:** Current device tree support depends on the QEMU 11 release.
    * **Migration PR (745):** Status remains unclear.
* **Scheduler & Testing:**
    * Jon found and fixed an issue in the scheduler.
    * Jon is working on an API to wait for a task, which will unblock Nicola’s user space testing PR.

### General Topics & Verification

* **Verus & Coconut Verification:** * Ziqiaozhou updated the group on adding the COCONUT project into Verus testing to detect if feature changes break verification.
    * A PhD intern from Microsoft Research may join to work on the verification project.
    * A PR to support iterators in the Verus project is nearly complete, which will resolve previous workarounds regarding Rust features.

### Observability Protocol Discussion

* **Data Typing:** Jörg discussed a conversation with Gerd regarding adding data types (beyond byte streams, such as 64-bit values or slices) to the observability protocol.
* **Design Decision:** Keeping a simple file-system-like interface (similar to QEMU FWCFG) versus moving toward a typed key-value store.
* **Workflow:** Nicolai Stange questioned the user-space workflow; Jörg clarified that user space will see source names rather than index numbers, and the kernel driver will handle the mapping.
* **Constraints:** The project must remain within the 12-15 week GSoC timeline, though long-term goals include notification mechanisms and support for writable sources.

