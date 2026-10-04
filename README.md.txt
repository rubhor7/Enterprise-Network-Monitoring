Absolutely. Copy **everything inside this single box** and paste it into your `README.md` file:

```markdown
# Enterprise Network Design & Implementation using Cisco Packet Tracer

## 📌 Project Overview

This project demonstrates the design and implementation of a small enterprise network using **Cisco Packet Tracer**.

The network is divided into multiple departmental VLANs to provide logical network segmentation. **Inter-VLAN routing** is implemented using router subinterfaces, while **OSPF** is used as the dynamic routing protocol between two routers.

The project also includes a dedicated server VLAN with **HTTP and DNS services**, along with end-to-end connectivity testing and network troubleshooting.

---

## 🎯 Objectives

- Design an enterprise-style network topology.
- Segment departments using VLANs.
- Configure 802.1Q trunking.
- Implement Inter-VLAN Routing.
- Configure OSPF dynamic routing.
- Configure HTTP and DNS services.
- Verify end-to-end network connectivity.
- Troubleshoot VLAN, trunking, routing, and connectivity issues.
- Document the complete network implementation.

---

## 🏗️ Network Topology

The network consists of:

### Routers

- **R1 – Cisco 2911**
- **R2 – Cisco 2911**

### Switches

- **SW1 – Cisco 2960** → Sales Department
- **SW2 – Cisco 2960** → Engineering Department
- **SW3 – Cisco 2960** → HR Department and Servers

### End Devices

- **PC1, PC2, PC3** → Sales
- **PC4, PC5, PC6** → Engineering
- **PC7, PC8** → HR
- **Server0** → Network Services

### Network Architecture

```text
                         +----------+
                         |    R1    |
                         | 2911     |
                         +----+-----+
                              |
                              | 10.0.0.0/30
                              | OSPF Area 0
                              |
                         +----+-----+
                         |    R2    |
                         | 2911     |
                         +----+-----+
                           |
                           |
                         SW3
                    HR + Servers
```

Departmental structure:

```text
R1
├── SW1 → Sales
│   ├── PC1
│   ├── PC2
│   └── PC3
│
└── SW2 → Engineering
    ├── PC4
    ├── PC5
    └── PC6

R2
└── SW3 → HR + Servers
    ├── PC7
    ├── PC8
    └── Server0
