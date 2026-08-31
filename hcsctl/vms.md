---
layout: page
title: Virtual machines
permalink: /hcsctl/vms.html
---

# Running virtual machines

A bootable Gen2/UEFI VHDX becomes an HCS compute system. You supply the image; hcsctl
does not install operating systems.

Create, start, and read the guest's address. The VM id is a GUID because it is also the
VM's Hyper-V socket address:

```
hcsctl vm create --vhdx C:\images\rocky-10.vhdx --network my-nat --dns 1.1.1.1
hcsctl vm start  --id <guid>
hcsctl vm ip     --id <guid>
hcsctl guest exec --vmid <guid> --cmd "uname -a"
```

`vm ip` and `guest exec` need the [hcsguest agent](guest-agent.html) installed in the
image; it answers over a Hyper-V socket, so it works with no NIC or DHCP lease at all.

To run VMs as Aspire resources instead, see [AspireHcs](../aspirehcs.html).

## More details

- [docs/usage.md](https://github.com/joshmakestuff/hcsctl/blob/main/docs/usage.md) —
  data disks, networks, the guest agent
