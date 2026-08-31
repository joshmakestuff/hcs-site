---
layout: page
title: Guest agent
permalink: /hcsctl/guest-agent.html
---

# The guest agent

`hcsguest` runs inside a VM as a service and talks to the host over a Hyper-V socket —
no NIC, no DHCP lease, no host elevation. It ships with hcsctl releases; host and guest
share a wire protocol, so pin the guest to the host's release tag.

## What it enables

```
hcsctl guest info    --vmid <guid>                 # what the guest says about itself
hcsctl guest exec    --vmid <guid> --cmd "..."     # run a command in the guest
hcsctl guest forward --vmid <guid> --port 22       # publish a guest TCP port on the host
hcsctl vm ip         --id <guid>                   # guest-reported addresses
hcsctl vm netconfig  --id <guid>                   # program the guest NIC (static networks)
```

`vm ip` reads the address from the guest, not the host network stack — a VM without
the agent reports no address. [AspireHcs](../aspirehcs.html) builds on the same
channel: VM readiness, endpoint addresses, and environment delivery into the guest all
come through the agent (agentless appliances use `WithGuestAddress` instead).

## Installing

Run inside the VM, substituting the host's hcsctl release tag for `v0.7.0` — there is
no `latest` form.

Windows, elevated PowerShell:

```powershell
& ([scriptblock]::Create((New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/joshmakestuff/hcsctl/v0.7.0/install/Install-HcsGuest.ps1'))) -Version v0.7.0
```

Linux, as root (systemd):

```sh
curl -fsSL https://raw.githubusercontent.com/joshmakestuff/hcsctl/v0.7.0/install/install-hcsguest.sh | sh -s -- -v v0.7.0
```

The scripts verify checksums before touching anything and are safe to rerun for repair
or upgrade. Air-gapped guests: copy the release artifact in and pass `-Path` / `-p`.
The Hyper-V socket transport must be present — in-box on Windows, `hv_sock` on stock
Linux kernels.

## Verifying from the host

```
hcsctl vm create --vhdx <path> --network default   # prints the VM id
hcsctl vm start  --id <id>
hcsctl guest info --vmid <id>                      # must answer: the agent is up
hcsctl vm ip     --id <id>                         # must print an address: DHCP works
hcsctl vm rm     --id <id> --force
```

## More details

- [install/README.md](https://github.com/joshmakestuff/hcsctl/blob/main/install/README.md) —
  installer flags, checksum and rollback behavior
- [examples/packer](https://github.com/joshmakestuff/hcsctl/blob/main/examples/packer/README.md) —
  building images with the agent already installed
