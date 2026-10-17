# Lab 01 — Two PCs Connected to a Switch

## 1. Lab Overview

**Objective:** Build a basic Local Area Network (LAN) using two PCs and one Cisco 2960 switch in Cisco Packet Tracer.

**Purpose:** Understand IPv4 addressing, subnet masks, Ethernet switching, MAC address learning, and connectivity testing with ICMP ping.

## 2. Topology

- PC-A → Switch FastEthernet0/1
- PC-B → Switch FastEthernet0/2
- Device: Cisco 2960 switch
- Connection: Copper Straight-Through cables

Both PCs are connected to the same switch and belong to the same IP subnet.

## 3. IP Address Configuration

| Device | IPv4 Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| PC-A | 192.168.1.10 | 255.255.255.0 | Not configured |
| PC-B | 192.168.1.20 | 255.255.255.0 | Not configured |

The subnet mask `255.255.255.0` is equivalent to `/24`. Both PCs are in the `192.168.1.0/24` network.

A default gateway is not required for communication between these two PCs because they are in the same subnet.

## 4. Connectivity Test

Command executed from PC-A:

```text
ping 192.168.1.20
```

Observed result:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
Minimum = 0ms, Maximum = 0ms, Average = 0ms
```

**Interpretation:** PC-A received replies to all four ICMP Echo Requests. This confirms that the two PCs can communicate in this Packet Tracer scenario.

The result does not prove that every network service works or that the network is secure.

## 5. Switch MAC Address Table

Command executed on the switch:

```text
enable
show mac address-table
```

Observed dynamic entries:

| VLAN | MAC Address | Type | Switch Port |
|---|---|---|---|
| 1 | 0006.2ad5.95c8 | DYNAMIC | Fa0/1 |
| 1 | 0090.213b.5316 | DYNAMIC | Fa0/2 |

The switch learns source MAC addresses from incoming Ethernet frames and associates them with the ports on which they were learned.

Dynamic entries are learned automatically. The table may change as devices communicate or entries age out.

## 6. Interface Status

Command:

```text
show interfaces status
```

Observed results:

- `Fa0/1`: connected, VLAN 1, full duplex, 100 Mbps
- `Fa0/2`: connected, VLAN 1, full duplex, 100 Mbps
- Unused ports: notconnect

**Interpretation:**
- `connected`: The port has an active link.
- `VLAN 1`: The port belongs to VLAN 1.
- `a-full`: Full duplex was selected through auto-negotiation.
- `a-100`: 100 Mbps was selected through auto-negotiation.
- `notconnect`: No active link is detected on that port.

Unused ports showing `notconnect` is normal.

## 7. Key Concepts

- **LAN (Local Area Network):** A network connecting devices within a limited area.
- **IPv4 address:** A logical address used to identify a device at the network layer.
- **Subnet mask:** Identifies the network and host portions of an IPv4 address.
- **Switch:** Forwards Ethernet frames using its MAC address table.
- **MAC address:** A link-layer address used for communication on an Ethernet network.
- **ICMP:** A protocol used by tools such as `ping` to test reachability.
- **VLAN:** A logical Layer 2 network segment.
- **Duplex:** Describes whether a link can transmit and receive simultaneously.

## 8. Lab Conclusion

Two PCs were configured in the same IPv4 subnet and connected to a Cisco 2960 switch. Successful ping replies demonstrated connectivity. The switch's MAC address table and interface status were inspected to understand basic Layer 2 switching.

**Next step:** Add a router and test communication between different IP subnets.

## 9. Security Notes

This lab demonstrates basic connectivity, not a security assessment. In a real network, unused ports, VLAN configuration, access controls, and switch management settings should be reviewed according to the network's requirements.
