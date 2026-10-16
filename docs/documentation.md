# Technical Documentation — VLSM and Static Routing

## 1. Project Overview

This lab demonstrates IPv4 subnet planning with Variable Length Subnet Masking (VLSM) and the configuration of static routes in Cisco Packet Tracer.

The initial address block is `192.168.5.0/24`. It is divided into four LAN subnets and one point-to-point subnet connecting two routers.

The network is organized as follows:

- **R1:** connects LAN 1 and LAN 2.
- **R2:** connects LAN 3 and LAN 4.
- **R1–R2 link:** carries traffic between the two routers.
- **Access switches:** connect PCs to their respective LANs.

## 2. Objectives

- Allocate IPv4 subnets according to host requirements.
- Minimize address-space waste through VLSM.
- Assign IP addresses to router interfaces and end devices.
- Configure static routes to remote LANs.
- Verify interfaces, routing tables and end-to-end connectivity.

## 3. Host Requirements

| Network | Required hosts | Selected prefix | Usable addresses |
|---|---:|---:|---:|
| LAN 1 | 45 | `/26` | 62 |
| LAN 2 | 64 | `/25` | 126 |
| LAN 3 | 14 | `/28` | 14 |
| LAN 4 | 9 | `/28` | 14 |
| R1–R2 | 2 | `/30` | 2 |

For each subnet, the required number of host addresses determines the prefix length. IPv4 LAN subnets reserve the network and broadcast addresses; the point-to-point subnet uses a `/30` mask, providing two usable IPv4 addresses.

## 4. VLSM Addressing Plan

The following table summarizes the subnet allocation derived from the requirements and confirmed by the routers' routing tables.

| Network | Network address | Subnet mask | First usable | Last usable | Broadcast |
|---|---|---|---|---|---|
| LAN 2 | `192.168.5.0/25` | `255.255.255.128` | `192.168.5.1` | `192.168.5.126` | `192.168.5.127` |
| LAN 1 | `192.168.5.128/26` | `255.255.255.192` | `192.168.5.129` | `192.168.5.190` | `192.168.5.191` |
| LAN 3 | `192.168.5.192/28` | `255.255.255.240` | `192.168.5.193` | `192.168.5.206` | `192.168.5.207` |
| LAN 4 | `192.168.5.208/28` | `255.255.255.240` | `192.168.5.209` | `192.168.5.222` | `192.168.5.223` |
| R1–R2 | `192.168.5.224/30` | `255.255.255.252` | `192.168.5.225` | `192.168.5.226` | `192.168.5.227` |

### VLSM considerations

The `/25` subnet supports the 64-host LAN because it provides 126 usable IPv4 addresses. A `/26` would provide only 62 usable addresses and would be insufficient.

The `/26` subnet accommodates 45 hosts, while each `/28` provides 14 usable addresses. The `/30` subnet provides exactly two usable addresses for the router-to-router connection.

## 5. Interface Addressing

### Router R1

| Interface | IPv4 address | Connected network |
|---|---|---|
| `GigabitEthernet0/1` | `192.168.5.190/26` | LAN 1 |
| `GigabitEthernet0/2` | `192.168.5.126/25` | LAN 2 |
| `GigabitEthernet0/0/0` | `192.168.5.225/30` | R1–R2 |

### Router R2

| Interface | IPv4 address | Connected network |
|---|---|---|
| `GigabitEthernet0/1` | `192.168.5.206/28` | LAN 3 |
| `GigabitEthernet0/2` | `192.168.5.222/28` | LAN 4 |
| `GigabitEthernet0/0/0` | `192.168.5.226/30` | R1–R2 |

## 6. Static Routing Configuration

Static routes explicitly define how each router reaches networks that are not directly connected.

### R1 — Routes to LAN 3 and LAN 4

```text
ip route 192.168.5.192 255.255.255.240 192.168.5.226
ip route 192.168.5.208 255.255.255.240 192.168.5.226
```

Both remote LANs are reachable through R2 at `192.168.5.226`.

### R2 — Routes to LAN 1 and LAN 2

```text
ip route 192.168.5.128 255.255.255.192 192.168.5.225
ip route 192.168.5.0 255.255.255.128 192.168.5.225
```

Both remote LANs are reachable through R1 at `192.168.5.225`.

These routes provide bidirectional Layer 3 reachability between the LANs. Each router also maintains routes to its directly connected networks.

## 7. Verification and Testing

### 7.1 Interface status

Run the following command on both routers:

```text
show ip interface brief
```

Verify that the interfaces used by the LANs and the point-to-point link have the expected IP addresses and are operational (`up/up`).

### 7.2 Routing tables

```text
show ip route
```

The expected routing-table entries include:

- `C` — directly connected networks.
- `L` — local interface addresses.
- `S` — manually configured static routes.

The evidence in `tests/shows.txt` shows that both routers have their connected networks and the expected static routes to remote LANs.

### 7.3 End-to-end connectivity

From each PC, use `ping` to test communication with devices in remote LANs. Test more than one destination to validate reachability through both routers.

For example:

```text
ping <remote-PC-IP>
```

If a ping fails, check the PC's IP address, subnet mask and default gateway, then inspect interface status and routing-table entries on both routers.

## 8. Troubleshooting Checklist

1. Confirm that each device has the correct IP address and subnet mask.
2. Verify that each PC's default gateway belongs to its local subnet.
3. Check that router interfaces are enabled and operational.
4. Confirm that the R1–R2 link uses addresses in `192.168.5.224/30`.
5. Verify the next-hop address and destination prefix of each static route.
6. Test local gateway reachability before testing remote LANs.
7. Use `show ip route` to identify missing or incorrect routes.

## 9. Key Takeaways

This lab reinforces the relationship between IP addressing and routing. VLSM provides subnet sizes that match the host requirements, while static routes allow routers to forward traffic to remote networks.

The combination of a structured addressing plan, correct next-hop selection and systematic verification is essential to building a functional routed IPv4 network.

## 10. Lab Resources

- Network topology: [`topology.png`](topology.png)
- Device configuration files: [`../configs/`](../configs/)
- Routing-table and interface evidence: [`../tests/shows.txt`](../tests/shows.txt)
- Packet Tracer project: [`../src/VLSM-and-Static-Routing.pkt`](../src/VLSM-and-Static-Routing.pkt)

## Author

**C. Victorin Vodinou**

[GitHub Profile](https://github.com/Victorin13)
[LinkedIn Profile](https://www.linkedin.com/in/victorin-vodinou/)
Gmail Prifile: victorinvodinou@gmail.com

Networking | Systems Administration | Network Security