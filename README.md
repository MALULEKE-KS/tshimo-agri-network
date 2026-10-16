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

```
tshimo-agri-network/
├── README.md
├── docs/                          living design documentation (source of truth)
│   ├── 01-client-requirements.md
│   ├── 02-physical-topology.md
│   ├── 03-logical-topology.md
│   ├── 04-ip-addressing-plan.md
│   ├── 05-change-requests.md
│   ├── 06–10                      control record, physical build, execution map, build walkthrough, test results
│   ├── 11-branch-office-design-proposal.md
│   ├── 12-final-video-script-and-defence-prep.md
│   └── assets/                    topology diagrams, banners — referenced by docs/ and submissions/
├── submissions/                   frozen, graded deliverables — one folder per milestone
│   ├── milestone-1/               design package (PDF, HTML, DOCX, eFundi text)
│   ├── milestone-2/               implementation report (PDF, HTML), .pkt, figures, eFundi text
│   └── final/                     technical report (PDF, HTML), final .pkt, figures, eFundi text
├── packet-tracer/                 the working .pkt file
├── configs/                       Cisco IOS configuration command sets, one file per device
├── screenshots/                   connectivity and configuration evidence
└── troubleshooting/               the advanced fault-isolation scenario: fault, isolation method, fix, verification
```

`docs/` is the living, continuously-updated design record — it evolves as the project moves through Milestone 2 and Final. `submissions/` holds a frozen snapshot of exactly what was graded at each milestone, so later edits to `docs/` never retroactively change what was actually submitted.

## Design summary

Single edge router (R1, router-on-a-stick with NAT/PAT to the ISP) → core switch (802.1Q trunking) → one access switch per department → end devices. Four business VLANs plus a management/native VLAN, each addressed as a uniform `/26` regardless of current headcount, so headcount growth (like CR1) never forces a re-address. See [`docs/04-ip-addressing-plan.md`](docs/04-ip-addressing-plan.md) for the full addressing logic.

## Status

| Milestone | Due | Status |
|---|---|---|
| Milestone 1 — Client Design Review | 28 Aug 2026 | Complete — package in [`submissions/milestone-1/`](submissions/milestone-1/) |
| Milestone 2 — Client Implementation Review | 2 Oct 2026 | Complete — network configured in Packet Tracer, 12/12 tests passing, assigned fault demonstrated and repaired; package in [`submissions/milestone-2/`](submissions/milestone-2/) |
| Final submission | 16 Oct 2026 | Package prepared in [`submissions/final/`](submissions/final/): technical report and final `.pkt`. The inset video is recorded and uploaded through eFundi (not stored in this repository). The branch-office design is a proposal only: it is not built or simulated |

Milestones 1 and 2 and the Final are three separate frozen packages; later work never edits an earlier one.

## Documents

- **[Milestone 1 — Client Design Review (PDF)](submissions/milestone-1/Milestone1_ClientDesignReview_Maluleke_48277444.pdf)** — the full design package: cover, contents, requirements, both topology diagrams, the complete addressing plan, and the initial repository plan, laid out as a single client-ready report. This is the Milestone 1 deliverable.
- [01 — Client Requirements](docs/01-client-requirements.md)
- [02 — Physical Topology](docs/02-physical-topology.md)
- [03 — Logical Topology](docs/03-logical-topology.md)
- [04 — IP Addressing Plan](docs/04-ip-addressing-plan.md)
- [05 — Change Requests](docs/05-change-requests.md)
- [06 — Project Status and Milestone 2 Control Record](docs/06-project-status-and-milestone-2-control.md)
- [07 — Physical Build Design](docs/07-physical-build-design.md)
- [08 — Milestone 2 Execution Map](docs/08-milestone-2-execution-map.md)
- [09 — Packet Tracer Build Walkthrough](docs/09-packet-tracer-build-walkthrough.md)
- [10 — Milestone 2 Test Results](docs/10-test-results.md)
- [11 — Branch Office Design Proposal](docs/11-branch-office-design-proposal.md) — design only; not built or simulated
- [12 — Final Video Script and Defence Prep](docs/12-final-video-script-and-defence-prep.md)
- [Advanced Fault Isolation record](troubleshooting/advanced-fault-isolation.md)
- **[Milestone 2 — Client Implementation Review (PDF)](submissions/milestone-2/Milestone2_ClientImplementationReview_Maluleke_48277444.pdf)** — the full implementation report with all testing and troubleshooting evidence. Submitted with [`tshimo-agri-hq-milestone-2.pkt`](packet-tracer/tshimo-agri-hq-milestone-2.pkt).
- **[Final Submission — Technical Report (PDF)](submissions/final/Final_TechnicalReport_Maluleke_48277444.pdf)** — design rationale, implementation, testing, the assigned fault, growth (CR1 and the branch proposal), issues, limitations and reflection. Submitted with [`Final_TshimoAgriHQ_Maluleke_48277444.pkt`](submissions/final/Final_TshimoAgriHQ_Maluleke_48277444.pkt), which is the same repaired network as the Milestone 2 file.
