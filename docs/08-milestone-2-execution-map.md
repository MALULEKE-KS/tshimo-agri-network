# Milestone 2 Execution Map

**Project:** Tshimo Agri Supplies network design  
**Module:** CMPG 325 Computer Networks  
**Student:** Kurhula Success Maluleke (48277444)  
**Project ID:** CMPG325-2026-042  
**Map created:** 25 September 2026  
**Milestone 2:** Client Implementation Review, due 2 October 2026

This is the source-linked work map for Milestone 2. It separates what the supplied milestone overview explicitly says to submit, what the project rubric assesses across the project, and what remains proposed or unknown. It is designed to keep implementation, testing, evidence, and submission aligned.

## 1. Source basis and limits

Reviewed sources in this workspace:

- `docs/CMPG 325_PROJECT IN ONE DIAGRAM.png`
- `docs/CMPG 325_PROJECT MARKING RUBRIC.png`
- Local project notes and handoff files (kept out of the repository)
- `README.md`
- `docs/01-client-requirements.md` through `docs/07-physical-build-design.md`
- Existing Milestone 1 files under `submissions/milestone-1/`

The supplied project diagram explicitly gives the Milestone 2 review date and these Milestone 2 review items: working Packet Tracer file, assigned feature implemented, testing evidence, and updated GitHub portfolio.

The marking rubric describes five weighted assessment dimensions and says all five components must be submitted. The supplied project diagram places the 15-20 minute inset video, technical report, and final Packet Tracer/GitHub package under Final Submission on 16 October 2026. Therefore:

- Treat the four items listed under Milestone 2 in the diagram as the minimum explicit Milestone 2 package.
- Build the documentation and evidence during Milestone 2 so they carry forward into the final report and video.
- Confirm against the full project handbook/eFundi instructions whether a technical report or video is also required at Milestone 2. That full handbook is not present in this workspace, so this map cannot verify additional submission rules.
- More lecture notes may be uploaded. The current `guidelines/` set is incomplete and must be rechecked before final submission.

## 1.1 Platform responsibilities across all phases

These tool boundaries apply throughout Milestone 2 and Final Submission. Do not treat a written plan, generated IOS text, or a diagram as proof that the network was actually built or tested.

| Phase | Platform | What happens there | Evidence/hand-off |
|---|---|---|---|
| Physical network build | Cisco Packet Tracer | Kurhula places R1, SW-CORE, four access switches, three servers, and department PCs to match the physical design, then cables them. Use straight-through copper between unlike devices (router↔switch, switch↔endpoint) and crossover between like devices (ISP↔R1, SW-CORE↔access switches), per the cable schedule in `docs/07-physical-build-design.md`; configure trunking in IOS, since a cable itself does not create a trunk. | Save the `.pkt` file and capture the complete topology in `screenshots/`. |
| IOS configuration authoring | Claude Code | Draft, review, and explain per-device IOS command blocks: VLANs and access ports, 802.1Q trunks, router subinterfaces, WAN/default route, NAT/PAT, and DHCP pools or relay as approved. Keep one readable config per device under `configs/`. | Commands are proposed text until applied successfully in Packet Tracer. |
| IOS configuration execution | Cisco Packet Tracer | Kurhula pastes each reviewed command block into the matching device CLI, observes IOS responses, resolves device/model syntax differences, and saves the Packet Tracer project. | Record any deviations in the config files and capture relevant CLI output. |
| Functional testing | Cisco Packet Tracer | Test DHCP leases, same-VLAN pings, ping between VLANs, ISP reachability, `tracert`/`traceroute` where supported, routing tables, trunks, VLANs, and NAT translations. Tests must be run in the simulation, not inferred from config text. | Save actual outcomes and screenshots in `screenshots/` and the test log. |
| Troubleshooting design | Claude Code | Select the fault, state the expected symptoms as hypotheses, map OSI-layer isolation, choose diagnostic commands, and document the minimal fix and retest plan. | Keep the planned scenario in `troubleshooting/`; do not label a simulated result before Packet Tracer confirms it. |
| Fault injection and recovery | Cisco Packet Tracer | Kurhula injects exactly the selected fault, observes the real symptoms, isolates it with ping/traceroute/show commands, fixes it, and verifies recovery by repeating the failed tests. | Capture before, isolation, fix, and after evidence in `screenshots/` and document actual results in `troubleshooting/`. Keep the primary submitted `.pkt` repaired. |
| Milestone 2 submission | eFundi | When the M2 assignment opens, Kurhula provides its actual instructions. Map those instructions against the artifacts that exist at that time; submit exactly what eFundi requests by 2 October 2026. | Update this map/checklist with eFundi-specific requirements. Do not assume this local rubric image is the complete submission instruction. |
| Final demonstration | Screen recorder (OBS or Windows Game Bar) and eFundi | Record the required 15-20 minute webcam-inset walkthrough, covering requirements, design, Packet Tracer demonstration, fault isolation, and reflection; submit with the final `.pkt`, GitHub portfolio, docs/report, and any required files by 16 October 2026. | Keep a video outline and verified demo path in the repo; recording and upload are performed by Kurhula. Re-check eFundi instructions before upload. |

