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

Both resource types can also consume references: `WithReference(other)` delivers
connection strings and endpoints into the guest.

```csharp
var vm = builder.AddHcsVm("appliance")
    .WithVhdx(@"d:\images\appliance.vhdx")
    .WithMemory(gigabytes: 4)
    .WithNetwork()
    .WithEndpoint(name: "api", targetPort: 8080)
    .WithTcpHealthCheck();

builder.AddProject<Projects.Api>("api").WithReference(vm).WaitFor(vm);
```

All HCS access goes through hcsctl's `--json` contract; this package contains no HCS
interop of its own.

## Getting set up

**Requirements:** Windows with Hyper-V, the .NET SDK, and Aspire. You supply the guest
VM image (a bootable Gen2/UEFI VHDX); AspireHcs does not install operating systems.
Container images come from a registry through `hcsctl image pull` and an elevated
`hcsctl image import`.

VM images need the [hcsguest agent](hcsctl/guest-agent.html): readiness, endpoint
addresses, and environment delivery into the guest all depend on it. Agentless
appliances with a fixed in-guest address use `WithGuestAddress` instead.

- **Consumer documentation** — the builder API, requirements, and setup:
  [src/AspireHcs/README.md](https://github.com/joshmakestuff/AspireHcs/blob/main/src/AspireHcs/README.md)
- **Sample** — a Linux VM, a Windows VM and a Windows container in one AppHost, with
  image preparation steps:
  [samples/HcsSample.AppHost](https://github.com/joshmakestuff/AspireHcs/blob/main/samples/HcsSample.AppHost/README.md)

## Guides

- [Running the sample](aspirehcs/sample.html) — container, Postgres, three kinds of VM, and
  guests consuming endpoints
- [Connect commands](aspirehcs/connect.html) — Connect (SSH)/(RDP)/(Shell), and when
  they use an hvsocket forward instead of the guest's address

## More details

- [Issues](https://github.com/joshmakestuff/AspireHcs/issues) — work in progress
