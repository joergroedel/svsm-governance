# Meeting Minutes: SVSM Development Call (June 17th, 2026)

## Topics:

### TSC Meeting Update

* **Downstream Branch Updates Planned:** Jörg said the updated downstream Linux and QEMU branches were posted for testing on Monday and are expected to become the official branches around June 18th. No issues had been reported so far.
* **Device-Tree Work Expected in QEMU Branch:** Jörg still needs to test Luigi's QEMU device-tree work, but expects to include it when the new downstream branches are published.
* **Local Environment Updates Needed:** Jörg said he will send an email once the branch switch happens. Developers should update their local Linux and QEMU environments after that.
* **Future SVSM Breaking Changes:** Jörg repeated that SVSM will eventually start using capabilities from the new host stack and will stop working with the current Linux 6.11-based downstream kernel and QEMU. This is expected in a couple of weeks, likely not before the end of July.
* **SVSM Address Move Requires Recent EDK2:** Jörg mentioned PR 1125, which moves SVSM from its current high address to the 128MB boundary inside guest memory. The change is meant to allow SVSM to allocate more guest memory later if it needs more than the initial 16MB.
* **OVMF Memory-Map Support Required:** Jörg said the address move needs a recent EDK2 with Gerd's patch for handling the IGVM memory map correctly. Without that patch, OVMF will not boot correctly after the change is merged.
* **AI Tooling PR Discussed:** Jörg pointed to Carlos' PR 1120 adding an `AGENTS.md` file. He asked people using AI tooling for SVSM work to review the proposed instructions and provide feedback so they work well for the project.
* **CI KVM Fix Progress:** Jörg said the TSC discussed the GitHub CI issue for which Carlos posted a KVM fix. Stefano said he tested a newer version, it works, and it has also been sent upstream.
* **Virtio Driver Upstreaming Progress:** Stefano said Oliver is close to enabling use of upstream virtio drivers without a downstream fork. Oliver is also finishing related safe MMIO crate changes and is expected to update the open PR soon.

### EDK2 Support Status

* **Memory-Map Patch Is in Latest Stable EDK2:** Luigi checked that Gerd's EDK2 commit is already included in the latest stable EDK2 tag, so using the latest stable release should work for the SVSM address move.
* **OVMF Variable Protocol Support Not Yet in Stable:** Luigi said OVMF variable protocol support appears to have been merged in EDK2 main, but is not yet in the stable tag. He expects it in the August stable release.

### vTPM Persistent State

* **CocoonFS Metadata Feature in Progress:** Nicolai said Tyler requested additional features for the CocoonFS persistence implementation. Nicolai is working on free-form metadata fields that could store a wrap key for the filesystem, allowing a remote server to unwrap and return it after successful attestation.
* **CocoonFS PR Rebase Planned:** Nicolai hopes to complete the metadata feature by the end of the week and then rebase the existing PR that integrates the work into SVSM.
* **KBS Communication Fixes Posted:** Arun said he has three PRs. The first fixes communication between SVSM and Trustee KBS through a proxy. The other two are draft proof-of-concept PRs for exploring how vTPM persistent state should work.
* **Resource ID Prototype:** Arun's prototype adds a resource ID to the attestation request. The attestation proxy receives the resource ID when it is started, SVSM includes it in the attestation request, and KBS can use it to fetch the corresponding secret after validating evidence from the secure enclave.
* **Encrypted vTPM State Prototype:** Arun attached vTPM state to SVSM as a virtio block device. The image includes a simple header with metadata such as cipher information, and the prototype decrypts the state after receiving the secret from KBS.
* **Storage Backend Used for POC:** Arun said the proof of concept currently uses the KBS storage backend. He noted that SVSM also has a TODO for wrapped-key support, and that CocoonFS metadata support for wrap keys will help replace the current prototype decryption code later.
* **Read-Only Prototype Limitation:** Nicolai and Stefano discussed how TPM state writes should work. Stefano said the older proof of concept wrapped libc read and write paths so the TPM could persist state. Arun's current virtio-block approach appears to be read-only for now, and write support still needs to be checked.
* **Writable TPM State Still Needed:** Nicolai said the TPM state should be writable. Stefano said the prior write-support work should be easy to rebase if needed.

### vTPM Writeback Behavior

* **Avoid Excessive Backend Writes:** James said Linux received a [TPM subsystem bug](https://lore.kernel.org/all/bbc41534-a2d9-42dc-ac8a-ff8a0b4fd41f@siemens.com/) related to a TPM implementation based on the same source code, where the TPM tried to write state every few seconds. He suggested applying the same fix in SVSM's vTPM implementation so it does not overwhelm or wear out the backend.
* **Incremental or Periodic Writes Preferred:** James explained that the software TPM is currently oriented toward a normal filesystem backend and can be too eager about writing. For SVSM, once a writable backend exists, it should avoid frequent backend writes and behave more like a real TPM that queues writes and flushes periodically.
* **Follow-Up Requested:** Jörg asked James to send the details and link to the mailing list. Stefano suggested also opening an issue so the work can be tracked.

### SVSM Time Support

* **TPM Time Source Unclear:** Nicolai asked whether SVSM currently wires a time source into the TPM. Jörg said SVSM has no internal notion of time yet, although it is on the list of things to add.
* **Planes May Help:** Jörg said the switch to planes should make timer support easier because the Linux 7.1 host stack provides timers and secure TSC support.