Claude Code cannot operate the Packet Tracer GUI, paste commands into device consoles, observe simulation output, create authentic screenshots from the simulation, or submit work to eFundi. It can prepare the design, command text, test procedure, documentation, and evidence checklist; Kurhula performs and verifies the GUI/simulation and submission steps.

## 2. Exact Milestone 2 submission package

The supplied project diagram labels Milestone 2 as **Client Implementation Review**, due **2 October 2026**. Prepare the following four items:

| Required item from project diagram | What to submit/show | Repository location or evidence |
|---|---|---|
| Working Packet Tracer file | Corrected, saved, opens without errors, complete HQ topology and configurations | `packet-tracer/tshimo-agri-hq-milestone-2.pkt` |
| Assigned feature implemented | Advanced network troubleshooting scenario implemented and demonstrated, not just described | `troubleshooting/` scenario record; controlled fault evidence; corrected final `.pkt` |
| Testing evidence | Tests show required network services and recovery work, with actual results | `screenshots/` plus `docs/08` test record or a dedicated test-results document |
| Updated GitHub portfolio | Organized documentation, per-device configs, Packet Tracer file, evidence, and meaningful development history | Repository README and committed project files |

Do not submit a deliberately broken final Packet Tracer file. Demonstrate the fault using a transient fault-and-repair sequence or a separately named fault-reproduction copy; leave the main submitted `.pkt` in the fully corrected state.

## 3. Rubric-to-work map

The rubric is for the overall project and assigns these weights. The work below should target its Excellent column (90-100%), while the Milestone 2 diagram remains the authority for the explicit M2 hand-in items.

| Rubric criterion | Weight | Excellent-level rubric intent | Milestone 2 work that supports it | Proof/evidence |
|---|---:|---|---|---|
| Client requirements and network design | 15% | Exceptional requirements understanding; complete appropriate physical/logical topology; efficient, correct, fully documented addressing; justified decisions | Preserve the Milestone 1 design; implement exactly that topology and address plan; explain VLAN separation, `/26` growth capacity, CR1, and branch reservation. Record any approved change and reason. | Design docs, labelled physical topology, VLAN/addressing table, working Packet Tracer file, brief design explanation in GitHub |
| Packet Tracer implementation and functionality | 35% | Correct device choices, cabling, services, stable network, no major errors | Build the complete HQ; configure VLANs, trunks, router subinterfaces, DHCP/DNS/internal services, WAN/default route, and NAT/PAT as selected and supported. | `.pkt`, config files, topology screenshot, show-command outputs, passing tests |
| Assigned networking feature / technical challenge | 20% | Feature correctly implemented and explained; demonstration shows deep understanding; verification complete and accurate | Create and troubleshoot a controlled Layer 2 fault: remove VLAN 30 from the SW-CORE-to-SW-WH trunk allowed list, causing Warehouse routed services to fail while local VLAN switching remains. Isolate, fix, and verify. | `troubleshooting/advanced-fault-isolation.md`, before/fix/after captures, show command outputs, passing retests |
| GitHub portfolio of evidence | 15% | Professional repository, comprehensive evidence, frequent meaningful commits, excellent README/diagrams/reports | Keep source docs, configs, `.pkt`, tests, fault evidence, and status accurate and easy to navigate. Commit planning/configuration/testing work in logical units; do not claim unperformed work. | README, committed file tree, diff/history, named evidence, updated project status |
| Video demonstration and technical defence | 15% | Clear, professional 15-20 minute inset presentation; excellent explanation and confident answers | Milestone 2 diagram places this under Final. Collect a demonstration outline, command outputs, screenshots, and decision rationale during M2; record the final video for 16 October unless the handbook explicitly requires an M2 video. | Final 15-20 minute inset video; technical report and project demo, per final deliverables |

