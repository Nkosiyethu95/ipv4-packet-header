# IPv4 Packet & Header (CCNA Day 10)

Study notes from **Jeremy's IT Lab** Free CCNA 200-301, Day 10 — built while learning Layer 3 with a Packet Tracer bank-branch lab.

**Author:** Innocent Nkosiyethu Mbatha (CCNA candidate)

---

## What is a packet?

A **packet** is a Layer 3 (Network layer) unit of data. It has two parts:

1. **IP header** — control information routers use to forward the packet  
2. **Payload** — the data being carried (usually a TCP or UDP segment, or an ICMP message)

On Ethernet, that whole packet sits inside a **frame** (Layer 2): Ethernet header (MACs) → IP packet → FCS trailer.

```
Frame  = Ethernet header + IP packet + trailer
Packet = IP header + payload
```

Routers rewrite the Ethernet header at every hop. They leave the IP header mostly alone, except they decrease **TTL** by 1.

---

## IPv4 packet diagram

![IPv4 packet = header + payload](images/ipv4-packet-diagram.jpg)

---

## Why the header (and its bits) matter

Every field in the IPv4 header has a **fixed size in bits**. Routers only understand the packet if those sizes are exact.

| Field | Size | Why it matters |
|-------|------|----------------|
| Version | 4 bits | Must be `4` for IPv4 |
| IHL | 4 bits | Header length in 32-bit words (min 5 = 20 bytes) |
| Type of Service | 8 bits | QoS / DSCP marking |
| Total Length | 16 bits | Entire packet size |
| Identification / Flags / Fragment Offset | 16 + 3 + 13 bits | Fragmentation |
| **TTL** | 8 bits | Max hops left; dropped when it hits 0 |
| **Protocol** | 8 bits | What the payload is (ICMP, TCP, UDP…) |
| Header Checksum | 16 bits | Header integrity |
| **Source IP** | 32 bits | Where it came from |
| **Destination IP** | 32 bits | Where it is going |
| Options | variable | Rare; padded to a 32-bit boundary |

The header is drawn as a grid of **32 bits per row**. If one field is a bit too short or too long, every field after it is read wrong — so Source IP, Destination IP and Protocol would land in the wrong place.

Minimum header size: **20 bytes** (no options).

---

## Protocol field (common values)

| Hex | Decimal | Protocol |
|-----|---------|----------|
| `0x01` | 1 | **ICMP** (ping, traceroute) |
| `0x06` | 6 | **TCP** |
| `0x11` | 17 | **UDP** |

---

## How this fits Layer 3

- **Routing** uses Destination IP to choose the next hop.  
- **TTL** stops loops; traceroute works by watching TTL expire.  
- **Protocol** tells the host which process should get the payload.  
- On a multilayer switch or router (like the bank lab distribution switches), packets are rewritten and forwarded using this header — not the Ethernet MAC alone.

---

## Lab note

These notes go with Jeremy's Day 10 video and the bank Packet Tracer topology (SVIs, HSRP, static routes to HQ). Capture traffic in Packet Tracer Simulation mode and inspect the IPv4 header on a ping from VLAN 20 to `172.16.10.10`.

---

## Sources

- Jeremy's IT Lab — Free CCNA 200-301, Day 10 (IPv4 Header)  
- Cisco CCNA 200-301 exam topics (Network Fundamentals / IP connectivity)
