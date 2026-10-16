# 12 — Final Video Script and Defence Prep

The video is 15% of the grade and the rubric's top band asks for four things: a clear, well-structured, professional presentation; the inset (webcam) present throughout; excellent explanation of design, implementation and testing; and confident answers that justify decisions. This file is a running order, a demo script, a pre-recording checklist and a question bank. Speak in your own words. The bullets are prompts, not a script to read aloud.

Target length is **17–18 minutes**, which leaves margin inside the required 15–20.

## 1. Before you record (15 minutes)

1. **Work on a copy.** Copy `submissions/final/Final_TshimoAgriHQ_Maluleke_48277444.pkt` to your Desktop and rename it `demo-copy.pkt`. Open that. Never save over the submission file, and never save the copy while the fault is active.
2. **Verify the file is healthy** (this has never been checked by anyone but you). On SW-CORE run `show interfaces trunk`. Fa0/1–Fa0/4 and Gi0/1 must all show `10,20,30,40,99`. If Fa0/3 is missing VLAN 30, stop and tell me.
3. **Close every device window**, then open each device fresh when you need it, and **read the prompt before typing** (`SW-CORE#`, `R1#`). The one real incident in this project was a config pasted onto the wrong device.
4. Check the DHCP leases still show on WH-PC-01 and ADMIN-PC-01 (`ipconfig`). If a PC has no address, set it to DHCP again.
5. Set up OBS: Sources → Display Capture, plus Video Capture Device (webcam) resized into a corner. Output MP4, 1080p. Record 20 seconds as a test and check the webcam and sound. Windows Game Bar generally records one window and does not composite a webcam, so use OBS for the inset. Test it before relying on it.
6. Close chat apps, mute notifications, and have these open and ready: the PDF report, `docs/` in the repo on GitHub, the `.pkt`, the `configs/` folder.
7. **eFundi limits (seen 16 Oct):** the Final assignment accepts only document types (.doc/.docx, .xls/.xlsx, PowerPoint, .pdf, .ps/.eps, .txt, .htm/.html, .rtf, .odt, .wpd, .hwp). **Neither `.pkt` nor a video file (`.mp4`) is on that list**, so both go in as links, not attachments. There are **0 resubmissions**. Originality is checked by Turnitin.

## 1a. Hosting the video and submitting (do this in order)

1. Upload the finished video to **YouTube as "Unlisted"**, or to OneDrive / Google Drive with "anyone with the link can view". Do not use a setting that needs the marker to sign in.
2. Open the link in a **private/incognito window** and play the first 10 seconds with sound, to prove a stranger can watch it.
3. Open `submissions/final/eFundi_Submission_Text.txt`, replace `[PASTE VIDEO LINK HERE]` with the real link, and paste the whole text into eFundi's text box.
4. Attach `Final_TechnicalReport_Maluleke_48277444.pdf` only.
5. Check that the GitHub `.pkt` link in the text opens (it requires the repo to be public, or the marker to have access).
6. Click **Save** first and re-read everything. With 0 resubmissions, press **Submit** only once, and before **23:55 on 16 Oct**.

## 2. Running order