The rubric says every component is compulsory and a missing component earns 0% for that component. The final project overview lists: Packet Tracer project, GitHub portfolio of evidence, technical report, 15-20 minute inset video, and any additional required files.

## 4. Design baseline to implement

### 4.1 HQ topology and endpoint counts

Build the HQ only. Do not add the future branch office; reserve its space in the documentation only.

- ISP router -> R1 -> SW-CORE -> four access switches.
- Access switches: `SW-ADM`, `SW-SAL`, `SW-WH`, `SW-SRV`.
- User endpoints: 8 Admin PCs, 12 Sales PCs, 20 Warehouse PCs (12 existing plus 8 CR1).
- Servers: DHCP, DNS, and File/ERP Server-PT devices.
- Total physical links: 6 network uplinks plus 43 endpoint links = 49.
- Port schedule and device labels: `docs/07-physical-build-design.md`.

### 4.2 VLANs and IP addressing

| VLAN | Name | Subnet | R1 gateway | Intended endpoints |
|---:|---|---|---|---|
| 10 | ADMIN | `10.23.0.0/26` | `10.23.0.1` | 8 Admin PCs |
| 20 | SALES | `10.23.0.64/26` | `10.23.0.65` | 12 Sales PCs |
| 30 | WAREHOUSE | `10.23.0.128/26` | `10.23.0.129` | 20 Warehouse PCs including CR1 |
| 40 | SERVERS | `10.23.0.192/26` | `10.23.0.193` | 3 servers |
| 99 | MGMT-NATIVE | `10.23.1.16/28` | `10.23.1.17` | Switch management; native on all trunks |

Other documented allocations: R1-ISP `/30` at `10.23.1.0/30`; future guest reservation `10.23.1.32/28`; future branch reservation `10.23.128.0/17`; HQ summary `10.23.0.0/23`.

### 4.3 Physical device/port map correction

**Physical feasibility check:** A common Packet Tracer Catalyst 2960-24TT has 24 FastEthernet ports and only two Gigabit uplinks. The earlier draft assigned five Gigabit interfaces (Gi0/1 through Gi0/5) to SW-CORE, which is not available on that common model. Use the following feasible core map unless a different core switch model is deliberately selected:

| Link | Endpoint A | Endpoint B | Mode |
|---|---|---|---|
| Router uplink | R1 Gi0/0 | SW-CORE Gi0/1 | 802.1Q trunk |
| Admin uplink | SW-CORE Fa0/1 | SW-ADM Fa0/1 | Trunk |
| Sales uplink | SW-CORE Fa0/2 | SW-SAL Fa0/1 | Trunk |
| Warehouse uplink | SW-CORE Fa0/3 | SW-WH Fa0/1 | Trunk |
| Server uplink | SW-CORE Fa0/4 | SW-SRV Fa0/1 | Trunk |
| WAN | R1 Gi0/1 | ISP router Ethernet/Gigabit interface | Routed `/30` |

The four access switches retain endpoint assignments `Fa0/2-Fa0/9`, `Fa0/2-Fa0/13`, `Fa0/2-Fa0/21`, and `Fa0/2-Fa0/4` respectively. This change only affects physical core port selection; it does not change VLAN IDs, subnets, gateways, or the router-on-a-stick model.

Before building, confirm the exact Packet Tracer models and displayed interface names. If the selected 2960 variant exposes a different port layout, update `docs/07-physical-build-design.md` and this table before cabling.

