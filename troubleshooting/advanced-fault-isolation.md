# Advanced Fault Isolation — VLAN pruned from a trunk

**Assigned challenge:** Network Troubleshooting, fault isolation scenario (Advanced).
**Network:** Tshimo Agri Supplies HQ, `packet-tracer/tshimo-agri-hq-milestone-2.pkt`.

Sections marked **Observed** are filled in only from what Packet Tracer actually showed during the demonstration. Everything else is the plan.

## 1. Scenario

A change on the core switch accidentally removes VLAN 30 (Warehouse) from the allowed-VLAN list on the SW-CORE → SW-WH trunk (SW-CORE Fa0/3). It's a realistic one-word mistake: typing `switchport trunk allowed vlan 10,20,40,99` instead of `...10,20,30,40,99`, or `remove 30` on the wrong port.

Injected on SW-CORE:

```
configure terminal
interface fastEthernet0/3
 switchport trunk allowed vlan remove 30
end
```

The fault is not saved (`write memory` is not run, and the `.pkt` is not saved while it's active).

## 2. Healthy baseline (before the fault)

From WH-PC-01 (Command Prompt):

| Check | Expected | Observed | Result |
|---|---|---|---|
| `ipconfig` | 10.23.0.138–.187, /26, gateway 10.23.0.129 | WH-PC-01: 10.23.0.138/26, gateway 10.23.0.129 | PASS |
| `ping 10.23.0.129` (gateway) | Replies | 4/4, 0% loss | PASS |
| `ping 10.23.0.196` (FILE-SRV) | Replies | 4/4, 0% loss | PASS |
| `ping` WH-PC-02 (10.23.0.139) | Replies | 4/4, 0% loss (captured separately during M2 functional testing, same healthy pre-fault network) | PASS |

Evidence: `screenshots/12-fault-baseline.png`, `screenshots/07-same-vlan-ping.png`

## 3. Symptoms (after the fault)

**Expected, as a hypothesis to test — not a conclusion:** Warehouse PCs can still reach each other (same switch, same VLAN), but can't reach their gateway or anything in another VLAN. Other departments are unaffected.

| Check from WH-PC-01 | Observed |
|---|---|
| `ping` gateway 10.23.0.129 | 100% loss (4/4 timed out) |
| `ping` FILE-SRV 10.23.0.196 | 100% loss (4/4 timed out) |
| `ping` WH-PC-02 (10.23.0.139) | 0% loss (4/4 replies) — same-VLAN switching unaffected |
| ADMIN-PC-01 `ping 10.23.0.196` (control test) | 4/4, 0% loss — Admin unaffected |

The control test passes while Warehouse fails, so the fault is scoped to Warehouse only. Everything Admin depends on (R1, SW-CORE's other ports, SW-SRV, the file server) is working, which leaves the path only Warehouse uses: SW-WH and its uplink to SW-CORE.

**Process note, kept for an honest record:** the first attempt at this step went wrong. During fault injection, `sw-wh.txt` was pasted onto the physical core switch. That overwrote its hostname to `SW-WH` and converted three of its four access-switch trunks (Fa0/2, Fa0/3, Fa0/4) into VLAN 30 access ports. On that first attempt the Admin control test failed three times in a row (25%, then 100%, then 100% loss), which didn't fit the design. Instead of recording it, it was investigated:
- `show cdp neighbors` showed the device labelled `SW-WH#` had all 5 of the core switch's neighbours, so it was physically SW-CORE.
- `configs/sw-core.txt` was re-applied in full and `show interfaces trunk` confirmed all trunks were restored.
- The file was saved, and only the intended one-line fault was re-injected.

All results in this section are from that clean rerun. The three failed Admin pings are still visible higher up in the scrollback of the original `screenshots/13b-fault-symptoms-admin-control.png`. They belong to the misconfigured period, not to the fault; the report's Figure 16 is cropped to the clean rerun only.

Evidence: `screenshots/13a-fault-symptoms-wh.png`, `screenshots/13b-fault-symptoms-admin-control.png`

## 4. Isolation, layer by layer

The method works bottom-up and only moves up a layer once the layer below is proven good.

**Scoping first.** Admin and Sales still work, and Warehouse PCs can still reach each other. That rules out R1, the servers, and SW-WH's access ports, and points at the path between SW-WH and R1: the SW-WH ↔ SW-CORE trunk, or SW-CORE ↔ R1.

| Layer | Command (device) | What it rules in or out | Observed |
|---|---|---|---|
| 2 — Data link | `show interfaces trunk` (SW-CORE) | Compare Fa0/3's allowed list against Fa0/1, Fa0/2, Fa0/4 | Fa0/1: `10,20,30,40,99`. Fa0/2: `10,20,30,40,99`. **Fa0/3: `10,20,40,99` — VLAN 30 missing.** Fa0/4: `10,20,30,40,99`. Gig0/1: `10,20,30,40,99`. All four ports show `trunking`/`on`, native VLAN 99 — only the allowed list on Fa0/3 differs. This isolates the fault to a single port's VLAN membership, not a link, mode, or native VLAN problem. | PASS |

Scoping before this check already ruled out Physical (link lights up, WH-PC-01 reaches WH-PC-02 on the same switch) and most of Network (WH-PC-01's own addressing was unaffected — same DHCP lease as before the fault). The trunk comparison above was sufficient to find the exact cause without needing every layer individually.

Evidence: `screenshots/14-fault-isolation-trunk.png`.

## 5. Root cause

**Observed:** SW-CORE's Fa0/3 (the trunk to SW-WH) had VLAN 30 removed from its allowed-VLAN list via `switchport trunk allowed vlan remove 30`, while every other trunk port (Fa0/1, Fa0/2, Fa0/4, Gig0/1) kept the full `10,20,30,40,99` list. Because Fa0/3 is the only physical path between SW-WH and the rest of the network, this single change blocked all VLAN 30 traffic from crossing that link — Warehouse PCs could still switch locally (same VLAN, same physical switch, doesn't cross the trunk) but lost every route that depended on reaching R1 or another VLAN.

## 6. Fix

Smallest change that restores the design, on SW-CORE:

```
configure terminal
interface fastEthernet0/3
 switchport trunk allowed vlan add 30
end
write memory
```

## 7. Verification

| Check | Expected | Observed | Result |
|---|---|---|---|
| SW-CORE `write memory` | Fix saved permanently (not left as a transient fault) | `[OK]` | PASS |
| SW-CORE `show interfaces trunk` | Fa0/3 allowed 10,20,30,40,99, matching Fa0/1/2/4 | Fa0/3 back to `10,20,30,40,99`, identical to the other three ports | PASS |
| WH-PC-01 `ping 10.23.0.129` (gateway) | Replies | 4/4, 0% loss | PASS |
| WH-PC-01 `ping 10.23.0.196` (file server) | Replies | 4/4, 0% loss, run immediately after the gateway test above | PASS |

Evidence: `screenshots/14-fault-isolation-trunk.png` (post-fix trunk state), `screenshots/15-fault-fixed.png` (both pings passing).

## 8. Why this fault is a good Advanced case

- The physical link is up, so link lights and Layer 1 checks look healthy. You can't spot it by looking at the topology.
- The failure is partial. Local Warehouse traffic still works, which can mislead a troubleshooter into blaming the gateway or DHCP.
- The two ends of the trunk disagree: SW-WH still allows VLAN 30, SW-CORE doesn't. Checking only one end misses it.
- It's a one-line mistake with a one-line fix. Finding it depends on method, not on rebuilding.

## 9. Final state

The primary submitted `.pkt` (`packet-tracer/tshimo-agri-hq-milestone-2.pkt`) is left in this fully repaired, fully functional state — `write memory` was run on SW-CORE and the Packet Tracer project itself was saved after the fix, not during the fault.