| Time | Segment | On screen | Say |
|---|---|---|---|
| 0:00–1:30 | **Client and brief** | Report cover, then Section 1 | Who the client is. What the brief gave: `10.23.0.0/16`, branch within 18 months, CR1, the troubleshooting challenge. **What it did not give: the departments and headcount were my assumption** (8 / 12 / 12+8 / 3 servers), made at Milestone 1. Say it plainly. |
| 1:30–5:00 | **Design and addressing** | Report Section 2, Figure 1 (address map) | Topology: ISP → R1 → SW-CORE → four access switches. Why router-on-a-stick. Why every department is a `/26`: 62 usable hosts, so CR1 needed no re-address. HQ is two contiguous `/24`s so it summarises as `10.23.0.0/23`. The top half of the `/16` is reserved for the branch. Why native VLAN 99. Mention the alternatives you rejected (Section 2.1). |
| 5:00–9:30 | **Live tour of the build** | Packet Tracer `demo-copy.pkt` | Topology and port labels. SW-CORE `show interfaces trunk`. SW-WH `show vlan brief`, noting 20 ports (CR1). R1 `show ip interface brief` and `show ip route`. DHCP-SRV pools. A PC's `ipconfig`. Ping across VLANs. Browse `http://files.tshimo.test`. Ping `203.0.113.1`, then **immediately** `show ip nat translations` on R1 so the entry is still there. |
| 9:30–15:00 | **Live fault demo** | Packet Tracer (steps in section 3 below) | The scenario, then baseline, inject, symptoms, control test, isolation, fix, verification. Narrate the reasoning, not just the commands. |
| 15:00–17:00 | **Issues and what you learned** | Report Section 7, then Section 9 | The wrong config pasted onto the core switch: how the Admin control test exposed it and how CDP confirmed it. The cabling and port-plan corrections. What you would do differently. |
| 17:00–18:00 | **Wrap-up** | Report Section 8 and 6.2 | Limitations (single point of failure, no ACLs). The branch design is a proposal, not built. Where everything is: the `.pkt`, the report, the GitHub repo. |

## 3. Live fault demo — step by step

Check the prompt reads `SW-CORE#` **before** typing anything on the core switch.

1. **Baseline.** WH-PC-01 command prompt:
   `ping 10.23.0.129` (gateway), `ping 10.23.0.196` (file server), `ping 10.23.0.139` (WH-PC-02). All should reply. Say: this is the healthy network.
2. **Inject** on SW-CORE (not saved):
   ```
   configure terminal
   interface fastEthernet0/3
    switchport trunk allowed vlan remove 30
   end
   ```
3. **Symptoms.** From WH-PC-01 repeat the three pings. Recorded result: gateway and file server fail (100% loss); WH-PC-02 still replies. Say these are *hypotheses I predicted*, and now I'm checking them.
4. **Control test.** From ADMIN-PC-01 `ping 10.23.0.196`. Recorded result: replies. Say what that proves: the fault is only Warehouse, so the servers and R1's routing for other VLANs are fine.
5. **Isolate by layer.** Layer 1: link lights are up and local pings work. Layer 3: WH-PC-01 still holds its lease and gateway. **Also run `show ip interface brief` on R1** and show Gi0/0.30 up/up. In the recorded evidence R1 was ruled out by inference, and showing it directly closes that gap. Layer 2: on SW-CORE `show interfaces trunk`, and point at Fa0/3's allowed list `10,20,40,99` against `10,20,30,40,99` on the other four ports.
6. **Say the root cause in one sentence:** Fa0/3 is the only path between SW-WH and the rest of the network, and VLAN 30 is no longer allowed on it.
7. **Fix:**
   ```
   configure terminal
   interface fastEthernet0/3
    switchport trunk allowed vlan add 30
   end
   write memory
   ```
   Say why `add` and not retyping the whole list.
8. **Verify.** `show interfaces trunk` again, then repeat the gateway and file-server pings from WH-PC-01. Both should reply.

**If a result differs from the recorded one**, say what you actually see and reason from it. Do not force the expected answer. The strongest moment in this project was believing a result that contradicted the prediction.

## 4. Question bank

Likely questions, with the answer the evidence supports. Know *why*, not just the answer.

**Design**