## 5. Working service plan, pending sign-off

Some implementation details are not in the confirmed brief. The following set is a coherent Packet Tracer proposal that avoids ambiguity and makes all M2 tests possible. Mark it approved in `docs/06-project-status-and-milestone-2-control.md` after Kurhula confirms the device/service choices.

| Item | Working proposal | Rationale/test use |
|---|---|---|
| R1 model | Cisco 2911 (or a selected PT router with Gi0/0 and Gi0/1) | Matches the documented interface plan and supports router subinterfaces |
| Core/access switches | Cisco 2960-24TT | 24 access ports; core uses Gi0/1 to R1 and Fa0/1-Fa0/4 for access-switch trunks |
| R1 WAN | `10.23.1.1/30` | First usable address in the documented point-to-point subnet |
| ISP WAN | `10.23.1.2/30` | Second usable address; R1 default route next hop |
| ISP test target | ISP router loopback `203.0.113.1/32` | Stable simulated external ping/DNS target for NAT verification |
| DHCP server | `10.23.0.194/26`, gateway `10.23.0.193`, DNS `10.23.0.195` | Fixed server address within VLAN 40 |
| DNS server | `10.23.0.195/26`, gateway `10.23.0.193` | DNS service for a test hostname such as `files.tshimo.test` |
| File/ERP server | `10.23.0.196/26`, gateway `10.23.0.193` | Enable a demonstrable Packet Tracer server service (e.g., FTP if supported) |
| DHCP location | Dedicated Server-PT DHCP service | Matches Milestone 1 report; relay user VLAN broadcast requests with `ip helper-address 10.23.0.194` on R1 VLAN 10/20/30 subinterfaces |
| DHCP pools | VLANs 10, 20, 30; `/26`; respective R1 gateway; DNS `10.23.0.195` | Each client VLAN receives correct address, gateway, and DNS |
| VLAN 99 switches | Static management IPs in `10.23.1.18-10.23.1.22/28`; default gateway `10.23.1.17` | Keeps native/management VLAN off DHCP and distinct per switch |
| Troubleshooting fault | Remove VLAN 30 from SW-CORE Fa0/3 allowed list | Warehouse endpoints can still communicate locally but fail to route to VLAN 40/other VLANs; easy to isolate with `show interfaces trunk` |

Notes:

- These values are not supplied verbatim by the rubric. They are recommended lab choices and require student approval before configs are treated as final.
- The `10.23.1.0/30` WAN subnet sits inside the documented `10.23.0.0/23` HQ summary. Keep the WAN link out of NAT inside-prefix ACLs; do not advertise a summary route in the PT ISP unless the routing demonstration specifically needs it.
- Use DHCP exclusions/reservations so clients do not lease gateway or infrastructure addresses.
- The server-address table and actual services must match what is configured in the final `.pkt` and described in the test evidence.

## 6. Build and configuration work order

### Phase A: confirm and freeze the lab decisions (25 September)

- [ ] Read any newly uploaded rubric/course brief/lecture files.
- [ ] Confirm router and switch models in Packet Tracer.
- [ ] Confirm the working service plan in Section 5 or amend it.
- [ ] Confirm DHCP server, DNS, file/ERP behavior, and chosen troubleshooting fault.
- [ ] Record approved decisions in the control record before generating final commands.

**Gate A:** no unknown interface names, WAN endpoint addresses, DHCP server address, or selected fault remain.

### Phase B: build and save physical topology (25-26 September)

- [ ] Create the 7 network devices and 43 endpoints listed in `docs/07-physical-build-design.md`.
- [ ] Use the corrected 2960-compatible core port map in Section 4.3.
- [ ] Label all devices consistently with the physical design.
- [ ] Verify all 49 links and endpoint port ranges.
- [ ] Save an unconfigured baseline as `packet-tracer/tshimo-agri-hq-baseline.pkt`.
- [ ] Capture `screenshots/01-physical-topology.png`.

**Gate B:** all physical links are correctly placed, the topology is readable, and the baseline file opens.

### Phase C: configure and capture baseline (26-29 September)

