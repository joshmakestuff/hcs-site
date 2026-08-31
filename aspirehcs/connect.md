---
layout: page
title: Connect commands
permalink: /aspirehcs/connect.html
---

# Connect (SSH) and Connect (RDP)

`WithSshCommand` and `WithRdpCommand` put Connect commands on a VM's dashboard
actions: Connect (SSH) opens a terminal session, Connect (RDP) opens mstsc, each with
the right address and account filled in. `WithShellCommand` is the container
equivalent — Connect (Shell) opens an interactive console inside the container via
`hcsctl container exec --interactive --tty`.

![The appliance resource's actions menu, with Connect (SSH)](../assets/sample/connect-menu.png)

![A Windows VM's actions menu, with Connect (RDP)](../assets/sample/connect-rdp-menu.png)

## Two roads into the guest

A VM with the [hcsguest agent](../hcsctl/guest-agent.html) is reachable two ways:

- **The leased address** — the guest's DHCP address, reported by the agent. Endpoints,
  health checks, and `WithReference` all use it: it is a real network address that
  other resources can reach.
- **An hvsocket forward** — at boot, once `hcsctl guest info` confirms the agent is
  reachable, AspireHcs starts `hcsctl guest forward`, publishing the guest port on a
  host loopback port. This is a pure Hyper-V socket relay: no NIC, no DHCP, no guest
  firewall in the path.

The Connect commands prefer the forward and fall back to the leased address when there
is no agent (agentless appliances declared with `WithGuestAddress`) or the forward has
stopped. The appliance's console log shows the sequence:

```
Guest leased 172.17.75.238 after 32050 ms.
Publishing 1 endpoint(s) at 172.17.75.238.
Wrote 3 environment value(s) to /etc/aspire.env in the guest.
Forwarding 'ssh' to 127.0.0.1:49458 over hvsocket.
```

![Console logs of a VM booting: lease, endpoint publish, env delivery, hvsocket forward](../assets/sample/console-logs.png)

## Why prefer the forward

The forward works when the guest's network does not: NIC disabled, firewall
misconfigured, DHCP broken. Management access survives a fully networkless guest —
verified by disabling the guest's NIC mid-RDP-session and recovering it over the same
channel.

## Why WithReference does not use it

The forward is a host-loopback address, reachable only from the AppHost's machine. A
connection string pointing at it would break any consumer that is itself a guest or
container. `WithReference` therefore stays on the leased address; the forward is
Connect-command-only.
