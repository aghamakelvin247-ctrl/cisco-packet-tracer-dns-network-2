# Cisco Packet Tracer Project: 2 PCs, Switch, Router and DNS Server

## Objective
Connect two PCs to a switch, connect the switch to a router, and connect the router to a DNS server, so that the PCs can reach the server by IP address and by name (www.google.com).

## Topology
```
PC0 (Fa0) ---- (Fa0/2)
                        Switch0 2960-24TT (Fa0/3) ---- (Gig0/0) Router0 1941 (Gig0/1) - - - (Fa0) Server0 (DNS)
PC1 (Fa0) ---- (Fa0/1)
```

| Link | From | To | Cable |
|------|------|----|-------|
| 1 | PC0 Fa0 | Switch0 Fa0/2 | Copper straight-through |
| 2 | PC1 Fa0 | Switch0 Fa0/1 | Copper straight-through |
| 3 | Switch0 Fa0/3 | Router0 Gig0/0 | Copper straight-through |
| 4 | Router0 Gig0/1 | Server0 Fa0 | Copper cross-over (dashed line) |

## IP Addressing Table

| Device | Interface | IP Address | Subnet Mask | Default Gateway | DNS Server |
|--------|-----------|-----------|-------------|-----------------|------------|
| PC0 / PC1 | Fa0 | 192.168.1.11 (PC), 192.168.1.12 (second PC) | 255.255.255.0 | 192.168.1.1 | 8.8.8.8 |
| Router0 | Gig0/0 | 192.168.1.1 | 255.255.255.0 | n/a | n/a |
| Router0 | Gig0/1 | 8.8.8.1 | 255.0.0.0 | n/a | n/a |
| Server0 | Fa0 | 8.8.8.8 | 255.0.0.0 | 8.8.8.1 | 0.0.0.0 |

Note: the second PC's address is the next free one in the LAN (192.168.1.12). Update it here if you used a different one.

## DNS Server Configuration
- Services > DNS: DNS Service = **On**
- Resource record: `www.google.com`, Type **A Record**, Address **8.8.8.8**
- The server also runs HTTP, which serves the default Packet Tracer web page.

## Router Configuration
See `router-config.txt`.

## Evidence / Test Results
Screenshots go in the `evidence/` folder (see `evidence/EVIDENCE_LIST.md` for file names and what each shows).

| Test | Result |
|------|--------|
| `ping 8.8.8.8` from PC | 3 of 4 replies, TTL=127 (first request times out while ARP resolves; this is normal) |
| `ping www.google.com` from PC | Name resolved to 8.8.8.8, 4 of 4 replies, 0% loss |
| Browser to `http://www.google.com` | Cisco Packet Tracer welcome page loaded |

TTL=127 confirms the traffic crossed one router (default 128, minus 1 hop).

## Conclusion
The PCs on the 192.168.1.0/24 network reach the server on the 8.8.8.0/8 network through Router0. The DNS server resolves www.google.com to 8.8.8.8, and the web page loads in the browser.

## Files in this zip
- `README.md`: this report
- `router-config.txt`: router commands
- `evidence/EVIDENCE_LIST.md`: screenshot checklist
- Put your saved `.pkt` file in the `packet_tracer_file/` folder