1. **Switch Layer 2 configuration**
   - Create VLANs 10, 20, 30, 40, 99 with consistent names.
   - Configure SW-CORE trunk ports Gi0/1 and Fa0/1-Fa0/4.
   - Configure access-switch Fa0/1 uplinks as trunks.
   - Set native VLAN 99 and explicitly allow VLANs `10,20,30,40,99` on all trunks.
   - Configure endpoint ports as access ports in their department VLAN.
   - Configure switch management SVIs and default gateways if remote management is part of the model.
   - Save reviewed per-device configs under `configs/sw-core.txt`, `configs/sw-adm.txt`, `configs/sw-sal.txt`, `configs/sw-wh.txt`, and `configs/sw-srv.txt`.

2. **Router configuration**
   - Bring up Gi0/0 and Gi0/1.
   - Configure 802.1Q subinterfaces for VLANs 10, 20, 30, 40, and 99 with the exact gateways/masks.
   - Configure WAN `/30` address and default route to the approved ISP next hop.
   - Configure DHCP relay on user VLAN subinterfaces if dedicated DHCP is approved.
   - Configure NAT/PAT only after outside reachability works; mark VLAN subinterfaces as NAT inside and Gi0/1 as NAT outside.
   - Save reviewed config as `configs/r1.txt`.

3. **Servers and clients**
   - Set static server addresses and gateways.
   - Configure the approved DHCP scopes and DNS option for VLANs 10, 20, and 30.
   - Configure DNS record(s) and the selected file/ERP demonstration service.
   - Set user endpoints to DHCP and label endpoints according to their department/port.

4. **Save checkpoints**
   - Save a copy after Layer 2 is operational and another after routing/services work.
   - Keep `packet-tracer/tshimo-agri-hq-milestone-2.pkt` as the final corrected submission copy.

**Gate C:** VLANs/trunks are correct, gateways respond, clients receive proper DHCP parameters, and routing/services work before introducing any fault.

### Phase D: functional testing (29-30 September)

Run each test from an endpoint or device CLI. Record source, destination, expected result, actual result, pass/fail, and screenshot path in a test log.

| Test ID | Test | Pass condition | Evidence |
|---|---|---|---|
| PHY-01 | Inspect full topology and link state | Every scheduled cable exists; links come up after configuration | `screenshots/01-physical-topology.png` |
| L2-01 | `show vlan brief` on every switch | VLANs 10/20/30/40/99 exist; endpoint ports assigned to correct VLAN | `screenshots/02-vlans.png` |
| L2-02 | `show interfaces trunk` on core/access switches | Trunks up; native VLAN 99; allowed VLANs include 10,20,30,40,99 | `screenshots/03-trunks.png` |
| L3-01 | `show ip interface brief` on R1 | All five subinterfaces and WAN interface are up/up with expected IPs | `screenshots/04-r1-interfaces.png` |
| L3-02 | `show ip route` on R1 | All connected VLAN/WAN routes appear; default route points to ISP | `screenshots/05-r1-routes.png` |
| DHCP-01 | Renew/check DHCP on one PC per user VLAN | Correct `/26`, gateway, DNS, and address range per VLAN | `screenshots/06-dhcp-clients.png` |
| LAN-01 | Ping between two clients in same VLAN | Successful replies | `screenshots/07-same-vlan-ping.png` |
| ROUTE-01 | Ping from Admin/Sales/Warehouse to VLAN 40 gateway/server | Successful replies through R1 | `screenshots/08-inter-vlan-ping.png` |
| DNS-01 | Resolve approved internal test hostname | Correct server IP returned | `screenshots/09-dns-test.png` |
| SVC-01 | Access approved FTP/HTTP/file service | Client reaches service and completes a basic operation | `screenshots/10-service-test.png` |
| NAT-01 | Ping ISP loopback and inspect `show ip nat translations` | External test target reachable; inside source is translated to WAN address | `screenshots/11-nat-test.png` |
| CR1-01 | Inspect Warehouse port allocation and 20 clients | WH-PC-01 through WH-PC-20 fit on VLAN 30 without subnet change | `screenshots/12-cr1-capacity.png` |