- **Why a `/26` for every department?** A `/26` has 6 host bits, so 64 addresses minus network and broadcast = 62 usable. One uniform rule is simple to operate, and CR1 (12 → 20 Warehouse) fit with no re-address. A `/27` (30 hosts) would also have fitted CR1 but left only 10 spare. Cost: the four business `/26`s use 256 addresses, 0.4% of the `/16`.
- **Why not a Layer 3 switch?** Router-on-a-stick keeps routing, relay and NAT in one device, which keeps the fault space small, and the 2960 is Layer 2 only. Trade-off: one 1 Gbps trunk carries all inter-VLAN traffic, which is ample for 43 hosts.
- **What does the summary route `10.23.0.0/23` give you?** `10.23.0.0/24` and `10.23.1.0/24` differ only in the last bit of the third octet, so one `/23` covers both. An upstream router needs one route instead of nine.
- **Why reserve a whole `/17` for the branch?** The brief gives no branch size. Fixing HQ now and holding back half the allocation avoids inventing detail. The `/17` holds 64 blocks of `/23`.
- **Why native VLAN 99 and not VLAN 1?** It keeps untagged traffic out of the default VLAN, and a native mismatch logs loudly.
- **Why a dedicated DHCP server with a relay?** Central pools in the server VLAN, and it demonstrates `ip helper-address`. The cost is that new leases depend on the server, the relay and VLAN 40.
- **Why doesn't NAT translate `10.23.1.0/24`?** The ACL matches only `10.23.0.0/24`, so WAN and management addresses keep their real addresses.
- **The department sizes, where did they come from?** They were my assumption at Milestone 1. The brief did not give them. The method would be the same for different numbers.

**Implementation**

- **How does DHCP work across VLANs?** The PC broadcasts; the broadcast does not leave its VLAN. R1's `ip helper-address 10.23.0.194` on the subinterface forwards it to DHCP-SRV as unicast, stamped with the subinterface's address. The server uses that to choose the matching pool.
- **Why crossover cables?** Like devices (switch to switch, router to router) use crossover by the standard rule. Auto-MDIX would probably have brought straight-through up anyway, but the rubric grades correct cabling.
- **Why Fa0/1–Fa0/4 on the core and not Gi0/2–Gi0/5?** A 2960-24TT has only two Gigabit ports. The change was port-only, with no VLAN or address change.
- **What does `encapsulation dot1Q 99 native` do?** Untagged frames on the trunk land in that subinterface, matching the switches' native VLAN 99.
- **Why was `show ip nat translations` empty in your evidence?** It ran about six minutes after the ping, and dynamic PAT entries expire when idle. The successful ping is the proof, because an untranslated `10.23.0.x` source could not get a reply from the ISP.

**The fault**

- **Why is this fault Advanced?** The link stays up, the failure is partial (local Warehouse switching still works), and the two ends of the trunk disagree, so checking one end misses it.
- **Why did local Warehouse pings still work?** Both PCs are on the same switch and VLAN, so the frame never crosses the Fa0/3 trunk.
- **How did you rule out R1's Warehouse subinterface?** Be honest: in the recorded evidence R1 was shown up/up *before* the fault, and the fix on SW-CORE alone restoring service shows R1 was never the cause. In the live demo, show `show ip interface brief` on R1 during the fault to rule it out directly.
- **The Admin control test, why does it matter?** It scopes the fault to one department before you touch any device, which removes the servers and R1's other routes from suspicion.
- **What other faults look similar?** An R1 subinterface down or with the wrong tag gives the same visible symptom for one VLAN. The trunk check passing is what separates them. See the fault table, Section 5.9 of the report. Say clearly those were analysed, not injected.
- **What happened with the first fault attempt?** I pasted `sw-wh.txt` onto the core switch by mistake. The Admin control test failed three times, which contradicted my prediction. The prompt read `SW-WH#`; CDP showed five neighbours, so it was the core switch. I re-applied `sw-core.txt`, verified the trunks, saved, then re-injected only the one-line fault. The first explanation I gave for the failures (spanning tree reconverging) was wrong.

**Limits and growth**

- **What are the single points of failure?** R1, SW-CORE and DHCP-SRV. Fixes: redundant router (HSRP/VRRP), redundant core, second DHCP path.
- **What security is missing?** No inter-VLAN ACLs, port security, DHCP snooping or SSH hardening. They were not required, but they would reduce the blast radius of exactly this kind of mistake.
- **Is the branch built?** No. Section 6.2 and `docs/11` are a design proposal: the branch mirrors HQ shifted by +128 in the third octet, uses spare port Gi0/2, one static route and one NAT ACL line. Nothing was simulated.
- **What would you do differently?** Run the NAT table check right after the ping, show R1's Warehouse subinterface during the fault, and check the hostname before pasting any config.
