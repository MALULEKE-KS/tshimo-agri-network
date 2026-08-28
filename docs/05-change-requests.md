# 05 — Change Requests

## CR1 — 8 additional staff, Warehouse/Logistics department

**Request:** the client will sign on 8 additional staff in one department. The network must absorb this without redesign.

**Department affected:** Warehouse/Logistics (VLAN 30) — chosen as the department to grow because it's the largest department in the assumed org structure and the most plausible place for an agri-supply distributor to add headcount (more stock handlers, dispatch, and delivery staff).

**Before CR1:** 12 staff, using 12 of 62 available addresses in `10.23.0.128/26`.

**After CR1:** 20 staff, using 20 of 62 available addresses in the same subnet.

**What changes:**
- Nothing at the addressing or routing layer — the VLAN, subnet, and gateway are unchanged.
- Nothing on R1 — the VLAN 30 sub-interface configuration is unchanged.
- Nothing on SW-CORE — the trunk configuration is unchanged.
- **SW-WH**: 8 additional access ports get assigned to VLAN 30 (already physically present — a 24-port switch was specified for a 12-person department precisely for this reason, see [`02`](02-physical-topology.md)) and 8 new end devices are cabled in.
- **DHCP scope for VLAN 30**: no change needed — the scope already spans the full `/26` and was never restricted to a smaller range.

**Why this counts as "no redesign":** the definition of redesign here is any change to VLAN boundaries, subnet masks, gateway addresses, routing, or trunk configuration. CR1 requires none of these — it's purely a cabling and switchport-assignment change, which is the entire point of provisioning uniform `/26`s regardless of current headcount.

## Future change — branch office within 18 months

**Request:** client may open a branch office; the addressing plan must allow it without redesigning the existing (HQ) network.

**What's already in place for this:** `10.23.128.0/17` — half of the assigned `/16` — is reserved and untouched (see [`04 — IP Addressing Plan`](04-ip-addressing-plan.md)). No HQ subnet, VLAN, or route currently uses any address inside that block.

**What still needs to happen when the branch office is confirmed** (out of scope until the client provides real requirements — headcount, departments, link type back to HQ):
1. Design the branch's own VLAN/subnet plan carved from `10.23.128.0/17`, following the same "uniform subnet size regardless of headcount" logic used at HQ.
2. Choose and configure the inter-site link (site-to-site VPN over the existing ISP connection, or a dedicated WAN link, depending on what the client is willing to pay for — not specified in the brief).
3. Extend routing so HQ and branch summary routes are exchanged (static routes are sufficient at this scale; a dynamic routing protocol is not justified for a two-site network).
4. No change to any HQ VLAN, subnet, gateway, or existing device configuration is required — this is the same "no redesign" guarantee CR1 relies on, just at the scale of a whole additional site rather than one department.

This change request is documented here as a placeholder for Milestone 2/Final, once (or if) the brief or client scenario supplies concrete branch-office requirements to design against.