If a service cannot be supported by the selected Packet Tracer server/device, record the limitation and the approved substitute; do not mark a test as passed without actually running it.

### Phase E: implement and demonstrate assigned troubleshooting feature (30 September-1 October)

Recommended reproducible fault: remove VLAN 30 from SW-CORE Fa0/3's allowed VLAN list while keeping VLAN 10/20/40/99 allowed.

Expected controlled demonstration:

1. **Healthy baseline:** Warehouse PC has a valid VLAN 30 DHCP lease; can reach gateway `10.23.0.129`, Server VLAN, and selected services.
2. **Inject fault:** remove VLAN 30 from the SW-CORE-to-SW-WH trunk allowed list. Do not corrupt the final saved submission copy.
3. **Observe symptoms:** Warehouse same-VLAN communication may still work, but routed access to the gateway/services fails. State exactly what the simulation shows; do not presume symptoms before observing them.
4. **Isolate layer by layer:**
   - Physical: inspect cable/link LEDs and `show interfaces status`.
   - Data link: check VLAN membership, trunk status, native/allowed VLANs using `show vlan brief`, `show interfaces trunk`, and relevant interface configuration.
   - Network: test client address/mask/gateway, then ping local gateway, server IP, and external test IP; use `tracert`/`traceroute` if supported.
   - Service: only investigate DHCP/DNS after Layer 1-3 evidence points there.
5. **Identify cause:** demonstrate VLAN 30 absent from SW-CORE Fa0/3 allowed list.
6. **Correct:** restore VLAN 30 to the trunk's allowed VLAN set; save the corrected running configuration.
7. **Verify:** repeat the exact failed pings/service test, check trunk output, and capture successful recovery.
8. **Restore clean deliverable:** ensure the primary `.pkt` is the corrected healthy network; save the fault-injection reproduction separately only if useful.

Store the explanation at `troubleshooting/advanced-fault-isolation.md`; evidence filenames should make before/fix/after order obvious.

**Gate E:** the fault is reproducible, the learner isolates it from evidence rather than guessing, fix is minimal, and all relevant tests pass again.

### Phase F: GitHub portfolio and submission handoff (1-2 October)

- [ ] Update README with Milestone 2 status and direct links to the `.pkt`, config set, tests, and troubleshooting record.
- [ ] Ensure the portfolio includes physical/logical design, address plan, build configuration, test evidence, troubleshooting, and accurate limitations.
- [ ] Keep raw course guideline files only if permitted and useful; do not imply the guideline set is complete.
- [ ] Ensure the two rubric/overview reference images are intentionally included/excluded, not accidentally left untracked.
- [ ] Create `submissions/milestone-2/` containing the frozen artifacts explicitly required for M2 and any handbook-required report/forms.
- [ ] Check that the main `.pkt` opens and represents the final corrected state.
- [ ] Check that all screenshots are legible and contain no misleading state.
- [ ] Review the actual eFundi submission instructions and confirm whether video/report are M2 requirements.
- [ ] Do not push/commit unless requested by Kurhula; when asked, create clear, meaningful commits with no AI attribution.

**Gate F:** all four explicit M2 items from the project diagram are present and navigable; no broken config, evidence, or misleading status remains.

## 7. Proposed repository deliverable layout

```text
docs/
  06-project-status-and-milestone-2-control.md
  07-physical-build-design.md
  08-milestone-2-execution-map.md
configs/
  r1.txt
  isp.txt
  sw-core.txt
  sw-adm.txt
  sw-sal.txt
  sw-wh.txt
  sw-srv.txt
packet-tracer/
  tshimo-agri-hq-baseline.pkt
  tshimo-agri-hq-milestone-2.pkt
screenshots/
  01-physical-topology.png
  02-vlans.png
  03-trunks.png
  04-r1-interfaces.png
  05-r1-routes.png
  06-dhcp-clients.png
  07-same-vlan-ping.png
  08-inter-vlan-ping.png
  09-dns-test.png
  10-service-test.png
  11-nat-test.png
  12-cr1-capacity.png
  troubleshooting-before.png
  troubleshooting-isolation.png
  troubleshooting-after.png
troubleshooting/
  advanced-fault-isolation.md
submissions/
  milestone-2/
    (frozen hand-in copies, assembled after successful verification)
```

