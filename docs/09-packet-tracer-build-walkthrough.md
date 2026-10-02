# 09 — Packet Tracer Build Walkthrough

**Purpose:** exact, click-by-click steps to place, name, and cable the HQ topology in Cisco Packet Tracer, following the approved physical design in [`07-physical-build-design.md`](07-physical-build-design.md) and the locked decisions in [`06-project-status-and-milestone-2-control.md`](06-project-status-and-milestone-2-control.md) §5.

**Scope of this document:** physical placement, naming, and cabling only — no IOS commands yet. IOS configuration (VLANs, trunks, subinterfaces, DHCP) is Phase C and comes after this build is verified (Gate B in `07-physical-build-design.md` §7).

**Platform note:** Claude Code cannot open, click, or type into Packet Tracer. Kurhula performs every step below inside the application; report back what the canvas/interface names actually look like (especially if a model isn't in the palette or an interface name differs) so this guide gets corrected before configuration starts.

---

## 0. Before you start

- Confirm your Packet Tracer version (Help > About). Icon layout below matches recent 8.x releases; if your palette looks different, tell me what you see instead of guessing.
- Open a new blank project. Save immediately as `packet-tracer/tshimo-agri-hq-baseline.pkt` so autosave has a target from the start.
- Device category bar is the row of icons bottom-left of the canvas. Within each category, the specific device models are the second row of icons that appears when you click a category.

---

## 1. Place the 7 network devices

Work top-to-bottom or left-to-right so the canvas stays readable — put ISP at the top/left, R1 below it, SW-CORE below R1, then the four access switches in a row beneath SW-CORE.

| # | Device | Category | Model to select | Rename to (double-click label under icon) |
|---|---|---|---|---|
| 1 | Edge router | Routers | **2911** | `R1` |
| 2 | ISP router | Routers | **2911** (or **1941**/**2811** if 2911 isn't available — tell me which) | `ISP` |
| 3 | Core switch | Switches | **2960-24TT** | `SW-CORE` |
| 4 | Admin switch | Switches | **2960-24TT** | `SW-ADM` |
| 5 | Sales switch | Switches | **2960-24TT** | `SW-SAL` |
| 6 | Warehouse switch | Switches | **2960-24TT** | `SW-WH` |
| 7 | Server switch | Switches | **2960-24TT** | `SW-SRV` |

**How to place one:** click the category icon (Routers or Switches) → click the specific model icon → click on the canvas where you want it → the device appears. Click the text label underneath the device icon to rename it exactly as in the table (device names are case-sensitive in later commands, keep them exactly as shown).

**If the 2911 or 2960-24TT is missing from your palette:** stop and tell me the exact model list you see. Do not substitute silently — a different model can mean different interface names (e.g. a 2950 switch has no Gi ports at all), which would break the port plan in `07-physical-build-design.md`.

**Checkpoint:** you should now have 7 device icons on the canvas, correctly labelled, with nothing cabled yet.

---

## 2. Place the 43 endpoints

Endpoints are added in four department batches plus one server batch. Placing one, then copy-pasting and renaming, is faster than dragging each individually.

### 2.1 Admin — 8 PCs

1. Category: **End Devices** → model **PC-PT**. Place one PC near SW-ADM.
2. Rename it `ADMIN-PC-01`.
3. Select it (click once), copy (`Ctrl+C`), paste (`Ctrl+V`) 7 times near the same area.
4. Rename each pasted copy sequentially: `ADMIN-PC-02` through `ADMIN-PC-08`.

### 2.2 Sales — 12 PCs

Same pattern near SW-SAL: `SALES-PC-01` through `SALES-PC-12`.

### 2.3 Warehouse — 20 PCs

Same pattern near SW-WH: `WH-PC-01` through `WH-PC-20`. This is the CR1-grown department — all 20 go in now; there's no separate "add later" step since the port headroom already accounts for CR1.

### 2.4 Servers — 3 servers

1. Category: **End Devices** → model **Server-PT**. Place 3 near SW-SRV, physically separated from the user PCs.
2. Rename: `DHCP-SRV`, `DNS-SRV`, `FILE-SRV`.

**Checkpoint:** canvas has 7 network devices + 43 endpoints = 50 icons total, all labelled, still nothing cabled.

---

## 3. Cable everything

Category: **Connections**. Pick the cable by what it joins:

- **Copper Cross-Over** between like devices: router↔router, switch↔switch. Packet Tracer draws these as dashed lines.
- **Copper Straight-Through** between unlike devices: router↔switch, switch↔PC/server. Drawn as solid lines.

Click the cable icon, click the first device, choose the port, click the second device, choose the port.

### 3.1 Network uplinks (6 links)

| Cable | From | Port | To | Port | Cable type |
|---|---|---|---|---|---|
| C01 | ISP | Gi0/0 | R1 | Gi0/1 | Cross-Over |
| C02 | R1 | Gi0/0 | SW-CORE | Gi0/1 | Straight-Through |
| C03 | SW-CORE | Fa0/1 | SW-ADM | Fa0/1 | Cross-Over |
| C04 | SW-CORE | Fa0/2 | SW-SAL | Fa0/1 | Cross-Over |
| C05 | SW-CORE | Fa0/3 | SW-WH | Fa0/1 | Cross-Over |
| C06 | SW-CORE | Fa0/4 | SW-SRV | Fa0/1 | Cross-Over |

### 3.2 Endpoint links (43 links) — all Copper Straight-Through

| Group | From | Port range | To | Port range |
|---|---|---|---|---|
| Admin | ADMIN-PC-01 → 08 | Fa0 | SW-ADM | Fa0/2 → Fa0/9 |
| Sales | SALES-PC-01 → 12 | Fa0 | SW-SAL | Fa0/2 → Fa0/13 |
| Warehouse | WH-PC-01 → 20 | Fa0 | SW-WH | Fa0/2 → Fa0/21 |
| Servers | DHCP-SRV, DNS-SRV, FILE-SRV | Fa0 | SW-SRV | Fa0/2, Fa0/3, Fa0/4 |

Keep the numbering consistent — PC-01 to the lowest port, PC-02 to the next, and so on — so screenshots and config files can be cross-checked without guessing later.

**Do not** connect any endpoint to a trunk/uplink port, and don't add extra links beyond this schedule.

---

## 4. What the finished baseline should look like

Before saving, visually check:

- [ ] 7 network devices + 43 endpoints, all labelled exactly per the tables above.
- [ ] One clean core (SW-CORE) with 4 clearly separated access zones beneath it (Admin, Sales, Warehouse, Servers), R1 and ISP above.
- [ ] 49 total links: 6 uplinks + 43 endpoint links.
- [ ] Switch-side link lights are green or amber (amber is spanning tree settling; it turns green within a minute).
- [ ] The ISP–R1 and R1–SW-CORE links show **red at the router ends**. That's expected: router interfaces start shut down until `no shutdown` is configured in Phase C.
- [ ] The 5 like-device links (ISP–R1 and SW-CORE to each access switch) are drawn **dashed** (crossover); every other link is solid (straight-through).
- [ ] No device is connected to more than its documented ports (e.g. SW-CORE should have exactly Gi0/1 + Fa0/1-Fa0/4 in use, nothing else).

If any link shows amber/red or won't connect, it usually means a port mismatch (e.g. trying to use a port that doesn't exist on that model) — report the exact device/port and I'll adjust the plan rather than you guessing a workaround.

---

## 5. Save and hand back

1. Save as `packet-tracer/tshimo-agri-hq-baseline.pkt` (already your save target from step 0).
2. Take a screenshot of the full canvas, save as `screenshots/01-physical-topology.png`.
3. Report back: did every device/model match this guide, did all 49 cables connect cleanly, and are there any renamed/substituted devices I need to know about.

Once confirmed, we move to Phase C: I'll draft the first IOS block (switch VLANs/trunks/access ports) for you to paste into SW-CORE, SW-ADM, SW-SAL, SW-WH, and SW-SRV one at a time.
