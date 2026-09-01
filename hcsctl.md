---
layout: page
title: hcsctl
permalink: /hcsctl.html
---

# hcsctl

A command-line interface over the Windows Host Compute Service, built on
[Microsoft/hcsshim](https://github.com/Microsoft/hcsshim). It exposes hcsshim's HCS and
HCN Go APIs as shell commands:

- container images and layers
- Hyper-V and process-isolated containers
- virtual machines
- a VM guest agent (`hcsguest`) for integration scenarios
- host compute networks

Every command has a `--json` form with a fixed document shape and exit codes.

## Getting set up

**Requirements:** Windows with the Hyper-V feature enabled, and membership of the
Hyper-V Administrators group. VM and Hyper-V-isolated container commands run
unelevated; `image import` and the other elevated commands are listed on the
[elevation page](hcsctl/elevation.html).

Download `hcsctl.exe` from the
[latest release](https://github.com/joshmakestuff/hcsctl/releases), or build from
source:

```
go build -o hcsctl.exe .
```

Then:

```
hcsctl help
hcsctl info
```

`hcsctl help` is the command inventory. `hcsctl info` reports what the host supports.

## Guides

- [Elevation](hcsctl/elevation.html) — which commands need an Administrator shell, and why
- [Containers](hcsctl/containers.html) — images, isolation modes, the container lifecycle
- [Virtual machines](hcsctl/vms.html) — booting a VHDX, the guest agent
- [Guest agent](hcsctl/guest-agent.html) — what hcsguest enables and how to install it
- [Networks](hcsctl/networks.html) — inspecting, creating and using host compute networks

## More details

- [docs/usage.md](https://github.com/joshmakestuff/hcsctl/blob/main/docs/usage.md) —
  the `--json` contract, elevation requirements, container isolation modes, the guest
  agent, worked examples
- [Issues](https://github.com/joshmakestuff/hcsctl/issues) — work in progress
