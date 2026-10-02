# Project Status and Milestone 2 Control Record

**Project:** Tshimo Agri Supplies network design  
**Module:** CMPG 325 Computer Networks  
**Student:** Kurhula Success Maluleke (48277444)  
**Project ID:** CMPG325-2026-042  
**Record date:** 23 September 2026  
**Milestone 2 due:** 2 October 2026

This document is the working control record for the transition from Milestone 1 design to Milestone 2 implementation. It records what is confirmed, what is proposed, what is still unknown, and what evidence must exist before submission.

## 1. Current verdict

**Status (2 Oct 2026): Milestone 2 implemented, tested and packaged.**

The configured network is saved at `packet-tracer/tshimo-agri-hq-milestone-2.pkt` (repaired, healthy state), with the unconfigured build kept at `packet-tracer/tshimo-agri-hq-baseline.pkt`. All 12 functional tests pass (`docs/10-test-results.md`), the assigned fault has been demonstrated end to end (`troubleshooting/advanced-fault-isolation.md`), and the submission package is in `submissions/milestone-2/`. Still to do: the actual eFundi upload (by Kurhula) and the Final submission work due 16 Oct 2026.

## 2. Confirmed project requirements

- Assigned address block: `10.23.0.0/16`.
- Client: Tshimo Agri Supplies, Potchefstroom.
- Assigned technical challenge: advanced network troubleshooting and fault isolation.
- HQ must support a possible branch office within 18 months without renumbering HQ.
- Change Request 1 adds eight Warehouse/Logistics staff without redesigning the network.
- Milestone 2 requires a working Packet Tracer file, implementation of the assigned feature, testing evidence, and an updated GitHub portfolio.
- All rubric components are compulsory. The rubric weights Packet Tracer implementation at 35%, the assigned feature at 20%, the GitHub portfolio at 15%, design at 15%, and the video/technical defence at 15%.
- Final submission also requires the Packet Tracer project, GitHub evidence portfolio, technical report, and a 15-20 minute inset video demonstration.

## 3. Confirmed design baseline

### 3.1 Devices and topology

`R1` is the edge router and performs router-on-a-stick inter-VLAN routing and NAT/PAT. `SW-CORE` distributes trunks to one access switch per department: `SW-ADM`, `SW-SAL`, `SW-WH`, and `SW-SRV`.

The documented interface plan is:

| Device | Interface | Connection | Mode |
|---|---|---|---|
| R1 | Gi0/0 | SW-CORE | 802.1Q trunk through subinterfaces |
| R1 | Gi0/1 | ISP | Routed WAN link |
| SW-CORE | Gi0/1 | R1 | Trunk |
| SW-CORE | Gi0/2 | SW-ADM | Trunk |
| SW-CORE | Gi0/3 | SW-SAL | Trunk |
| SW-CORE | Gi0/4 | SW-WH | Trunk |
| SW-CORE | Gi0/5 | SW-SRV | Trunk |
| Access switches | Fa0/1 | SW-CORE | Trunk |
| Access switches | Fa0/2 onward | End devices | Access ports |

### 3.2 VLAN and addressing baseline

| VLAN | Name | Network | Gateway | Role |
|---:|---|---|---|---|
| 10 | ADMIN | `10.23.0.0/26` | `10.23.0.1` | Admin/Management users |
| 20 | SALES | `10.23.0.64/26` | `10.23.0.65` | Sales users |
| 30 | WAREHOUSE | `10.23.0.128/26` | `10.23.0.129` | Warehouse/Logistics, including CR1 |
| 40 | SERVERS | `10.23.0.192/26` | `10.23.0.193` | DHCP, DNS, file/ERP services |
| 99 | MGMT-NATIVE | `10.23.1.16/28` | `10.23.1.17` | Switch management and trunk native VLAN |

Additional reservations:

- R1-ISP WAN: `10.23.1.0/30`.
- Future guest Wi-Fi: `10.23.1.32/28`.
- Future branch office: `10.23.128.0/17`, untouched.
- HQ summary block: `10.23.0.0/23`.

### 3.3 CR1 outcome

Warehouse/Logistics grows from 12 to 20 users. The existing VLAN 30 `/26` provides 62 usable addresses, so the address boundary, gateway, routing, and trunks remain unchanged. The implementation must show the additional Warehouse access ports and/or the resulting 20-device capacity.

## 4. Milestone 1 evidence already present

