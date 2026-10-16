# 11 — Branch Office Design Proposal

**Status: design proposal. Not built. Not simulated. Not tested.**

Nothing in this document has been configured in Packet Tracer. The IOS blocks below are written from the confirmed HQ configuration and standard IOS behaviour, and are untested. They exist to show that the reservation made in [`04 — IP Addressing Plan`](04-ip-addressing-plan.md) works. They are not evidence that a branch was built. The brief only says a branch *may* open within 18 months, with no headcount, departments or link type, so the real design must be confirmed with the client first. [`07 — Physical Build Design`](07-physical-build-design.md) says not to build the branch unless the brief changes, and that has not changed.

This closes the item left open in [`05 — Change Requests`](05-change-requests.md) ("full branch-office design work … once the branch site's requirements are defined"): it gives a concrete design to confirm or adjust, rather than leaving the scope as a promise.

## 1. The idea: HQ shifted by +128 in the third octet

| Purpose | HQ (built) | Branch (proposed) |
|---|---|---|
| VLAN 10 / 20 / 30 / 40 | `10.23.0.0/26`, `.0.64/26`, `.0.128/26`, `.0.192/26` | `10.23.128.0/26`, `.128.64/26`, `.128.128/26`, `.128.192/26` |
| VLAN 99 management / native | `10.23.1.16/28` | `10.23.129.16/28` |
| Gateways | `.1`, `.65`, `.129`, `.193`, `.1.17` | `.128.1`, `.128.65`, `.128.129`, `.128.193`, `.129.17` |
| Site summary | `10.23.0.0/23` | `10.23.128.0/23` |
| Inter-site link | `10.23.1.4/30`: R1 `.5`, branch router `.6` | |

**Why mirror HQ:** the branch uses the same VLAN IDs, the same uniform-`/26` rule and the same layout, so anyone who understands HQ understands the branch. A host's address tells you its site (third octet below 128 is HQ, 128 and above is branch) and its role. Only the VLANs the branch needs would be created; the rest of its block stays unused.

**Capacity:** `10.23.128.0/17` contains 2^(23−17) = **64** blocks of `/23`, each the size of HQ's whole footprint. Further sites can be added the same way without touching HQ.

**No overlap check:** the `/30` at `10.23.1.4` sits between the existing WAN `/30` (`10.23.1.0`–`.3`) and the management `/28` (`10.23.1.16`–`.31`), so it collides with nothing and stays inside HQ's `10.23.0.0/23` summary.

## 2. What changes at HQ — stated in full

| Change at HQ | Why |
|---|---|
| Cable R1 **Gi0/2** (spare, currently shut) to the branch router | The inter-site link; no new HQ hardware |
| `interface Gi0/2`: address `10.23.1.5/30`, `ip nat inside`, `no shutdown` | Addressing, and marks branch traffic as "inside" so NAT will translate it |
| One static route to the branch summary | HQ must know where the branch is |
| One line added to NAT ACL 1 | Only needed if the branch reaches the internet through HQ |

**What does not change:** every HQ VLAN, subnet, gateway, trunk, switch configuration and DHCP pool. The NAT ACL line is a real, small change to R1, and it is listed here deliberately, so the "no HQ redesign" claim is accurate rather than overstated.

### R1 additions (untested)

```
interface gigabitEthernet0/2
 description Inter-site link to BR1 Gi0/0
 ip address 10.23.1.5 255.255.255.252
 ip nat inside
 no shutdown
exit
ip route 10.23.128.0 255.255.254.0 10.23.1.6
access-list 1 permit 10.23.128.0 0.0.1.255
```

Traffic between branch and HQ does not get translated: NAT only applies when a packet leaves the **outside** interface (Gi0/1), and HQ-bound traffic leaves via the VLAN subinterfaces, which are inside interfaces.

## 3. Branch router sketch (untested)

```
hostname BR1
interface gigabitEthernet0/0
 description Inter-site link to R1 Gi0/2
 ip address 10.23.1.6 255.255.255.252
 no shutdown
exit
interface gigabitEthernet0/1
 no ip address
 no shutdown
exit
interface gigabitEthernet0/1.10
 encapsulation dot1Q 10
 ip address 10.23.128.1 255.255.255.192
exit
! ... .20 / .30 / .40 likewise (10.23.128.65, .129, .193), only for VLANs the branch needs
interface gigabitEthernet0/1.99
 encapsulation dot1Q 99 native
 ip address 10.23.129.17 255.255.255.240
exit
ip route 0.0.0.0 0.0.0.0 10.23.1.5
```

Branch switches would follow the same pattern as HQ's access switches: static trunks, native VLAN 99, explicit allowed list.

## 4. Decisions to confirm with the client

| Question | Why it matters | Default assumed here |
|---|---|---|
| Branch headcount and departments | Sets how many VLANs and ports | HQ-like layout, only needed VLANs created |
| Inter-site link type | Cost, security, bandwidth | Site-to-site VPN over the ISP, or a leased line. A direct Ethernet link would stand in for it in Packet Tracer |
| Internet path | Via HQ NAT (needs the one ACL line) or the branch's own ISP | Via HQ, for one egress point |
| DHCP location | Local pools survive a link outage; central needs new pools on DHCP-SRV | Local pools on BR1 (`ip dhcp pool`, DNS `10.23.0.195`) |
| Servers at the branch | Whether VLAN 40 exists there at all | Use HQ servers across the link |

Routing: two static routes are enough for two sites. A routing protocol is not justified at this scale.

## 5. Verification plan (when/if built)

| ID | Test | Pass condition |
|---|---|---|
| BR-01 | Ping `10.23.1.5` ↔ `10.23.1.6` | Link up, 4/4 |
| BR-02 | `show ip route` on both routers | R1 has the static route to `10.23.128.0/23`; BR1 has the default route |
| BR-03 | Branch PC gets DHCP | Address in the branch `/26`, correct gateway and DNS |
| BR-04 | Branch PC → FILE-SRV `10.23.0.196`, and `tracert` | Replies; path goes through BR1 then R1 |
| BR-05 | HQ PC → branch PC | Replies (confirms the return route) |
| BR-06 | Branch PC → `203.0.113.1`, then immediately `show ip nat translations` on R1 | Replies; translation entry present |
| BR-07 | Compare HQ switch and R1 VLAN/trunk/subinterface config before and after | Only the four changes in section 2 |
