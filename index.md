---
layout: home
title: Home
---

# hcsctl & AspireHcs

Two projects for running Hyper-V virtual machines and Windows containers through the
[Host Compute Service (HCS)](https://learn.microsoft.com/virtualization/api/hcs/overview),
the API underneath Docker on Windows and Windows Sandbox.

## Before you start

Both projects run on **Windows with the Hyper-V feature enabled** (Windows 10 1809 /
Windows Server 2019 or later). Nothing escalates on its own; what your token holds
decides what runs:

- **Join the Hyper-V Administrators group.** VMs and Hyper-V-isolated containers then run
  from an ordinary, unelevated shell — including the whole AspireHcs dev loop. Membership
  takes effect after signing out and back in.
- **Elevate once per image.** `hcsctl image import` (and `export`, `rm`, `layer mount`,
  every `storage` command, process-isolated containers) need an Administrator shell.
- **Bring your own VM image.** A bootable Gen2/UEFI VHDX with the
  [hcsguest agent](hcsctl/guest-agent.html) installed; neither project installs
  operating systems.

`hcsctl info` reports what the current token can run; `hcsctl help` marks the commands
that need elevation.

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