The following are present under `submissions/milestone-1/`:

- Milestone 1 PDF.
- Milestone 1 HTML source.
- Milestone 1 DOCX source.
- eFundi submission text.

The following design documents are present under `docs/`:

- `01-client-requirements.md`
- `02-physical-topology.md`
- `03-logical-topology.md`
- `04-ip-addressing-plan.md`
- `05-change-requests.md`
- Topology and summary images under `docs/assets/`

Milestone 1 submission to eFundi is not confirmed by repository evidence and must be verified separately.

## 5. Decisions locked 25 September 2026

Confirmed by Kurhula in session on 25 Sep 2026. These are approved for config-drafting purposes; re-confirm only if the actual Packet Tracer device palette forces a change.

| Item | Approved value | Confirmed by | Date |
|---|---|---|---|
| Core switch/port plan | Cisco 2960-24TT; SW-CORE Gi0/1 to R1, Fa0/1-Fa0/4 to the four access switches (supersedes the original Gi0/1-Gi0/5 plan, which is infeasible on a standard 2960-24TT) | Kurhula | 25 Sep 2026 |
| DHCP architecture | Dedicated Server-PT DHCP server in VLAN 40, R1 `ip helper-address` relay on VLAN 10/20/30 subinterfaces | Kurhula | 25 Sep 2026 |
| Server addresses | DHCP-SRV `10.23.0.194/26`, DNS-SRV `10.23.0.195/26`, FILE-SRV `10.23.0.196/26`, gateway `10.23.0.193` | Kurhula | 25 Sep 2026 |
| Router model | Cisco 2911 (Gi0/0, Gi0/1) | Kurhula | 25 Sep 2026 |
| ISP model and addressing | R1 WAN `10.23.1.1/30`; ISP WAN `10.23.1.2/30` (R1 default route next hop); ISP test target `203.0.113.1/32` | Kurhula | 25 Sep 2026 |
| Troubleshooting fault | Remove VLAN 30 from the SW-CORE-to-SW-WH trunk allowed-VLAN list (SW-CORE Fa0/3) | Kurhula | 25 Sep 2026 |
| Guideline set | More lecture notes may still be uploaded | Open | — |
| Submission packaging | Milestone 2 folder does not exist yet | Create after testing | — |

### Approved implementation

- Dedicated DHCP server in VLAN 40, statically addressed `10.23.0.194/26`.
- DNS and file/ERP servers at `10.23.0.195/26` and `10.23.0.196/26`.
- R1 uses `ip helper-address 10.23.0.194` on VLAN 10, 20, and 30 subinterfaces.
- VLAN 99 is the native VLAN on every trunk.
- The controlled troubleshooting fault is an incorrect allowed-VLAN list on SW-CORE Fa0/3 (VLAN 30 removed).

Not yet locked: exact eFundi M2 submission instructions, and whether a report/video is required at M2 in addition to the four diagram items (diagram indicates no).

## 6. Milestone 2 build checklist

- [ ] Confirm the unresolved decisions in Section 5.
- [ ] Build all documented devices and links in Packet Tracer.
- [ ] Configure VLANs and names on all switches.
- [ ] Configure trunks, native VLAN 99, and allowed VLANs.
- [ ] Configure access ports for Admin, Sales, Warehouse, and Servers.
- [ ] Configure R1 subinterfaces and gateway addresses.
- [ ] Configure the WAN link, default route, and NAT/PAT.
- [ ] Configure DHCP scopes and relay behavior.
- [ ] Configure DNS and file/ERP server services if required by the brief.
- [ ] Add the CR1 Warehouse devices or demonstrate the 20-user port plan.
- [ ] Save the working `.pkt` file under `packet-tracer/`.
- [ ] Export final device configurations into `configs/`.
- [ ] Test same-VLAN connectivity.
- [ ] Test inter-VLAN routing.
- [ ] Test DHCP on every user VLAN.
- [ ] Test DNS and internal services.
- [ ] Test NAT/Internet reachability if the ISP simulation is included.
- [ ] Capture configuration and connectivity screenshots.
- [ ] Run the selected fault scenario, capture the failure, isolation, fix, and verification.
- [ ] Create the Milestone 2 evidence and submission package.
- [ ] Update the README and commit history with meaningful project work.

## 7. Required evidence inventory

At minimum, the portfolio should contain evidence of:

