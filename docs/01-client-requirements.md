# 01 — Client Requirements

## Client

Tshimo Agri Supplies, Potchefstroom. Operates in the agriculture supply industry — the brief did not specify further detail (e.g. whether they supply seed, feed, equipment, or a mix), so the design assumes a general agri-supply distributor: a small office function (admin, sales) plus a warehouse/logistics operation that is the largest headcount department.

## Given constraints (from the assignment brief)

- Assigned addressing block: `10.23.0.0/16`
- Assigned networking challenge: **Network Troubleshooting** — fault isolation scenario, Advanced difficulty
- A branch office may open within 18 months — the addressing plan and topology must accommodate this without redesigning the existing network
- **Change Request CR1**: the client will sign on 8 additional staff in one department — the network must absorb this without redesign

## Assumption: department structure

Tshimo Agri Supplies' organisational structure was defined as part of this design, based on a realistic staffing model for a small-to-medium agricultural supply distributor. It forms the basis for the topology and addressing plan below.

| Department | Assumed headcount | Rationale |
|---|---|---|
| Admin/Management | 8 | Small office function: owner/management, finance, HR |
| Sales | 12 | Client-facing and order desk staff for an agri-supply distributor |
| Warehouse/Logistics | 12 (+8 under CR1 = 20) | Largest department — stock handling, dispatch, delivery coordination; the department CR1 grows |
| Servers | 3 | DHCP, DNS, and file/print services, hosted on-prem |

## Derived requirements

1. **Segmentation** — each department isolated on its own VLAN/subnet, so broadcast domains and access policy can be managed independently.
2. **Growth headroom** — every department subnet must have enough free host addresses to absorb realistic headcount growth (CR1 being the concrete test case) without changing the subnet boundary.
3. **Future branch office** — a contiguous, currently-unused portion of the `/16` block must be reserved so a second site can be added later using the same addressing scheme, without renumbering HQ.
4. **Internet access** — outbound access via a single ISP-facing WAN link, translated through NAT/PAT (client has no public IP allocation of its own).
5. **Internal services** — DHCP, DNS, and file sharing hosted internally rather than relying on the ISP or cloud, consistent with a small business that wants control over its own infrastructure and predictable costs.
6. **Troubleshooting readiness** — the design must be complex enough (multiple VLANs, a trunk, a routed uplink) to support a genuine Advanced-level fault-isolation exercise — see the `troubleshooting/` folder for the fault scenario itself.

These requirements drove the addressing and topology decisions documented in [`02`](02-physical-topology.md), [`03`](03-logical-topology.md), and [`04`](04-ip-addressing-plan.md).
