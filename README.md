# VLSM and Static Routing

## Overview

This project is a small Cisco Packet Tracer lab designed to practice
IPv4 subnetting using VLSM and static routing.

The topology consists of:

- 2 Cisco routers
- 4 LANs
- 4 PCs
- 4 Cisco Catalyst 2960 switches
- 1 point-to-point link between the routers

## Objectives

- Subnet the 192.168.5.0/24 network using VLSM
- Allocate an appropriate subnet to each LAN
- Configure IPv4 addresses on routers and PCs
- Configure a /30 point-to-point network between R1 and R2
- Configure static routes
- Verify end-to-end connectivity

## Network Requirements

| Network | Hosts Required |
|---------|----------------|
| LAN1    | 45             |
| LAN2    | 64             |
| LAN3    | 14             |
| LAN4    | 9              |
| R1-R2   | 2              |

## Repository Structure

```text
.
├── configs/
│   └── Device configuration files (.txt)
├── docs/
│   ├── documentation.md
│   └── topology.png
├── src/
│   └── VLSM-and-Static-Routing.pkt
├── tests/
│   ├── simulation-mode.png
│   ├── end-to-end-connectivity.png
│   ├── shows.txt
├── .gitignore
├── LICENSE
└── README.md
```

## Author

**C. Victorin Vodinou**

[GitHub Profile](https://github.com/Victorin13)
[LinkedIn Profile](https://www.linkedin.com/in/victorin-vodinou/)
Gmail Prifile: victorinvodinou@gmail.com

Networking | Systems Administration | Network Security

## License

This project is distributed under the MIT License. See [`LICENSE`](LICENSE) for details.