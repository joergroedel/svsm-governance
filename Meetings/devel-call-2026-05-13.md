# Meeting Minutes: SVSM Development Call (May 13th, 2026)

## Topics:

### TSC Meeting Update

* **Security Report Follow-Up:** Jörg said the TSC spent most of its recent meeting on security-process work and non-public bug reports, so only limited details could be shared publicly.
* **TPM Issue Review Volunteers:** Because some reports affect the TPM path, Jörg asked Nicolai Stange and James Bottomley to help assess those reports once sharing is cleared, and both agreed.
* **CPUID/XCR0 Handling Issue:** Jörg highlighted a newly opened GitHub issue around unsound CPUID handling for `XCR0`, saying the broader XSAVE and CPUID handling likely needs more substantial cleanup than first expected.
* **Possible GSoC Alignment:** Jörg noted that some of this CPUID work may fit naturally into the related Google Summer of Code project.

### XSAVE and CPUID Discussion

* **Current Problem:** The group discussed that matching CPUID entries directly on `XCR0` does not scale, because supporting all possible `XCR0` combinations in the CPUID page would lead to a combinatorial explosion.
* **Linux-Inspired Fix:** Carlos López said he opened a pull request earlier that day which instead computes the required XSAVE area size by iterating over the enabled feature bits and querying the relevant `0xD` subleaves, similar to how Linux handles it.
* **IGVM Builder Impact:** The proposed fix still requires IGVM Builder to request the necessary CPUID subleaves so the SVSM can compute sizes for the enabled features.
* **Need for SVSM Cross-Checks:** Jörg argued that SVSM must also validate CPUID feature and XSAVE-state consistency itself, because the PSP cannot reliably cross-check mismatches across multiple security pages.
* **Feature-Bit Sanitization:** The group agreed SVSM should clear advertised CPUID feature bits when the corresponding XSAVE size information is missing, to avoid exposing inconsistent capabilities such as unsupported shadow-stack state handling.
* **Next Step:** Jörg said he plans to review Carlos's PR later in the week and continue the technical discussion there.

### Project Updates

* **QEMU Rebase:** Luigi Leonardi announced that the project QEMU fork has been rebased to `11.0.0` and asked others to start testing it.
* **Known VGA Issue Still Present:** Luigi said the existing VGA-related boot problem remains unresolved after the rebase.
* **Device Tree Work Next:** Luigi said the rebase work leaves the branch ready for device-tree support, which he expects to post soon.
