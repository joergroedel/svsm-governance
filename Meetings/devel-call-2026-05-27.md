# Meeting Minutes: SVSM Development Call (May 27th, 2026)

## Topics:

### TSC Meeting Update

* **No TSC Update:** Stefano hosted the call because Jörg was unavailable and said there was no TSC update, since the TSC did not meet the previous day.

### Google Summer of Code Introduction

* **First COCONUT-SVSM GSoC Participation:** Stefano said this is the first year COCONUT-SVSM is participating in Google Summer of Code, under the QEMU community umbrella.
* **Coding Period Started:** Stefano noted that the coding period had just started and will run until the end of August.
* **New Contributors Welcomed:** Stefano welcomed Nicola Ramacciotti and Tanish Desai to the community and invited them to introduce their projects.
* **Weekly Call Available for Blockers:** Stefano encouraged both GSoC contributors to join future development calls whenever they want to discuss blockers, ask questions, or coordinate work, and also invited them to open issues, fix issues, and send PRs.

### Observability Protocol Project

* **Project Overview:** Nicola Ramacciotti said he will work with Stefano, Jörg, and Gerd on observability support and a new SVSM protocol.
* **Guest-Facing Services:** Nicola said the project is intended to provide services such as log-buffer observability and memory information to guests.
* **Driver and User-Space Tooling:** Nicola said the work will involve a guest device driver and a user-space utility.
* **Existing Proposal:** Nicola noted that a generic overview of the protocol had already been sent to the mailing list a couple of weeks earlier.

### PCID Support Project

* **Project Overview:** Tanish Desai said he will work on x86 Process-Context Identifier support in COCONUT-SVSM.
* **Mentorship:** Tanish said Luigi and Jörg will mentor the PCID project.
* **CPUID Feature Cleanup:** Tanish said the first phase is to remove inconsistent open-coded CPUID feature checks and replace them with standardized constants similar to `x86-cpuid-db`, organized by leaves and subleaves.
* **PCID Assignment Strategy:** Tanish said the second phase is to develop a PCID assignment strategy where all page tables belonging to a task share the same PCID, while page tables outside that task get different PCIDs.
* **Context-Switch TLB Handling:** Tanish said the TLB code should then be updated so context switches replace only entries associated with the relevant task's PCID while preserving other TLB entries.
* **PCID-Aware Invalidation:** Tanish said the final phase is to move from broad TLB invalidation toward PCID-aware flushing, including looking at AMD's newer invalidate page-table command for AMD systems.
