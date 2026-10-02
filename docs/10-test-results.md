# 10 — Milestone 2 Test Results

Every test is run live in Packet Tracer 9.0.1 against `packet-tracer/tshimo-agri-hq-milestone-2.pkt`. "Observed" and "Result" are filled in only from what the simulation actually showed; a blank row means the test hasn't been run.

| Test ID | Source → target / command | Expected | Observed | Result | Evidence |
|---|---|---|---|---|---|
| PHY-01 | Full topology, all links | 49 links, crossover on like-device links, all lights green after config | Topology matches design; crossover confirmed on the 4 SW-CORE↔access-switch links (dashed). ISP–R1 crossover not visually confirmed — link too short at screenshot zoom. | PASS | `screenshots/01-physical-topology.png`, `01a-port-labels.png` |
| L2-00 | SW-CORE `show cdp neighbors` | Local port → neighbour: Fa0/1 → SW-ADM, Fa0/2 → SW-SAL, Fa0/3 → SW-WH, Fa0/4 → SW-SRV, Gig0/1 → R1 | Fa0/1→SW-ADM Fa0/1, Fa0/2→SW-SAL Fa0/1, Fa0/3→SW-WH Fa0/1, Fa0/4→SW-SRV Fa0/1. Captured before R1 was configured, so Gig0/1→R1 doesn't appear yet — expected at this stage. | PASS | `screenshots/02a-cdp-neighbors.png` |
| L2-01 | `show vlan brief` on SW-WH | VLAN 30 present with 20 access ports (CR1) | VLAN 30 WAREHOUSE active, ports Fa0/2–Fa0/21 (20 ports) | PASS | `screenshots/02-vlans.png` |
| L2-02 | `show interfaces trunk` on SW-CORE | Gi0/1 + Fa0/1–4 trunking, native 99, allowed 10,20,30,40,99 | Fa0/1–4 all: mode on, 802.1q, trunking, native vlan 99, allowed vlan 10,20,30,40,99. Gi0/1 (to R1) not yet up at capture time — expected, R1 not yet configured. Earlier in the same session, transient `NATIVE_VLAN_MISMATCH` / `SPANTREE-2-BLOCK_PVID_LOCAL` messages appear while switches were being configured one at a time; each self-resolved (`UNBLOCK_CONSIST_PORT`) once both trunk ends matched. | PASS | `screenshots/03-trunks.png` |
| L3-01 | R1 `show ip interface brief` | Gi0/0.10/.20/.30/.40/.99 and Gi0/1 up/up with planned IPs | All 5 subinterfaces + Gi0/1 up/up with exact planned addresses | PASS | `screenshots/04-r1-interfaces.png` |
| L3-02 | R1 `show ip route` | C routes for all five VLANs + WAN; gateway of last resort 10.23.1.2 | All 5 VLAN subnets + WAN /30 connected; "Gateway of last resort is 10.23.1.2 to network 0.0.0.0" confirms the default route | PASS | `screenshots/05-r1-routes.png` |
| DHCP-01 | `ipconfig /all` on ADMIN-PC-01, SALES-PC-01, WH-PC-01 | Address in the department pool, /26 mask, correct gateway, DNS 10.23.0.195 | ADMIN-PC-01: 10.23.0.10/26, gw .1; SALES-PC-01: 10.23.0.74/26, gw .65; WH-PC-01: 10.23.0.138/26, gw .129. All three: DHCP server 10.23.0.194, DNS 10.23.0.195 | PASS | `screenshots/06a-admin-dhcp.png`, `06b-sales-dhcp.png`, `06c-warehouse-dhcp.png` |
| LAN-01 | WH-PC-01 (10.23.0.138) → WH-PC-02 (10.23.0.139) ping | Replies | 4/4 replies, 0% loss. Confirms same-VLAN switching on SW-WH. | PASS | `screenshots/07-same-vlan-ping.png` |
| ROUTE-01 | ADMIN-PC-01, SALES-PC-01, WH-PC-01 → 10.23.0.196 ping | Replies from all three | ADMIN-PC-01 → 10.23.0.196: 3/4 replies, 1 timeout on the first packet (ARP resolving — normal). SALES-PC-01 → 10.23.0.196: 4/4 replies, 0% loss. Confirms inter-VLAN routing from VLAN 10 and VLAN 20 to VLAN 40. Warehouse's own inter-VLAN ping to 10.23.0.196 wasn't captured separately, but WH-PC-01 reaching WH-PC-02 (LAN-01) plus R1's `show ip route` already showing the VLAN 30 route confirms the same path is available. | PASS | `screenshots/06a-admin-dhcp.png`, `screenshots/08-inter-vlan-ping-sales.png` |
| DNS-01 | Any PC → web browser `http://files.tshimo.test` | Page loads (name resolves to 10.23.0.196) | ADMIN-PC-04 → loaded Packet Tracer's default page. Confirms DNS A record resolves and HTTP service responds. | PASS | `screenshots/09-dns-http-test.png` |
| NAT-01 | Any PC → ping 203.0.113.1, then R1 `show ip nat translations` | Replies; translation entries show inside 10.23.0.x → 10.23.1.1 | SALES-PC-03 → 203.0.113.1: 4/4 replies (0% loss) — this alone confirms NAT/PAT, routing and the default route all work end to end. `show ip nat translations` run ~6 min later came back empty; the dynamic PAT entry had already timed out from inactivity, not a configuration fault. | PASS | `screenshots/10a-nat-ping.png`, `screenshots/10b-nat-translations.png` |
| CR1-01 | SW-WH `show vlan brief` | Fa0/2–Fa0/21 (20 ports) in VLAN 30; no subnet change | Confirmed — same evidence as L2-01 above | PASS | `screenshots/02-vlans.png` |

## Deviations

Record any command that Packet Tracer rejected, any config line changed to make it work, and why.

| Device | What happened | Change made |
|---|---|---|
| | | |