```

---

## 🌐 VLAN Design

| Department | VLAN | Network | Default Gateway |
|------------|------|---------|-----------------|
| Sales | 10 | 192.168.10.0/24 | 192.168.10.1 |
| Engineering | 20 | 192.168.20.0/24 | 192.168.20.1 |
| HR | 30 | 192.168.30.0/24 | 192.168.30.1 |
| Servers | 40 | 192.168.40.0/24 | 192.168.40.1 |

---

## 🔢 IP Addressing

### R1

| Interface | IP Address | Purpose |
|-----------|------------|---------|
| Gi0/0.10 | 192.168.10.1/24 | Sales Gateway |
| Gi0/1.20 | 192.168.20.1/24 | Engineering Gateway |
| Gi0/2 | 10.0.0.1/30 | R1–R2 Link |

### R2

| Interface | IP Address | Purpose |
|-----------|------------|---------|
| Gi0/0.30 | 192.168.30.1/24 | HR Gateway |
| Gi0/0.40 | 192.168.40.1/24 | Server Gateway |
| Gi0/1 | 10.0.0.2/30 | R1–R2 Link |

### End Devices

| Device | IP Address | VLAN |
|--------|------------|------|
| PC1 | 192.168.10.10 | 10 |
| PC4 | 192.168.20.10 | 20 |
| PC7 | 192.168.30.10 | 30 |
| PC8 | 192.168.30.11 | 30 |
| Server0 | 192.168.40.10 | 40 |

---

## 🔀 Routing

### Inter-VLAN Routing

Router subinterfaces are used to provide communication between departmental VLANs.

The configured gateways are:

```text
VLAN 10 → 192.168.10.1
VLAN 20 → 192.168.20.1
VLAN 30 → 192.168.30.1
VLAN 40 → 192.168.40.1
```

This allows devices in different VLANs and departments to communicate through the routers.

---

## 🌐 OSPF Dynamic Routing

**Routing Protocol:** OSPF  
**OSPF Process ID:** 1  
**Area:** 0

R1 and R2 are connected through:

```text
R1 Gi0/2 → 10.0.0.1/30
R2 Gi0/1 → 10.0.0.2/30
```

The OSPF neighbor relationship was successfully established:

```text
Neighbor ID: 2.2.2.2
State: FULL/DR
Address: 10.0.0.2
Interface: GigabitEthernet0/2
```

### Routes Learned by R1

R1 dynamically learns:

```text
192.168.30.0/24 via 10.0.0.2
192.168.40.0/24 via 10.0.0.2
```

### Routes Learned by R2

R2 dynamically learns:

```text
192.168.10.0/24 via 10.0.0.1
192.168.20.0/24 via 10.0.0.1
```

This demonstrates successful dynamic routing between the two routers.

---

## 🔗 Switching Configuration

802.1Q trunking is configured between the routers and switches.

### SW1 – Sales

```text
Fa0/1 → Trunk
Fa0/2 → VLAN 10
Fa0/3 → VLAN 10
```

### SW2 – Engineering

```text
Fa0/1 → Trunk
Fa0/2 → VLAN 20
Fa0/3 → VLAN 20
```

### SW3 – HR & Servers

```text
Fa0/1 → Trunk
Fa0/2 → VLAN 40
Fa0/3 → VLAN 30
Fa0/4 → VLAN 30
```

### Trunking

The router-facing switch ports operate using:

```text
802.1Q
```

The active VLANs are carried across the required trunk links.

---

## 🖥️ Network Services

### HTTP Web Server

Server0 is located in the dedicated Server VLAN.

```text
Server IP: 192.168.40.10
```

The HTTP service is enabled and hosts a custom webpage titled:

**Enterprise Network Design & Implementation**

---

### DNS

DNS service is configured on Server0.

DNS record:

```text
networkmonitor.local → 192.168.40.10
```

The DNS name successfully resolves from PC1.

The web server can therefore be accessed using:

```text
http://networkmonitor.local
```

---

## 🧪 Connectivity Testing

The network was tested using ICMP ping and DNS resolution.

### PC1 → Engineering

```text
PC1 → 192.168.20.10
```

Result:

```text
4 packets sent
4 packets received
0% packet loss
```

### PC1 → HR

```text
PC1 → 192.168.30.10
```

Result:

```text
4 packets sent
4 packets received
0% packet loss
```

### PC1 → Server

```text
PC1 → 192.168.40.10
```

Result:

```text
4 packets sent
4 packets received
0% packet loss
```

### PC7 → HR Gateway

```text
PC7 → 192.168.30.1
```

Result:

```text
4 packets sent
4 packets received
0% packet loss
```

### PC8 → HR Gateway

Connectivity successfully verified.

### DNS Resolution

```text
nslookup networkmonitor.local
```

Result:

```text
networkmonitor.local → 192.168.40.10
```

### Web Server Access

The following URL successfully loaded from PC1:

```text
http://networkmonitor.local
```

---

## 🛠️ Troubleshooting Performed

During implementation, several configuration issues were identified and resolved.

### VLAN and Port Mapping

The physical ports on SW3 were identified using MAC address learning and corrected:

```text
Fa0/3 → VLAN 30 → PC7
Fa0/4 → VLAN 30 → PC8
Fa0/2 → VLAN 40 → Server0
```

### SW1 Trunk

The router-facing port was identified as:

```text
Fa0/1
```

and configured as an 802.1Q trunk.

### SW2 Trunk

The router-facing port was initially operating as an access port and was corrected to:

```text
Fa0/1 → 802.1Q trunk
```

After correction, VLAN 20 traffic was successfully carried between SW2 and R1.

### Connectivity Verification

After the corrections, inter-VLAN and end-to-end connectivity tests completed successfully with **0% packet loss**.

---

## 📊 Verification Commands

The following Cisco IOS commands were used during implementation and troubleshooting:

```text
show ip interface brief
show ip ospf neighbor
show ip route
show vlan brief
show interfaces trunk
show cdp neighbors
```

End-device testing was performed using:

```text
ping
nslookup
```

---

## 🧰 Technologies Used

- Cisco Packet Tracer
- Cisco 2911 Routers
- Cisco 2960 Switches
- IPv4 Addressing
- Subnetting
- VLAN
- 802.1Q Trunking
- Inter-VLAN Routing
- Router Subinterfaces
- OSPF Dynamic Routing
- DNS
- HTTP
- ICMP Ping
- Cisco IOS CLI

---

## 🎯 Key Learning Outcomes

Through this project, I gained practical experience in:

- Designing an enterprise-style network topology.
- Creating and managing VLANs.
- Assigning switch ports to VLANs.
- Configuring 802.1Q trunk links.
- Implementing router-on-a-stick Inter-VLAN routing.
- Configuring OSPF dynamic routing.
- Understanding routing tables and OSPF-learned routes.
- Configuring basic DNS and HTTP services.
- Testing end-to-end connectivity.
- Troubleshooting VLAN and trunking issues.
- Using Cisco IOS CLI commands for network verification.

---

## 📸 Project Evidence

Configuration and testing screenshots are available in the [`screenshots`](./screenshots) folder.

The folder contains evidence for:

- Router interface status
- OSPF neighbor relationship
- R1 routing table
- R2 routing table
- SW1 VLAN configuration
- SW1 trunk configuration
- SW2 VLAN configuration
- SW2 trunk configuration
- SW3 VLAN configuration
- SW3 trunk configuration
- Inter-VLAN connectivity
- Server connectivity
- DNS name resolution
- Web server access

---

## 📂 Project Structure

```text
Enterprise-Network-Monitoring/
│
├── Enterprise_Network_Monitoring.pkt
├── README.md
│
└── screenshots/
    ├── R1_IP_Interface_Status.png
    ├── R2_IP_Interface_Status.png
    ├── R1_OSPF_Neighbor.png
    ├── R1_Routing_Table.png
    ├── R2_Routing_Table.png
    ├── SW1_VLAN_Configuration.png
    ├── SW1_Trunk_Configuration.png
    ├── SW2_VLAN_Configuration.png
    ├── SW2_Trunk_Configuration.png
    ├── SW3_VLAN_Configuration.png
    ├── SW3_Trunk_Configuration.png
    ├── PC1_to_Engineering_Ping.png
    ├── PC1_to_HR_Ping.png
    ├── PC1_to_Server_Ping.png
    ├── DNS_Name_Resolution.png
    └── Web_Server_DNS_Test.png
```

---

## 🚀 Project Highlights

This project demonstrates a complete enterprise network workflow:

```text
VLAN Segmentation
        ↓
802.1Q Trunking
        ↓
Inter-VLAN Routing
        ↓
OSPF Dynamic Routing
        ↓
DNS & HTTP Services
        ↓
End-to-End Connectivity Testing
```

The final implementation successfully connects multiple departments and a dedicated server network through a routed enterprise topology.

---

## 👩‍💻 Project Author

**Rujuta Bhor**

Electronics & Telecommunication Engineering