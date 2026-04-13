# Meeting Minutes: SVSM Development Call (April 8th, 2026)

## Topics:

### **Introduction and Updates**

* **TSC Meeting**: There was no Technical Steering Committee (TSC) meeting this week as Joerg is on vacation, so there were no official updates from that group to share.
* **General Updates**: Stefano Garzarella and Peter Fang had no additional updates to provide from their sides.

### **Main Discussion: IGVM Measurements and CoRIM Strategy**

The primary agenda item was a discussion regarding a unified strategy for handling guest measurements with the Independent Guest Virtual Machine (IGVM) format across the community using the Confidential Reference Integrity Manifest (CoRIM).

* **Vision for CoRIM Integration**: 
    * The goal is to expand IGVM processing to include measurement information directly within an IGVM file using CoRIM, which uses the CBOR binary format.
    * Hardware vendors (Intel, AMD) are expected to produce standard schemas for platform-specific measurement fields (e.g., MRTD for Intel, launch digest for SNP).
    * The IGVM file builder would be able to merge custom information with these platform-specific measurements to produce a compound CoRIM document.
    * This document can be signed by a trusted authority to authenticate the expected measurements at attestation time.
* **Operational Benefits**:
    * Embedding CoRIM artifacts at build time would allow QEMU to extract them during loading.
    * The guest could then obtain these artifacts to send to a relying party during attestation, eliminating the need for separate out-of-band measurement processing.
* **Discussion Points**:
    * **IGVM Measure Tool**: If this vision is fully realized, the standalone `igvm-measure` tool may no longer be strictly necessary because measurements would be built-in.
    * **Guest-to-Host Protocol**: Stefano Garzarella raised the question of how the guest will obtain CoRIM from the host. Jon Lange noted that the protocol (e.g., GHCB, GHCI, or a standard VSOCK interface) still needs to be standardized by the industry.
    * **Backward Compatibility**: Gerd Hoffmann suggested that the functionality to calculate measurements should remain available independently from the CoRIM header to support transitions.
    * **Google's Workflow**: Adam Dunlap shared that Google currently generates and signs launch measurements separately. He confirmed that the proposed CoRIM plan is unlikely to break their existing workflow and would likely be a beneficial addition later.

### **Decisions and Next Steps**

* **Crate Centralization**: The community agreed to push measurement and CoRIM generation logic into the common IGVM crate.
* **Tool Refactoring**: The existing `igvm-measure` tool will eventually be refactored into a "thin wrapper" that calls the common IGVM crate logic.
* **Interim Development**:
    * Development on the measurement logic will begin before waiting for finalized vendor formats.
    * The team will define a simple, interim CBOR format for measurements to enable immediate progress.
    * Chris Oo will investigate a suitable CBOR crate for the ecosystem or consider "vendoring" one for now.
* **Vendor Engagement**: Jon Lange is continuing conversations with Intel and AMD to obtain provisional format information to enable tool building.
