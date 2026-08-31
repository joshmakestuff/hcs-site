---
layout: page
title: Networks
permalink: /hcsctl/networks.html
---

# Handling networks

hcsctl works with [host compute networks (HCN)](https://learn.microsoft.com/virtualization/api/hcn/overview) —
the same networks Docker on Windows and WSL use. Containers and VMs attach to one by
name with `--network`. Every Windows client SKU with Hyper-V already has the Default
Switch, so you can attach without creating anything.

## Inspecting

All unelevated:

```
hcsctl network ls                          # networks, subnets, endpoint counts
hcsctl network endpoints --network my-nat  # endpoints and their addresses
hcsctl network inspect --name my-nat       # the effective HCN document
```

## Creating

Two forms — an explicit NAT network or an isolated private one:

```
hcsctl network create --name my-nat --type nat --subnet 172.30.0.0/24 --gateway 172.30.0.1
hcsctl network create --name lab --type private
```

Windows permits one NAT network per host. Hosts that run Docker or WSL already have it,
and a second one can break both — do not create a NAT network on such a host.

## Using

NAT networks require `--dns` when creating a VM, and support port publishing on
container endpoints (fixed at creation, not changeable at runtime):

```
hcsctl vm create --vhdx C:\images\rocky-10.vhdx --network my-nat --dns 1.1.1.1
hcsctl container create --ref mcr.microsoft.com/windows/servercore:ltsc2022 \
                        --network my-nat --publish 39082:8082/tcp
```

## Removing

```
hcsctl network rm --name my-nat
```

Only removes an empty network; it refuses to remove its endpoints.

## More details

- [docs/usage.md](https://github.com/joshmakestuff/hcsctl/blob/main/docs/usage.md) —
  elevation requirements and worked examples
