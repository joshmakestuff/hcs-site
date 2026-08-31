---
layout: home
title: Home
---

# hcsctl & AspireHcs

Two projects for running Hyper-V virtual machines and Windows containers through the
[Host Compute Service (HCS)](https://learn.microsoft.com/virtualization/api/hcs/overview),
the API underneath Docker on Windows and Windows Sandbox.

## Why

Windows ships a capable virtualization API, but there is no first-class command-line
surface over it — the usual answers are Docker (containers only, its own daemon) or
Hyper-V Manager and PowerShell cmdlets (VMs only, VMMS-based, heavyweight). HCS itself
does both, with no management service in the way.

- **[hcsctl](hcsctl.html)** is that command-line surface: images, layers, containers,
  VMs, networks and a guest agent as shell commands. Every command has a `--json` form
  with a fixed document shape and exit codes, so programs and agents can drive it as
  reliably as a person can.
- **[AspireHcs](aspirehcs.html)** is the proof of that contract: an
  [Aspire](https://aspire.dev) hosting integration that adds Hyper-V VMs and
  Hyper-V-isolated containers to the Aspire local dev loop — ephemeral resources with
  dashboard state, logs, health checks and Connect buttons. It contains no HCS interop
  of its own; everything goes through `hcsctl --json`.

## Status

Both projects are experimental and pre-1.0, Windows only, with no compatibility
guarantee between releases.

## Source

- [github.com/joshmakestuff/hcsctl](https://github.com/joshmakestuff/hcsctl)
- [github.com/joshmakestuff/AspireHcs](https://github.com/joshmakestuff/AspireHcs)
