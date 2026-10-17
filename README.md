# Tshimo Agri Supplies — Network Design & Implementation

CMPG 325 Computer Networks individual semester project, North-West University (Mahikeng campus).

- **Student:** Kurhula Success Maluleke (48277444)
- **Project ID:** CMPG325-2026-042 &nbsp;|&nbsp; **Client ID:** CLI-042
- **Client:** Tshimo Agri Supplies, Potchefstroom (agriculture industry)
- **Assigned troubleshooting challenge:** Network Troubleshooting — fault isolation scenario (Advanced difficulty)

> **Project status: complete.** All three graded stages (Milestone 1, Milestone 2, Final Submission) have been submitted through eFundi. The repository is now a finished record; see [Status](#status) for what was built versus what is only a proposal.

## Outcome at a glance

| | |
|---|---|
| Network built in Cisco Packet Tracer 9.0.1 | 7 network devices, 40 PCs, 3 servers, 49 links |
| VLANs | 10 ADMIN · 20 SALES · 30 WAREHOUSE · 40 SERVERS · 99 MGMT-NATIVE |
| Functional tests | **12 / 12 passed** ([test log](docs/10-test-results.md)) |
| Change Request CR1 (+8 Warehouse staff) | Absorbed on the live network with no subnet, gateway, trunk or router change |
| Assigned fault (VLAN pruned from a trunk) | Injected, isolated layer by layer, fixed and verified ([record](troubleshooting/advanced-fault-isolation.md)) |
| Branch office | Address space reserved (`10.23.128.0/17`); a design **proposal** only, not built ([proposal](docs/11-branch-office-design-proposal.md)) |

## Start here — where to find each assessed item

| Rubric component | Weight | Where the evidence is |
|---|---:|---|
| Client requirements & network design | 15% | [Milestone 1 report](submissions/milestone-1/Milestone1_ClientDesignReview_Maluleke_48277444.pdf), [`docs/01–05`](docs/), and Section 2 of the [Final technical report](submissions/final/Final_TechnicalReport_Maluleke_48277444.pdf) (decisions, alternatives, addressing arithmetic) |
| Packet Tracer implementation & functionality | 35% | [`packet-tracer/`](packet-tracer/) (final `.pkt`), [`configs/`](configs/) (one IOS file per device), [`screenshots/`](screenshots/), [test log](docs/10-test-results.md) |
| Assigned feature / technical challenge | 20% | [`troubleshooting/advanced-fault-isolation.md`](troubleshooting/advanced-fault-isolation.md) and Section 5 of the Final report |
| GitHub portfolio of evidence | 15% | This repository: layered docs, per-device configs, 20 numbered screenshots, three frozen submission packages, and the commit history |
| Video demonstration & technical defence | 15% | Submitted through eFundi (not stored here for size). Prep material: [`docs/12`](docs/12-final-video-script-and-defence-prep.md) |

## What this project is

A full network design, simulation, and fault-isolation exercise for a fictional agri-supply client, built entirely in Cisco Packet Tracer against an assigned addressing block of `10.23.0.0/16`. The brief requires a design that can absorb two future changes without re-addressing:

1. **Branch office** — may open within 18 months
2. **Change Request CR1** — client hires 8 additional staff into the Warehouse/Logistics department

The brief does not give the client's departments or headcounts. The structure used (Admin 8, Sales 12, Warehouse 12 + 8 under CR1, 3 servers) is an **assumption** made at Milestone 1; the design method does not depend on those exact numbers.

## Design summary

Single edge router (R1, router-on-a-stick with DHCP relay and NAT/PAT to the ISP) → core switch (802.1Q trunking) → one access switch per department → end devices. Four business VLANs plus a management/native VLAN, each addressed as a uniform `/26` (62 usable hosts) regardless of current headcount, so headcount growth (like CR1) never forces a re-address. HQ occupies two contiguous `/24`s that summarise as `10.23.0.0/23`, and the upper half of the allocation, `10.23.128.0/17`, is reserved untouched for the branch. See [`docs/04-ip-addressing-plan.md`](docs/04-ip-addressing-plan.md) for the full addressing logic.

## Repository structure

```
tshimo-agri-network/
├── README.md
├── docs/                          design and implementation record
│   ├── 01-client-requirements.md
│   ├── 02-physical-topology.md
│   ├── 03-logical-topology.md
│   ├── 04-ip-addressing-plan.md
│   ├── 05-change-requests.md
│   ├── 06–10                      control record, physical build, execution map, build walkthrough, test results
│   ├── 11-branch-office-design-proposal.md
│   ├── 12-final-video-script-and-defence-prep.md
│   └── assets/                    topology diagrams, banners
├── submissions/                   frozen, graded deliverables — one folder per stage
│   ├── milestone-1/               design package (PDF, HTML, DOCX, eFundi text)
│   ├── milestone-2/               implementation report (PDF, HTML), .pkt, figures, eFundi text
│   └── final/                     technical report (PDF, HTML), final .pkt, figures, eFundi text
├── packet-tracer/                 baseline (unconfigured) and milestone-2 (configured, repaired) .pkt
├── configs/                       Cisco IOS configuration command sets, one file per device
├── screenshots/                   connectivity and configuration evidence, numbered by test
└── troubleshooting/               the advanced fault-isolation scenario: fault, isolation method, fix, verification
```

`docs/` is the living design record. `submissions/` holds a frozen snapshot of exactly what was submitted at each stage, so later edits to `docs/` never retroactively change what was actually graded. Documents `06`, `08` and `09` are historical planning records kept as written; the evidence of what was actually done is in `10`, `screenshots/`, `configs/` and the change log in `06`.

## Status

| Stage | Due | Status |
|---|---|---|
| Milestone 1 — Client Design Review | 28 Aug 2026 | Complete and submitted — [`submissions/milestone-1/`](submissions/milestone-1/) |
| Milestone 2 — Client Implementation Review | 2 Oct 2026 | Complete and submitted — network configured in Packet Tracer, 12/12 tests passing, assigned fault demonstrated and repaired — [`submissions/milestone-2/`](submissions/milestone-2/) |
| Final submission | 16 Oct 2026 | Complete and submitted — technical report and final `.pkt` in [`submissions/final/`](submissions/final/); inset video submitted through eFundi |

**Built versus proposed.** Built, tested and evidenced: the HQ network, CR1, and the assigned fault demonstration. Proposed only (not built, not simulated): the branch-office design in [`docs/11`](docs/11-branch-office-design-proposal.md), and the improvements listed in Section 8 of the Final report (redundancy, inter-VLAN ACLs, port security, DHCP snooping, secure remote access).

The three submissions are separate frozen packages; later work never edits an earlier one.

## Opening the Packet Tracer files

Open with Cisco Packet Tracer 9.0.1.

- [`packet-tracer/tshimo-agri-hq-milestone-2.pkt`](packet-tracer/tshimo-agri-hq-milestone-2.pkt) — the configured network in its repaired, healthy state. Identical to the files in `submissions/milestone-2/` and `submissions/final/`.
- [`packet-tracer/tshimo-agri-hq-baseline.pkt`](packet-tracer/tshimo-agri-hq-baseline.pkt) — the unconfigured physical build (cabling checkpoint, before any IOS configuration).

To check the network is healthy: on SW-CORE run `show interfaces trunk` — every trunk must allow VLANs `10,20,30,40,99` with native VLAN 99.

## Documents

- **[Milestone 1 — Client Design Review (PDF)](submissions/milestone-1/Milestone1_ClientDesignReview_Maluleke_48277444.pdf)** — the full design package: requirements, both topology diagrams, the complete addressing plan, and the initial repository plan.
- [01 — Client Requirements](docs/01-client-requirements.md)
- [02 — Physical Topology](docs/02-physical-topology.md)
- [03 — Logical Topology](docs/03-logical-topology.md)
- [04 — IP Addressing Plan](docs/04-ip-addressing-plan.md)
- [05 — Change Requests](docs/05-change-requests.md)
- [06 — Project Status and Control Record](docs/06-project-status-and-milestone-2-control.md)
- [07 — Physical Build Design](docs/07-physical-build-design.md)
- [08 — Milestone 2 Execution Map](docs/08-milestone-2-execution-map.md) *(historical plan)*
- [09 — Packet Tracer Build Walkthrough](docs/09-packet-tracer-build-walkthrough.md) *(historical build guide)*
- [10 — Milestone 2 Test Results](docs/10-test-results.md)
- [11 — Branch Office Design Proposal](docs/11-branch-office-design-proposal.md) — design only; not built or simulated
- [12 — Final Video Script and Defence Prep](docs/12-final-video-script-and-defence-prep.md)
- [Advanced Fault Isolation record](troubleshooting/advanced-fault-isolation.md)
- **[Milestone 2 — Client Implementation Review (PDF)](submissions/milestone-2/Milestone2_ClientImplementationReview_Maluleke_48277444.pdf)** — the implementation report with all testing and troubleshooting evidence.
- **[Final Submission — Technical Report (PDF)](submissions/final/Final_TechnicalReport_Maluleke_48277444.pdf)** — design rationale, implementation, testing, the assigned fault, growth (CR1 and the branch proposal), issues, limitations and reflection.
