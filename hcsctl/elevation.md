---
layout: page
title: Elevation
permalink: /hcsctl/elevation.html
---

# Elevation

hcsctl never elevates itself. `hcsctl info` reports what the current token can run, and
`hcsctl help` marks each command that needs elevation.

Membership of the **Hyper-V Administrators** group is the baseline; it takes effect
after signing out and back in. With it, everything below marked unelevated runs from
an ordinary shell.

| Command | Token | Why |
|---|---|---|
| `image pull`, `image ls` | unelevated | registry download and local metadata |
| `image import`, `image export` | **elevated** | computestorage refuses a UAC-filtered token; needs `SeBackupPrivilege` + `SeRestorePrivilege` |
| `image rm` | **elevated** | destroys layers through computestorage |
| `layer mount`, `layer unmount` | **elevated** | host-side VHD attach |
| `storage *` | **elevated** | direct computestorage calls |
| `container` — Hyper-V isolation (default) | unelevated | the scratch is a blank.vhdx copy plus the VM-group ACE; no host attach |
| `container create|run` — process isolation | **elevated** at create | the scratch must be attached and mounted on the host; `--scratch-size` also needs `SeManageVolumePrivilege` |
| `vm *` | unelevated | Hyper-V Administrators membership is sufficient |
| `guest *` | unelevated | Hyper-V socket to the agent |
| `network *` | unelevated | HCN reads and writes both work from a filtered token |
| `cim create|merge|usage|verify|destroy` | unelevated | building a CIM needs no privilege |
| `cim mount|unmount` | **elevated** | the specific right is unidentified; `SeManageVolumePrivilege` is not sufficient |
| `info` | unelevated | |

Typical sequence: elevate once to `image import`, then run containers and VMs
unelevated. [AspireHcs](../aspirehcs.html) relies on this — the AppHost runs
unelevated and refuses process isolation at model-build time.

## More details

- [docs/usage.md § Elevation](https://github.com/joshmakestuff/hcsctl/blob/main/docs/usage.md#elevation)
