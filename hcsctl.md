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

**Requirements:** Windows with the Hyper-V feature enabled. Most operations need an
elevated (Administrator) shell.

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

## More details

- [docs/usage.md](https://github.com/joshmakestuff/hcsctl/blob/main/docs/usage.md) —
  the `--json` contract, elevation requirements, container isolation modes, the guest
  agent, worked examples
- [Issues](https://github.com/joshmakestuff/hcsctl/issues) — work in progress
