# 04 — IP Addressing Plan

## Assigned block

`10.23.0.0/16` — 65,536 addresses, allocated to Tshimo Agri Supplies (Client ID CLI-042) for this design.

## Full address table

| Purpose | VLAN | Subnet | Gateway | Usable range | Hosts available |
|---|---|---|---|---|---|
| Admin/Management | 10 | `10.23.0.0/26` | `10.23.0.1` | `.2 – .62` | 62 |
| Sales | 20 | `10.23.0.64/26` | `10.23.0.65` | `.66 – .126` | 62 |
| Warehouse/Logistics | 30 | `10.23.0.128/26` | `10.23.0.129` | `.130 – .190` | 62 |
| Servers | 40 | `10.23.0.192/26` | `10.23.0.193` | `.194 – .254` | 62 |
| Mgmt/Native | 99 | `10.23.1.16/28` | `10.23.1.17` | `.18 – .30` | 14 |
| Guest Wi-Fi (future) | — | `10.23.1.32/28` | `10.23.1.33` | `.34 – .46` | 14 |
| R1–ISP WAN link | — | `10.23.1.0/30` | n/a (P2P) | `.1 – .2` | 2 |
| Branch office (reserved) | — | `10.23.128.0/17` | — | — | 32,768 |

## Design logic

**Uniform /26 per department, regardless of current headcount.** Admin (8 staff) and Warehouse (12, growing to 20 under CR1) both get a `/26` — 62 usable addresses. This is deliberate over-provisioning: it means CR1 (adding 8 staff to Warehouse) is satisfied entirely within the existing `10.23.0.128/26` subnet — no VLSM re-carve, no re-addressing, no config change to any other device on the network. The only cost is address space, and at a `/16` block, address space is not scarce.

**Two contiguous /24s for HQ.** Business VLANs (10, 20, 30, 40) sit inside `10.23.0.0/24`; infrastructure — the WAN link, management/native VLAN, and reserved guest Wi-Fi — sits inside `10.23.1.0/24`. Because these two `/24`s are contiguous, the whole of HQ can be advertised upstream (or documented for the ISP) as a single summary route: `10.23.0.0/23`. This keeps the routing table small and the addressing plan easy to reason about — the entire current site fits in a well-defined, summarisable block.

**`10.23.128.0/17` reserved for the branch office.** Half of the entire `/16` allocation — 32,768 addresses — is set aside, untouched, for the branch office the client may open within 18 months. When that happens, the branch gets its own VLAN/subnet plan carved out of this block, its own summary route, and connects back to HQ (VPN or WAN link) without any renumbering at HQ. This directly satisfies the brief's "no redesign" constraint for the branch-office scenario, the same way the uniform `/26`s satisfy it for CR1.

**Everything between `10.23.1.0/24`'s end and `10.23.128.0/17`'s start** (i.e. most of `10.23.2.0/24` through `10.23.127.0/24`) is unallocated headroom — available for additional infrastructure VLANs (a second guest network, IoT/sensors relevant to an agri client, a VoIP VLAN) without touching either the HQ business subnets or the branch-office reservation.

## Why /16 wasn't subnetted evenly across all needs upfront

A single VLSM scheme covering HQ, branch office, and all future growth in one pass would need to guess at the branch office's size and structure now — information the brief doesn't provide (branch may open in up to 18 months, structure unknown). Instead, this plan fixes HQ's shape now (it's known) and reserves a large, untouched block for the branch office to be designed properly once its actual requirements exist — consistent with the instruction to design for the constraint, not to fabricate detail the client hasn't specified.
