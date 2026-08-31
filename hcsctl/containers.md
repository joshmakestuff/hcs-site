---
layout: page
title: Containers
permalink: /hcsctl/containers.html
---

# Running containers

hcsctl runs Windows containers straight against HCS — no daemon. Images come from a
registry into a local store, then containers run from that store.

## Images

```
hcsctl image pull   --ref mcr.microsoft.com/windows/servercore:ltsc2022
hcsctl image import --ref mcr.microsoft.com/windows/servercore:ltsc2022   # elevated
hcsctl image ls
```

`pull` downloads (unelevated); `import` expands the layers for HCS (elevated).

## Isolation

`--isolation hyperv` is the default: layers are handed to a utility VM, and the guest
can be an older Windows build than the host. `--isolation process` stacks the layers on
the host — it needs elevation at create, and the image must fall inside the host's
process-isolation compatibility window. `hcsctl info` reports both per image.

## One-shot

`container run` creates, boots, runs one command, and tears down:

```
hcsctl container run --ref mcr.microsoft.com/windows/servercore:ltsc2022 \
                     --cmd "cmd /c ver"
```

## Lifecycle

`--cmd` at create records the primary process; `start` launches it and stays attached
as its pump, writing its output to `primary.log`:

```
hcsctl container create --ref mcr.microsoft.com/windows/servercore:ltsc2022 \
                        --id web --cmd C:\app\web.exe
hcsctl container start  --id web    # stays attached until the primary exits
```

From a second shell:

```
hcsctl container exec --id web --cmd "cmd /c ver"
hcsctl container exec --id web --cmd cmd --interactive --tty   # a real shell
hcsctl container logs --id web --follow
hcsctl container stop --id web
hcsctl container rm   --id web
```

Without `--cmd`, `start` just boots the container and returns; `logs` has nothing to
report, but `exec` works the same.

Also: `ls`, `ps`, `stats`, `inspect`, `pause`/`resume`, `kill`. Networking — attaching
to a network and publishing ports — is on the [networks page](networks.html).

## More details

- [docs/usage.md](https://github.com/joshmakestuff/hcsctl/blob/main/docs/usage.md) —
  elevation per command, the `--json` and `--stream-json` contracts, worked examples
