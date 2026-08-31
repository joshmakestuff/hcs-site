---
layout: page
title: AspireHcs
permalink: /aspirehcs.html
---

# AspireHcs

An experimental [Aspire](https://aspire.dev) hosting integration for the Windows Host
Compute System API. It adds two resource types to the Aspire local dev loop, both
ephemeral — created on `aspire run`, torn down on exit, with state and logs in the
dashboard:

- **Hyper-V virtual machines** — boot a VHDX as an HCS compute system, with
  serial-console logs, endpoints on the guest's leased address, TCP health checks, and
  Connect (SSH/RDP) buttons in the dashboard.
- **Hyper-V-isolated Windows containers** — run images from an
  [hcsctl](hcsctl.html) store.

Both kinds consume as well as serve: `WithReference(other)` delivers connection strings
and endpoints into the guest.

All HCS access goes through hcsctl's `--json` contract; this package contains no HCS
interop of its own.

## Getting set up

**Requirements:** Windows with Hyper-V, the .NET SDK, and Aspire. You supply the guest
VM image (a bootable Gen2/UEFI VHDX); AspireHcs does not install operating systems.
Container images come from a registry through `hcsctl image pull` and an elevated
`hcsctl image import`.

- **Consumer documentation** — the builder API, requirements, and setup:
  [src/AspireHcs/README.md](https://github.com/joshmakestuff/AspireHcs/blob/main/src/AspireHcs/README.md)
- **Sample** — a Linux VM, a Windows VM and a Windows container in one AppHost, with
  image preparation steps:
  [samples/HcsSample.AppHost](https://github.com/joshmakestuff/AspireHcs/blob/main/samples/HcsSample.AppHost/README.md)

## More details

- [Issues](https://github.com/joshmakestuff/AspireHcs/issues) — work in progress