The ISP may be integrated into the Packet Tracer file as a router, but include `configs/isp.txt` if configured as a separate device.

## 8. Final submission preparation (not to confuse with M2 hand-in)

The supplied project diagram schedules Final Submission for 16 October 2026 and lists:

1. Packet Tracer project (`.pkt`).
2. GitHub portfolio of evidence.
3. Technical report.
4. 15-20 minute inset video demonstration.
5. Any additional files required by the project handbook.

The video should explain the client/design, physical and logical topology, addressing, selected services, working tests, assigned fault isolation, the correction, and evidence of recovery. The student should be able to defend all decisions and answer questions; use the video rubric's Excellent criteria as the target. Do not record the final version until the network and evidence are stable.

## 9. Time plan and daily exit conditions

This schedule is a practical allocation from 25 September to the 2 October deadline; adjust based on actual Packet Tracer progress while preserving test and evidence time.

| Date | Main objective | Exit condition |
|---|---|---|
| 25 Sep | Confirm unknowns; correct feasible port plan; build physical topology | All devices/links built; initial `.pkt` saved; topology screenshot captured |
| 26 Sep | VLANs, trunks, access-port configuration | `show vlan brief` and `show interfaces trunk` match the design |
| 27 Sep | R1 subinterfaces, WAN, routing | Gateways reachable; connected/default routes verified |
| 28 Sep | DHCP, DNS, server service | Every user VLAN obtains expected lease; DNS/service smoke tests pass |
| 29 Sep | NAT/PAT, end-to-end tests, CR1 | Full functional test matrix run; failures fixed and logged |
| 30 Sep | Controlled troubleshooting fault and recovery | Before/fault/isolation/fix/after evidence captured |
| 1 Oct | Documentation, GitHub portfolio, report check | All M2 package artifacts exist; handoff/repository review complete |
| 2 Oct | Final open-and-verify and submit | Corrected `.pkt` opens; four explicit M2 items submitted; actual eFundi receipt/status checked |

Reserve the last day for verification and upload, not first-time configuration.

## 10. Explicit unknowns and approval record

The following are not fully specified by the available local sources:

- Full project handbook and exact eFundi submission form/instructions.
- Whether a technical report or video is required at Milestone 2 or only at Final.
- Exact Packet Tracer versions and device model inventory available to the student.
- Whether the assigned troubleshooting feature has a lecturer-specific fault type beyond the rubric/overview wording.
- Whether an internal file service must use a specified protocol.
- Exact DHCP/DNS service implementation requested by the lecturer.
- New lecture notes may add or refine curriculum expectations.

Record approvals/changes here before configuration begins:

| Decision | Approved value | Confirmed by | Date |
|---|---|---|---|
| Router and interface model | Cisco 2911 (Gi0/0, Gi0/1) | Kurhula | 25 Sep 2026 |
| Core/access switch model and port map | Cisco 2960-24TT; SW-CORE Gi0/1 to R1, Fa0/1-Fa0/4 to access switches | Kurhula | 25 Sep 2026 |
| DHCP architecture and server IP | Dedicated Server-PT DHCP in VLAN 40 at `10.23.0.194/26`, R1 `ip helper-address` relay | Kurhula | 25 Sep 2026 |
| DNS/file service and server IPs | DNS-SRV `10.23.0.195/26`; FILE-SRV `10.23.0.196/26` | Kurhula | 25 Sep 2026 |
| ISP model and addressing | R1 WAN `10.23.1.1/30`; ISP WAN `10.23.1.2/30`; ISP test target `203.0.113.1/32` | Kurhula | 25 Sep 2026 |
| Troubleshooting fault | Remove VLAN 30 from SW-CORE Fa0/3 allowed-VLAN list | Kurhula | 25 Sep 2026 |
| M2 report/video requirement checked against handbook | Pending — diagram indicates no report/video required at M2, full handbook not yet reviewed | Pending | Pending |
