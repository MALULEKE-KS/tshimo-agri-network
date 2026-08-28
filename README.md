# Tshimo Agri Supplies — Network Design

CMPG 325 Computer Networks individual semester project, North-West University (Mahikeng campus).

- **Student:** Kurhula Success Maluleke (48277444)
- **Project ID:** CMPG325-2026-042 &nbsp;|&nbsp; **Client ID:** CLI-042
- **Client:** Tshimo Agri Supplies, Potchefstroom (agriculture industry)
- **Assigned troubleshooting challenge:** Network Troubleshooting — fault isolation scenario (Advanced difficulty)

## What this project is

A full network design, simulation, and fault-isolation exercise for a fictional agri-supply client, built entirely in Cisco Packet Tracer against an assigned addressing block of `10.23.0.0/16`. The brief requires a design that can absorb two future changes without re-addressing:

1. **Branch office** — may open within 18 months
2. **Change Request CR1** — client hires 8 additional staff into the Warehouse/Logistics department

## Repository structure

| Path | Contents |
|---|---|
| `docs/` | Design documentation — requirements, topology, addressing plan, change requests, plus the Milestone 1 submission and diagrams |
| `packet-tracer/` | The working `.pkt` file |
| `configs/` | Cisco IOS configuration command sets, one file per device |
| `screenshots/` | Connectivity and configuration evidence |
| `troubleshooting/` | The advanced fault-isolation scenario: fault, isolation method, fix, verification |

## Design summary

Single edge router (R1, router-on-a-stick with NAT/PAT to the ISP) → core switch (802.1Q trunking) → one access switch per department → end devices. Four business VLANs plus a management/native VLAN, each addressed as a uniform `/26` regardless of current headcount, so headcount growth (like CR1) never forces a re-address. See [`docs/04-ip-addressing-plan.md`](docs/04-ip-addressing-plan.md) for the full addressing logic.

## Status

| Milestone | Due | Status |
|---|---|---|
| Milestone 1 — Client Design Review | 28 Aug 2026 | Design package prepared, not yet submitted |
| Milestone 2 | 2 Oct 2026 | Not started |
| Final submission | 16 Oct 2026 | Not started |

## Documents

- **[Milestone 1 — Client Design Review (PDF)](docs/Milestone1_ClientDesignReview_Maluleke_48277444.pdf)** — the full design package: cover, contents, requirements, both topology diagrams, the complete addressing plan, and the initial repository plan, laid out as a single client-ready report. This is the primary Milestone 1 deliverable.
- [Milestone 1 — Word source (.docx)](docs/Milestone1_ClientDesignReview_Maluleke_48277444.docx) — original editable submission copy
- [01 — Client Requirements](docs/01-client-requirements.md)
- [02 — Physical Topology](docs/02-physical-topology.md)
- [03 — Logical Topology](docs/03-logical-topology.md)
- [04 — IP Addressing Plan](docs/04-ip-addressing-plan.md)
- [05 — Change Requests](docs/05-change-requests.md)
