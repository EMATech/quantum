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

## Build

```bash
make
```

## Install

```bash
sudo make install-module
sudo modprobe snd-quantum
```

## Uninstall

```bash
sudo rmmod snd-quantum
sudo make uninstall-module
```

## Note

This project is independent and is not affiliated with PreSonus™ or Fender™.

[rfc]: https://lore.kernel.org/all/20260820083646.11383-1-nicholas.johnson-opensource@outlook.com.au/
[presonus-quantum-linux]: https://github.com/EMATech/presonus-quantum-linux/tree/generic
[presonus-quantum2626-linux]: https://github.com/jamie-steele/presonus-quantum-linux
