# Meeting Minutes: SVSM Development Call (April 29th, 2026)

## Topics:

### TSC Meeting Update

* **Security Reporting Process:** Jörg Rödel reported that the current security reporting flow is too dependent on a single inbox and is being revised.
* **GitHub Vulnerability Reporting:** Private vulnerability reporting is now enabled on the main repository, and previously received reports have been mirrored there for restricted follow-up.
* **Embargo Policy:** The TSC discussed embargo handling, but deferred decisions because the project is still too early to make firm commitments.
* **Monthly Release:** The latest monthly development release is already out, so there were no additional release issues to cover.
* **GDB Usage Question:** The TSC wants feedback from the broader community on whether anyone actively relies on the current GDB-related workflow, because upcoming changes may affect it.

### CCC Annual Project Update Preview

* **Slide Deck Review:** Jörg walked through the slides prepared for the April 30th Confidential Computing Consortium project update.
* **Scope Wording Update:** The presentation now describes COCONUT as providing secure services for confidential workloads, broadening the framing beyond confidential VMs alone.
* **Current Execution Modes:** The talk introduces the current modes and uses them to frame later roadmap discussions.
* **Development Highlights and Metrics:** The presentation includes updated development highlights, contributor and merge statistics, release checkpoints, and OpenSSF scorecard progress.
* **Roadmap Priorities:** The preview emphasized upstream adoption gaps, persistence support, and work needed to move COCONUT further toward a paravisor-style model.
* **Broader Workload Direction:** Jörg also outlined ongoing discussion about supporting smaller workloads directly on COCONUT as the project grows.
* **GSoC Status:** The community is still waiting on Google's acceptance notifications through the QEMU project.

### Project Updates and Discussion

* **Cocoon FS Timeline:** Nicolai Stange said the Cocoon FS work, including the metadata support previously requested by Tyler, should be ready next week or the week after.
* **Direct Workload Use Cases:** Nicolai asked what kinds of workloads were envisioned for direct execution on SVSM.
* **Initial Target:** Jörg said the first targets would likely be small workloads rather than database-style applications, and initially may avoid storage-heavy use cases.
* **Communication Path:** The discussion noted that VSOCK can provide a practical communication mechanism for such workloads even without conventional networking.
* **Possible Future Runtimes:** The group briefly mentioned small WebAssembly-style runtimes as one possible future direction.
