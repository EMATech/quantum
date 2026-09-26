# quantum

Linux kernel ALSA driver for PreSonus™ Quantum Thunderbolt™ audio interfaces.

## Current Support

Currently, only the **PreSonus Quantum 2626** is supported.
Work is in progress to extend support to the rest of the Quantum product family.

## Origin & Attribution

This repository is an out-of-tree build adaptation
of the original [RFC patch series](rfc)
by **Nicholas Johnson** (August 20, 2026).

> Hi all,
>
> This RFC series adds an ALSA PCI driver for PreSonus Quantum PCIe/Thunderbolt
> audio interfaces.
>
> This project is independent and is not affiliated with PreSonus or Fender.
>
> The driver provides playback, capture, raw MIDI, sample-rate control,
> clock-source control, XRUN reporting, and surprise-removal handling. It
> targets the Quantum PCI family and currently has verified support for the
> Quantum 2626.
>
> The implementation keeps the hardware path direct and multichannel, with
> the aim of being small, reviewable, and suitable for eventual upstreaming.
>
> I want to thank my best friend, Ju Ern, for lending me the machine used to
> develop the driver, and the Asahi Linux project for m1n1 and the tooling
> that made the hardware tracing and analysis possible.
>
> Only the verified 2626 device ID is matched by the driver at present.
>
> I am sending this as RFC to gather review and testing before asking for
> merge. I would especially appreciate feedback on ALSA integration, device
> enumeration, runtime PM/latency handling, and upstream expectations for the
> driver.
>
> Thanks,
> Nicholas Johnson

## Modifications

This fork adapts the original RFC to support **out-of-tree compilation**.

- Added a custom `Makefile` for external building based upon
  [my previous work][presonus-quantum-linux]
  on [Jamie Steele's attempt][presonus-quantum2626-linux].
- Driver source code remains unchanged from
  the original RFC (Quantum 2626 only).
- **Next Steps:** Refactoring the device detection and initialization logic
  to support the full Quantum range, building on the foundations laid
  in [Jamie Steele's attempt][presonus-quantum2626-linux]
  and [my own research][presonus-quantum-linux].


## Development Workflow

This repository serves as a local working tree
for out-of-tree building and testing.
When contributing changes back to the Linux kernel community,
**do not send commits directly**.
Instead, generate standard patch files that exclude local build infrastructure.

### Generating Patches for Upstream Submission

To create patch files suitable for the `linux-sound` mailing list,
excluding the root `Makefile`, `README.md`, and `LICENSE`:

```bash
# Replace <base-commit> with the hash of the last clean import (e.g., the RFC import commit)
git format-patch <base-commit>..HEAD -- ':!Makefile' ':!README.md' ':!LICENSE'
```

### Sending Patches

Once generated, review the patches and send them via `git send-email`:

```bash
git send-email --to=linux-sound@vger.kernel.org \
  --cc=tiwai@suse.com --cc=perex@perex.cz \
  *.patch
```

### Code Style & Checks

Before generating patches, ensure your code adheres to kernel standards:

```bash
# Check for coding style issues
make C=1

# Run static analysis (needs sparse installed)
make C=2
```

> **Note:** Always mark incomplete features
> or known issues with `// FIXME` or `// TODO` comments in the code.
> These will be visible in the generated patches
> and signal to reviewers which parts are work-in-progress.

## Usage

### Build

```bash
make
```

### Install

```bash
sudo make install-module
sudo modprobe snd-quantum
```

### Uninstall

```bash
sudo rmmod snd-quantum
sudo make uninstall-module
```

## Note

This project is independent and is not affiliated with PreSonus™ or Fender™.

[rfc]: https://lore.kernel.org/all/20260820083646.11383-1-nicholas.johnson-opensource@outlook.com.au/
[presonus-quantum-linux]: https://github.com/EMATech/presonus-quantum-linux/tree/generic
[presonus-quantum2626-linux]: https://github.com/jamie-steele/presonus-quantum-linux
