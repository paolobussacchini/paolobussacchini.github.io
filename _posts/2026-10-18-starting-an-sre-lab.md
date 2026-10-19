---
layout: post
title: "Starting an SRE lab: CoreOS, Ignition, and the laptop->cluster path"
date: 2026-10-18
categories: [sre-lab, homelab]
tags: [sre, coreos, ignition, virtualbox, kubernetes, ssh, learning]
---

This is the first public note for **sre-lab** - a personal learning path toward platform / SRE work: IaC, lightweight Kubernetes, GitOps, and a small Go service. Before Terraform or Argo CD, I wanted a grounded base: a VM I can rebuild, a declarative first-boot config, and SSH that works without console passwords.

What follows is educational and sanitised. No secrets, no company runbooks, no employer internals - just patterns anyone can practice on a laptop.

## Why a lab at all

On the CV, *sre-lab* is marked *in progress*: practice the delivery loop from laptop to cluster. The first slice is deliberately small:

1. Run an immutable-ish Linux appliance in a local hypervisor.
2. Configure it once with **Ignition** (from a readable **Butane** file).
3. Reach it over **SSH** with a key.
4. Only then: pin a lightweight Kubernetes install, Helm, and a smoke PVC to see where data lives.

That order matters. Skipping the OS/network layer and jumping straight to "install k3s somehow" hides the failure modes you need for day-2 thinking.

## The mental model

```text
Laptop (host OS)
  └── Hypervisor (e.g. VirtualBox)
        └── VM disk
              └── Fedora CoreOS (first boot applies Ignition)
                    └── sshd + your user
                          └── (later) k3s -> Helm -> PVC smoke test
```

**Fedora CoreOS** fits this lab because it is built around Ignition and a "don't casually `dnf install` forever" mindset. Day-0 config belongs in the recipe, not in ad-hoc shell history.

## Butane -> Ignition (the recipe)

- **Butane** (`.bu`): human-readable YAML - hostname, user, public SSH key, directories, systemd units you will add later.
- **Ignition** (`.ign`): machine JSON applied **once** on first boot.
- The `butane` tool compiles A -> B.

Typical flow on a laptop: compile the Ignition file, serve it with a tiny HTTP server on the host, boot a **CoreOS Live** ISO in the VM, and install to disk with something like `coreos-installer` pointing at that URL.

Two lessons that cost me time (and are worth writing down):

1. **Product, not version number.** A Fedora *Workstation* live ISO is the wrong tool for this path. You want **Fedora CoreOS Live** (appliance + installer + Ignition).
2. **Encoding.** On Windows PowerShell, redirecting JSON with `>` can yield UTF-16. The HTTP download may look fine; Ignition parse will not. Prefer explicit UTF-8 when writing the `.ign` file.

The `version` field in a Butane file is the **schema** version of that file format - not "which Fedora release I installed."

## Networking: why `127.0.0.1:2222`

With VirtualBox **NAT**, the guest often sits on an internal address (commonly `10.0.2.15`) that your LAN never sees. Port-forward host `2222` -> guest `22`, then:

```text
ssh -i <private-key> -p 2222 core@127.0.0.1
```

Bridge mode can work depending on Wi-Fi/NIC quirks; NAT + forward is a reliable default for a single laptop lab.

## SSH without a console password

Put the **public** key in Butane / on the guest `authorized_keys`. Keep the **private** key only on the host. CoreOS lab images often have **no** password login on the console - that is expected. Missing `-i` or `-p` usually shows up as `Permission denied (publickey)`.

Checkpoint when it works: expected hostname, guest interface up, user `core` (or whatever you declared), SSH only via key.

## How this ties to sre-lab

| Lab slice | Practice |
|---|---|
| CoreOS + Ignition + SSH | Rebuildable machine identity |
| Next: pinned k3s + Helm + PVC | Where workloads and data live on the node |
| Later: Terraform / Go service / Argo CD | IaC + app + GitOps sync |

The CV project description stays the north star; this post is the **foundation chapter** - the part that makes later automation honest instead of magical.

## What I am deliberately not publishing

- Private keys, tokens, or Ignition files with secrets.
- Employer diagrams, internal hostnames, or product-specific installers.
- Copy-paste "production" procedures. This is a **personal** learning loop.

If you are on a similar path: start from an empty disk, write a Butane file you understand, get SSH working, then add one systemd one-shot at a time. Reprovision from scratch at least twice - that is the real test.

---

*First published note for sre-lab. An earlier Italian CoreOS walkthrough remains in `_drafts/` for my own notes.*
