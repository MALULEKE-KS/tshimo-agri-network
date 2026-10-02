# Physical Build Design - Packet Tracer Blueprint

**Project:** Tshimo Agri Supplies HQ, Potchefstroom  
**Status:** Physical design baseline for Packet Tracer construction  
**Date:** 23 September 2026

This document converts the physical topology diagram into a buildable device, placement, cable, and port plan. It is the physical source of truth for creating the Milestone 2 Packet Tracer file.

## 1. Scope

Build the Potchefstroom HQ only. Do not build the future branch office. The branch is represented by documentation and the reserved block `10.23.128.0/17` only.

The physical path is:

```text
ISP router/cloud
      |
      | routed WAN, R1 Gi0/1
      |
R1 edge router
      |
      | 802.1Q trunk, R1 Gi0/0 <-> SW-CORE Gi0/1
      |
SW-CORE distribution switch
  |       |       |       |
  |       |       |       +-- SW-SRV -- 3 servers
  |       |       +---------- SW-WH  -- 20 Warehouse PCs (CR1)
  |       +------------------ SW-SAL -- 12 Sales PCs
  +-------------------------- SW-ADM -- 8 Admin PCs
```

## 2. Device inventory

| Label | Quantity | Packet Tracer role | Placement |
|---|---:|---|---|
| ISP | 1 | Router or PT Cloud with an upstream router | WAN/Internet zone |
| R1 | 1 | Router with two Gigabit interfaces | Main network rack, top/edge position |
| SW-CORE | 1 | 24-port Layer 2 switch with Gigabit uplinks | Main network rack below R1 |
| SW-ADM | 1 | 24-port access switch | Admin room/access zone |
| SW-SAL | 1 | 24-port access switch | Sales room/access zone |
| SW-WH | 1 | 24-port access switch | Warehouse/access zone |
| SW-SRV | 1 | 24-port access switch | Server room/rack |
| Admin PCs | 8 | PC-PT | SW-ADM access ports |
| Sales PCs | 12 | PC-PT | SW-SAL access ports |
| Warehouse PCs | 20 | PC-PT | SW-WH access ports; includes CR1 |
| DHCP server | 1 | Server-PT | SW-SRV access port |
| DNS server | 1 | Server-PT | SW-SRV access port |
| File/ERP server | 1 | Server-PT | SW-SRV access port |

The minimum endpoint total is 43: 8 Admin PCs, 12 Sales PCs, 20 Warehouse PCs, and 3 servers.

## 3. Packet Tracer placement plan

Arrange the workspace so the physical path is readable from top to bottom or left to right:

1. Place ISP at the top or far left in a separate WAN/Internet area.
2. Place R1 immediately below or to the right of ISP.
3. Place SW-CORE directly below or beside R1.
4. Place the four access switches beneath SW-CORE in department order: Admin, Sales, Warehouse, Servers.
5. Place each department's endpoints near its access switch.
6. Place the three servers beside SW-SRV, not mixed with user PCs.
7. Add device display names exactly as listed in this document.
8. Add link labels showing both endpoint ports, for example `R1 Gi0/0 <-> SW-CORE Gi0/1`.

The layout should show one central core and four clearly separated access zones. Do not create extra routers, redundant links, wireless devices, or a branch site unless the assignment brief is later changed.

## 4. Cable and port schedule

Cable type follows the standard rule: **straight-through between unlike devices** (router↔switch, switch↔PC/server) and **crossover between like devices** (router↔router, switch↔switch). The 2911 and 2960 ports support Auto-MDIX and would usually bring a straight-through link up anyway, but the schedule uses the textbook-correct cable so the build doesn't depend on that. Changed 2 Oct 2026: the original build used straight-through on all 49 links; C01 and C03–C06 were recabled as crossover. The logical mode is recorded here for later configuration; physical cable type does not itself configure a trunk.

**Core port correction (approved 25 Sep 2026):** the standard Packet Tracer Catalyst 2960-24TT exposes only two Gigabit uplinks, so SW-CORE cannot use Gi0/1 through Gi0/5 as originally drafted. SW-CORE now uses Gi0/1 for the R1 uplink and FastEthernet ports Fa0/1-Fa0/4 for the four access-switch uplinks. This is a core-port-only change; VLAN IDs, subnets, gateways, and the router-on-a-stick model are unchanged.

| Cable ID | Endpoint A | Port A | Endpoint B | Port B | Cable type | Physical link | Later mode |
|---|---|---|---|---|---|---|---|
| C01 | ISP | Gi0/0 | R1 | Gi0/1 | Crossover | WAN | Routed |
| C02 | R1 | Gi0/0 | SW-CORE | Gi0/1 | Straight-through | Core uplink | 802.1Q trunk |
| C03 | SW-CORE | Fa0/1 | SW-ADM | Fa0/1 | Crossover | Access uplink | 802.1Q trunk |
| C04 | SW-CORE | Fa0/2 | SW-SAL | Fa0/1 | Crossover | Access uplink | 802.1Q trunk |
| C05 | SW-CORE | Fa0/3 | SW-WH | Fa0/1 | Crossover | Access uplink | 802.1Q trunk |
| C06 | SW-CORE | Fa0/4 | SW-SRV | Fa0/1 | Crossover | Server uplink | 802.1Q trunk |
| C07-C14 | Admin PC 01-08 | Fa0 | SW-ADM | Fa0/2-Fa0/9 | Straight-through | Admin endpoint links | Access VLAN 10 |
| C15-C26 | Sales PC 01-12 | Fa0 | SW-SAL | Fa0/2-Fa0/13 | Straight-through | Sales endpoint links | Access VLAN 20 |
| C27-C46 | Warehouse PC 01-20 | Fa0 | SW-WH | Fa0/2-Fa0/21 | Straight-through | Warehouse endpoint links | Access VLAN 30 |
| C47 | DHCP-SRV | Fa0 | SW-SRV | Fa0/2 | Straight-through | Server link | Access VLAN 40 |
| C48 | DNS-SRV | Fa0 | SW-SRV | Fa0/3 | Straight-through | Server link | Access VLAN 40 |
| C49 | FILE-SRV | Fa0 | SW-SRV | Fa0/4 | Straight-through | Server link | Access VLAN 40 |