- Physical Packet Tracer topology.
- R1 `show ip interface brief`.
- R1 `show ip route` and NAT status.
- Switch `show vlan brief`.
- Switch `show interfaces trunk`.
- DHCP leases or client address acquisition in each user VLAN.
- Successful same-VLAN and inter-VLAN pings.
- DNS/service verification.
- CR1 Warehouse capacity.
- Troubleshooting fault before correction.
- OSI-layer isolation process and relevant `show`, `ping`, and `tracert`/`traceroute` results.
- Corrected configuration and successful verification.

Screenshots must be named clearly and placed under `screenshots/`. Explanatory notes should identify the device, command or test, expected result, actual result, and conclusion.

## 8. Guideline material status

The `guidelines/` folder currently contains selected CMPG 325 lecture decks and the project overview/rubric images. It is **not complete**; further lecture notes may be uploaded later.

**Text audit, 28 Sep 2026:** extracted and reviewed the slide text of the four `.pptx` decks (Chapters 1, 3, 4, 5 — Chapter 2's three `.ppt` files are an older binary format and were not extracted; titles suggest physical-layer signal/media/connection theory, consistent with the pattern below but not directly confirmed). Findings:

- Chapters 3, 4, and 5 are pure course theory (data-link/LAN framing, MAC sublayer protocols — ALOHA, CSMA/CD, CSMA/CA, taking-turns protocols — and network-layer IP binary/decimal conversion drills). No project-specific requirements, no submission instructions, nothing referencing this semester project.
- Chapter 1 contains one graded item, "Unit 1 — Lab/AI task": a separate weekly lab (Jupyter Notebook OSI/TCP-IP visualizer, Python, 5-10 min video), due **3 August 2026**. This is **not** part of CMPG325-2026-042 and is already past due independent of this project.

Conclusion: nothing in the four decks checked changes or adds to the confirmed project requirements in this document. The three Chapter 2 `.ppt` files remain unchecked; if a fuller handbook or eFundi brief surfaces later, re-verify against it rather than these lecture decks, which are general course theory, not a project spec.

The current guideline set confirms the relevance of:

- OSI and TCP/IP layers.
- Hosts, switches, routers, gateways, servers, LANs, and WANs.
- Data-link and MAC-layer operation.
- IP addressing and the network layer.
- Testing, troubleshooting, documentation, and technical defence.

New guideline material must be reviewed for requirements that affect configuration, testing, documentation, or the final video. Existing design decisions should not be changed merely because a lecture deck is incomplete or generic; changes require a confirmed project requirement or a demonstrated technical error.

## 8.1 Platform responsibility rule

This division applies to Milestone 2 and all later phases:

- **Cisco Packet Tracer:** actual placement/cabling, IOS command execution, DHCP/server setup, simulation, testing, fault injection, repair, and authentic screenshots are performed and observed in Packet Tracer by Kurhula.
- **Claude Code:** prepares the physical/logical plan, drafts and explains IOS command blocks, test cases, OSI troubleshooting steps, documentation, evidence naming, and submission mapping. Generated commands are not evidence that they have been executed.
- **eFundi:** the actual Milestone 2 submission instructions must be checked when available; map the requested items to the files that truly exist before upload.
- **OBS or Windows Game Bar:** record the final 15-20 minute webcam-inset walkthrough; Kurhula records and uploads it through eFundi.

See `docs/08-milestone-2-execution-map.md` for the phase-by-phase responsibility and evidence matrix.

## 9. Repository state at record creation

- Git branch: `master`.
- Remote: GitHub repository `MALULEKE-KS/tshimo-agri-network`.
- Existing committed work is the Milestone 1 design package and supporting documentation.
- `configs/`, `packet-tracer/`, `screenshots/`, and `troubleshooting/` contain placeholders only.
- The newly added `guidelines/` folder and copied rubric/overview images are currently untracked and must be intentionally included or excluded in a future commit.
- No Packet Tracer file or implementation evidence has been verified yet.

## 10. Change log

| Date | Change |
|---|---|
| 23 Sep 2026 | Audited repository and Milestone 1 design against newly added project overview and marking rubric. |
| 23 Sep 2026 | Recorded that the guideline folder is incomplete and may receive more lecture notes. |
| 23 Sep 2026 | Identified DHCP architecture, server IPs, exact router model, ISP details, fault scenario, and Milestone 2 packaging as unresolved locks. |
| 23 Sep 2026 | Created this control record as the handoff point for implementation. |
| 23 Sep 2026 | Created `CLAUDE_CODE_HANDOFF.md` for terminal-based implementation handoff. |
| 23 Sep 2026 | Created `docs/07-physical-build-design.md` as the Packet Tracer physical build source of truth. |
| 25 Sep 2026 | Created `docs/08-milestone-2-execution-map.md`, mapping the supplied M2 diagram, rubric, build sequence, tests, evidence, and final deliverables. |
| 25 Sep 2026 | Clarified cross-phase platform responsibilities: Packet Tracer executes the network work, Claude Code prepares/documentation, eFundi instructions govern hand-in, and OBS/Windows Game Bar records the final video. |
| 25 Sep 2026 | Kurhula confirmed all six pending Section 5 decisions: core port plan (2960-24TT, Fa0/1-Fa0/4), router model (2911), DHCP architecture (dedicated server + relay), server addresses, ISP/WAN addressing, and troubleshooting fault (VLAN 30 removed from SW-CORE Fa0/3 allowed list). |
| 25 Sep 2026 | Corrected the stale Gi0/1-Gi0/5 core port references in `docs/07-physical-build-design.md` and `CLAUDE_CODE_HANDOFF.md` to match the approved Fa0/1-Fa0/4 plan. |
| 28 Sep 2026 | Extracted and audited slide text from the four `.pptx` lecture decks; confirmed no hidden project-specific requirements (pure course theory plus one unrelated past-due weekly lab). Chapter 2's `.ppt` files remain unchecked. |
| 28 Sep 2026 | Drafted switch configs `configs/sw-core.txt`, `sw-adm.txt`, `sw-sal.txt`, `sw-wh.txt`, `sw-srv.txt`. Not yet applied. |
| 2 Oct 2026 | Kurhula built the physical topology in Packet Tracer 9.0.1 and saved `packet-tracer/tshimo-agri-hq-baseline.pkt` plus `screenshots/01-physical-topology.png`. Screenshot shows all 7 network devices and 43 endpoints in the planned layout; exact port numbers not yet verified (to be confirmed with `show cdp neighbors` on SW-CORE). A second file, `tshimo-agri-hq-baseline.pka`, was also saved at repo root — purpose unconfirmed. |
| 2 Oct 2026 | Drafted `configs/r1.txt`, `configs/isp.txt`, and `configs/servers.md`. Not yet applied. |
| 2 Oct 2026 | Gate B (physical build) passed. Clean topology screenshot retaken; port-label screenshot confirms ISP Gi0/0 ↔ R1 Gi0/1, R1 Gi0/0 to SW-CORE, SW-CORE Fa0/1–Fa0/4 to the access switches' Fa0/1, and server ports Fa0/2–Fa0/4. SW-CORE's Gi0/1 label and per-PC port order aren't legible at screenshot zoom; SW-CORE's end to be confirmed with `show cdp neighbors` in Phase C. Kurhula confirmed the root-level `.pka` file is not needed. |
| 2 Oct 2026 | Recabled the 5 like-device links (ISP–R1, SW-CORE to each access switch) from straight-through to crossover, after a rubric check against "correct cabling". Baseline `.pkt` resaved; both screenshots retaken. Open: the ISP–R1 dash pattern and the ISP-side port (Gi0/0 vs Gi0/1) aren't legible in the screenshots; confirm both before Phase C. |
| 2 Oct 2026 | Phase C–E completed in Packet Tracer: all five switch configs, R1 and ISP applied; servers and DHCP set up; 12/12 functional tests passed (`docs/10-test-results.md`); assigned fault injected, isolated, fixed and verified (`troubleshooting/advanced-fault-isolation.md`). ISP port resolved as Gi0/0 (NAT ping to the ISP loopback succeeded with `isp.txt` applied unchanged). During the fault demo `sw-wh.txt` was pasted onto SW-CORE by mistake; detected via CDP and trunk checks, corrected by re-applying `sw-core.txt`, and the fault demo re-run cleanly. Final repaired network saved as `packet-tracer/tshimo-agri-hq-milestone-2.pkt`. |
| 2 Oct 2026 | Built the Milestone 2 submission package in `submissions/milestone-2/`: report (PDF + HTML source, with cropped evidence figures and all device configs as an appendix) and a frozen copy of the final `.pkt`. Lecturer's stated hand-in: full documentation with all testing evidence, plus the Packet Tracer file. |
