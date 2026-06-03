# Meeting Minutes: SVSM Development Call (June 3rd, 2026)

## Topics:

### TSC Meeting Update

* **KVM Planes Patch Set Ready for Testing:** Jörg said the KVM planes work is now ready on top of Linux 7.1, with KVM and QEMU branches pushed, and he plans to post the patches to the mailing list the following week.
* **Direct VMSA Setting Deferred:** Jörg said direct VMSA setting is still missing from the planes branch, although things work without it, and he plans to handle it in a separate patch set after updating the downstream kernel tree.
* **Testing Requested:** Jörg asked people to test the planes branches on their own setups and report problems before the mailing-list posting. His own testing with SMP VMs, multiple vCPUs, and SVSM looked good so far.
* **TSC Membership Expansion:** The TSC discussed adding more people to the TSC, with more details expected the following week.
* **Virtio Downstream Fork Strategy Reconfirmed:** The TSC recapped the virtio strategy and agreed to keep a downstream fork for now, then switch back to upstream once the required upstream changes are available.
* **Guest-Side Downstream Forks Avoided:** The TSC decided not to reintroduce downstream forks for guest-side components such as EDK2 and guest kernels. For future features such as alternate injection, the plan is to get guest-side changes upstream and build the kernel work on top of the KVM planes work.
* **CI Failure Investigation:** Jörg said the intermittent KVM/SVSM CI failure, tracked as issue 1081, appears mostly on Intel hosts and is currently reproducible for Stefano on Azure. Stefano's debugging suggests Hyper-V behavior may still be within specification while differing from real hardware, so the issue needs further discussion with KVM developers.
* **Log Buffer PR Strategy:** Jörg said the TSC wants to merge the log-buffer PR as soon as possible and handle any follow-up changes afterward so the related Google Summer of Code project is not blocked.

### KVM Planes Interrupt Routing

* **Per-Plane APICs Still Needed:** James asked whether device-plane interrupt routing removes the need for per-plane APICs. Jörg said it does not, because each plane still needs local APIC support for IRQ delivery, IPIs, and timers.
* **Device Plane Assignment:** Jörg explained that QEMU has a default device-plane setting plus per-device overrides for assigning devices to planes.
* **Runtime Device Movement Needs More Work:** James asked about moving devices between planes at runtime. Jörg said QEMU support would be needed, and pending interrupts could be lost unless the guest first quiesces or disables the device before moving it.

### Virtio MSI Routing With Planes

* **Virtio MSI Plane Information Is Lost:** Jörg said virtio devices in QEMU appear to signal MSIs through writes to the address space, which reaches KVM without device or plane information.
* **PCI MSI Path Works Better:** Jörg contrasted that with QEMU's PCI MSI path, where the PCI core can attach plane information to the MSI message before passing it to KVM.
* **Current Workaround Is Limited:** Jörg said the current workaround is to use a default plane for address-space MSI writes, but that will not work once virtio devices such as virtio-blk need assignment to other planes.
* **Possible Directions:** James suggested reconstructing device-to-plane information from MSI ownership, while Jörg mentioned either converting virtio to use the PCI-style MSI mechanism or making QEMU address spaces per-plane. Jörg asked anyone with ideas or interest in the problem to follow up.

### Wait Queue Enhancement

* **Single-Task Wait Queue Limitation:** Nicola asked whether anyone is working on improving the wait queue, which currently has room for only a single task and can panic if another task is already queued.
* **No Known Owner for Multi-Task Support:** Jörg said John Lang had worked on that code, but he was not aware of anyone extending it for multiple waiting tasks.
* **Issue to Be Opened:** Nicola said he would open an issue and involve John before proceeding.
