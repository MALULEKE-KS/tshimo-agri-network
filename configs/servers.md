# Server and endpoint setup (GUI, not CLI)

Server-PT and PC-PT devices are configured through their Packet Tracer tabs, not an IOS console. Click the device, then use the tabs named below. Do this **after** the switch, R1, and ISP configs are pasted in.

All three servers sit in VLAN 40 (`10.23.0.192/26`, gateway `10.23.0.193`).

## DHCP-SRV — `10.23.0.194`

**Desktop → IP Configuration → Static**

| Field | Value |
|---|---|
| IPv4 Address | `10.23.0.194` |
| Subnet Mask | `255.255.255.192` |
| Default Gateway | `10.23.0.193` |
| DNS Server | `10.23.0.195` |

**Services → DHCP → Service: On**, then create one pool per user VLAN. Fill the fields, click **Add**. Edit the built-in `serverPool` (click it, change fields, click **Save**) — it can't be deleted.

| Pool Name | Default Gateway | DNS Server | Start IP Address | Subnet Mask | Maximum Users |
|---|---|---|---|---|---|
| `ADMIN-POOL` | `10.23.0.1` | `10.23.0.195` | `10.23.0.10` | `255.255.255.192` | `50` |
| `SALES-POOL` | `10.23.0.65` | `10.23.0.195` | `10.23.0.74` | `255.255.255.192` | `50` |
| `WH-POOL` | `10.23.0.129` | `10.23.0.195` | `10.23.0.138` | `255.255.255.192` | `50` |
| `serverPool` (edit) | `10.23.0.193` | `10.23.0.195` | `10.23.0.200` | `255.255.255.192` | `20` |

Why the start addresses: each pool begins 9 addresses above its gateway, so `.1`–`.9` of every `/26` stays free for gateways and any future static devices. 50 leases per pool still covers Warehouse's 20 users (CR1 included) with room to spare.

How requests reach this server: PCs broadcast DHCP requests inside their own VLAN. R1's `ip helper-address 10.23.0.194` on each user subinterface forwards them here as unicast, tagged with the subinterface's gateway address, and the server picks the pool whose range matches that gateway.

## DNS-SRV — `10.23.0.195`

**Desktop → IP Configuration → Static:** `10.23.0.195` / `255.255.255.192` / gateway `10.23.0.193` / DNS `10.23.0.195`

**Services → DNS → DNS Service: On**, add an A record:

| Name | Type | Address |
|---|---|---|
| `files.tshimo.test` | A Record | `10.23.0.196` |

Check **Services → DHCP** is **Off** on this server.

## FILE-SRV — `10.23.0.196`

**Desktop → IP Configuration → Static:** `10.23.0.196` / `255.255.255.192` / gateway `10.23.0.193` / DNS `10.23.0.195`

**Services → HTTP** and **Services → FTP** are On by default — leave them on. FTP's default account is `cisco` / `cisco`. Check **Services → DHCP** is **Off**.

## All 40 PCs

**Desktop → IP Configuration → DHCP.** Each PC should show "DHCP request successful" and an address from its department's pool:

| Department | Expected range | Gateway |
|---|---|---|
| Admin | `10.23.0.10`–`10.23.0.59` | `10.23.0.1` |
| Sales | `10.23.0.74`–`10.23.0.123` | `10.23.0.65` |
| Warehouse | `10.23.0.138`–`10.23.0.187` | `10.23.0.129` |

If a PC says "DHCP request failed", don't keep clicking — check `show interfaces trunk` on SW-CORE and `show ip interface brief` on R1 first, and report what you see.

## Quick service checks

- PC Command Prompt: `ping 10.23.0.196` → replies (inter-VLAN routing works)
- PC Web Browser: `http://files.tshimo.test` → Packet Tracer's default page loads (DNS + HTTP work)
- PC Command Prompt: `ping 203.0.113.1` → replies (NAT + default route work); then on R1: `show ip nat translations`
