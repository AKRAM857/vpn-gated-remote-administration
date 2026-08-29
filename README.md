# Secure Remote Administration via WireGuard VPN

A multi-VM network security lab for securing remote administration over untrusted networks using WireGuard, private DNS, nftables, and segmented network architecture.

The project implements the complete path from a remote client to a private management server, then verifies the security properties of the architecture through packet captures, port scans, routing analysis, and controlled attack scenarios.
## What this project actually does

Rather than "configure a VPN and call it done," this project treats security
claims as things to be demonstrated under controlled observation:

- **Architecture**: a WireGuard gateway sits between a remote administrator
  and a private management network, fronted by private DNS resolution and
  nftables-enforced access control.
- **Threat model**: an observer positioned on the untrusted path between
  client and gateway represents a network-level attacker with no
  cryptographic material and no path into the management network.
- **Verification, not assertion**: 10 controlled experiments use packet
  capture, port scanning, and negative-case testing (wrong keys, unauthorized
  peers) to demonstrate — not just claim — confidentiality, peer
  authentication, and reduced attack surface.

## Why this project

Administrators often manage infrastructure from networks they do not control. Exposing SSH or other management services directly to those networks increases the attack surface.

This project uses a WireGuard gateway as the controlled entry point to a private management network. The goal is not only to build the architecture, but to verify what an observer on the untrusted network can actually see and what an unauthorized client can and cannot access.


## Lab topology

5 VMs on VirtualBox: **Client**, **Router** (doubles as the network observer
via `tcpdump`/`tshark`), **WireGuard Gateway** (VPN + routing + firewall),
and **Management Server** (also runs private DNS via BIND9).

Full topology, addressing plan, and threat model: [`report/03-lab-architecture`](./report/03-lab-architecture).

## Report structure

| Chapter | Covers |
|---|---|
| 1 — Introduction | Motivation, problem statement, objectives |
| 2 — Theory | L2/L3 separation, underlay/overlay, tunneling, WireGuard internals, cryptographic foundations |
| 3 — Lab Architecture | VM roles, threat model, addressing plan |
| 4 — Network & DNS | VirtualBox networking, routing, private DNS zone |
| 5 — WireGuard Implementation | Keys, peers, tunnel config, gateway routing |
| 6 — Firewall Integration | nftables rules on gateway and management server |
| 7 — Verification Experiments | 10 packet-capture-based experiments (see below) |
| 8 — Analysis | What was proven vs. explicitly out of scope, limitations, comparison to alternative VPN architectures |

## Experiments

Baseline plaintext capture · WireGuard tunnel opacity · SSH end-to-end trace ·
DNS-through-tunnel visibility · port scan (exposed vs. gated) · unauthorized
peer rejection · incorrect server key rejection · AllowedIPs routing behavior
· routing table diffing · packet structure/encapsulation analysis.

Evidence (pcaps, screenshots, notes) for each: [`experiments/`](./experiments).

## Status
🚧 Work in progress — currently in the front-matter / Chapter 1 stage.

## License
TBD