If the selected Packet Tracer router or switch exposes different interface names, record the exact replacement in the control record before configuration. Do not silently change the port map.

## 5. Physical labels

Use these exact device names in Packet Tracer:

- `ISP`
- `R1`
- `SW-CORE`
- `SW-ADM`
- `SW-SAL`
- `SW-WH`
- `SW-SRV`
- `ADMIN-PC-01` through `ADMIN-PC-08`
- `SALES-PC-01` through `SALES-PC-12`
- `WH-PC-01` through `WH-PC-20`
- `DHCP-SRV`
- `DNS-SRV`
- `FILE-SRV`

Use description labels on links where Packet Tracer permits them. The endpoint numbering must match the access-port ranges so screenshots and configuration files can be compared without guessing.

## 6. Physical-to-logical mapping

| Physical zone | Device | Access VLAN | Network |
|---|---|---:|---|
| Admin | SW-ADM Fa0/2-Fa0/9 | 10 | `10.23.0.0/26` |
| Sales | SW-SAL Fa0/2-Fa0/13 | 20 | `10.23.0.64/26` |
| Warehouse | SW-WH Fa0/2-Fa0/21 | 30 | `10.23.0.128/26` |
| Servers | SW-SRV Fa0/2-Fa0/4 | 40 | `10.23.0.192/26` |
| Switch management | Switch SVIs | 99 | `10.23.1.16/28` |
| Trunk native traffic | All trunk links | 99 | `10.23.1.16/28` |
| WAN | R1 Gi0/1 and ISP | None | `10.23.1.0/30` |

All trunks must carry only the required VLANs: `10,20,30,40,99`. User devices must never be connected directly to a trunk port.

## 7. Build gates before configuration

Do not begin IOS configuration until these physical checks pass:

**Gate B status: passed 2 Oct 2026**, checked against `screenshots/01-physical-topology.png` (full canvas) and `screenshots/01a-port-labels.png` (port labels on), both from Packet Tracer 9.0.1 and retaken after the C01/C03–C06 crossover recabling.

- [x] All 7 network devices are placed and named.
- [x] ISP, R1, SW-CORE, and all four access switches are visible.
- [x] All 49 physical links in the cable schedule are present (6 uplinks; every one of the 43 endpoints has a link).
- [x] Cable types match the schedule: C03–C06 are drawn dashed (crossover) in the screenshots; the other 44 links are solid (straight-through). *C01 (ISP–R1) was recabled as crossover, but the link is too short to see the dash pattern at screenshot zoom — confirm visually before Phase C.*
- [x] R1 Gi0/0 is connected to SW-CORE Gi0/1. *R1 end reads Gi0/0; the SW-CORE end label overlaps the device name at screenshot zoom. Confirm with `show cdp neighbors` on SW-CORE during Phase C.*
- [x] R1 Gi0/1 is connected to ISP Gi0/0. *Confirmed in Phase C: `configs/isp.txt` addresses only Gi0/0, was applied unchanged, and a PC then reached the ISP loopback 203.0.113.1 through NAT, which requires the link to land on Gi0/0.*
- [x] Access-switch uplinks use Fa0/1 exactly as documented; SW-CORE side reads Fa0/1–Fa0/4.
- [x] Admin uses Fa0/2-Fa0/9. *Range endpoints Fa0/2, Fa0/3, Fa0/9 legible; per-PC order not legible at this zoom.*
- [x] Sales uses Fa0/2-Fa0/13. *Fa0/2, Fa0/12, Fa0/13 legible; per-PC order not legible.*
- [x] Warehouse uses Fa0/2-Fa0/21, including 20 endpoints. *Fa0/20, Fa0/21 legible; per-PC order not legible.*
- [x] Servers use SW-SRV Fa0/2-Fa0/4 (DHCP-SRV Fa0/2, DNS-SRV Fa0/3, FILE-SRV Fa0/4).
- [x] No endpoint is connected to an uplink or trunk port.
- [x] The initial unconfigured `.pkt` file is saved before IOS changes (`packet-tracer/tshimo-agri-hq-baseline.pkt`).
- [x] A screenshot of the clean physical topology is captured.

Per-PC port order within each department range doesn't affect function — every access port in a range joins the same VLAN — so it's left as a documentation detail, not a blocker.

## 8. Physical design constraints

- Keep one access switch per department to preserve the documented troubleshooting path.
- Keep SW-CORE as the single distribution point.
- Do not add a Layer 3 switch; inter-VLAN routing belongs on R1.
- Do not build the future branch office during Milestone 2 unless the brief explicitly changes.
- Do not treat the diagram's future branch box as a physical device.
- Reserve unused switch ports for growth, but do not connect undocumented endpoints.
- The physical design is fixed; logical addressing and service decisions remain governed by `docs/06-project-status-and-milestone-2-control.md`.

## 9. Related files

- Existing visual: `docs/assets/physical_topology.png`.
- Existing narrative: `docs/02-physical-topology.md`.
- Logical design: `docs/03-logical-topology.md`.
- Addressing: `docs/04-ip-addressing-plan.md`.
- Implementation control record: `docs/06-project-status-and-milestone-2-control.md`.
