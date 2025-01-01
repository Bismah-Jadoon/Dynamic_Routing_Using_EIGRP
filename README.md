

# **Enhanced Interior Gateway Routing Protocol (EIGRP)**

## **What is EIGRP?**
EIGRP is an advanced distance-vector routing protocol that combines the best features of link-state and distance-vector protocols. It’s Cisco proprietary (until recently) and designed for scalable and efficient routing in medium to large networks.

### **Key Features of EIGRP**
1. **Fast Convergence:** Uses the Diffusing Update Algorithm (DUAL) to quickly calculate backup routes.
2. **Classless Protocol:** Supports VLSM (Variable Length Subnet Mask) and CIDR (Classless Inter-Domain Routing).
3. **Reduced Bandwidth Usage:** Sends partial and bounded updates, rather than periodic full updates.
4. **Supports Multiple Protocols:** IPv4, IPv6, and other protocols.
5. **Metrics:** EIGRP uses bandwidth, delay, reliability, and load to calculate its composite metric.

---

### **EIGRP Terms**
- **Successor Route:** The best route to a destination.
- **Feasible Successor:** A backup route that meets the feasibility condition (lower advertised distance than the current best path).
- **DUAL Algorithm:** Guarantees loop-free and efficient path calculation.

---

# Configuring EIGRP in the Network

This document provides step-by-step instructions for configuring **EIGRP (Autonomous System 100)** for your topology, as illustrated in the network diagram. The configuration also includes using **passive interfaces** to secure LAN segments.

---

## **Steps to Configure EIGRP**

### 1. Enable EIGRP on All Routers

Each router must have EIGRP enabled, networks advertised, and auto-summary disabled (to support discontiguous networks).

#### **Router R1 Configuration**
```bash
R1>enable
R1#configure terminal
R1(config)#router eigrp 100
R1(config-router)#no auto-summary
R1(config-router)#network 1.0.0.0 0.0.0.3
R1(config-router)#network 3.0.0.0 0.0.0.3
```

#### **Router R2 Configuration**
```bash
R2>enable
R2#configure terminal
R2(config)#router eigrp 100
R2(config-router)#no auto-summary
R2(config-router)#network 1.0.0.0 0.0.0.3
R2(config-router)#network 2.0.0.0 0.0.0.3
R2(config-router)#network 4.0.0.0 0.0.0.3
```

#### **Router R3 Configuration**
```bash
R3>enable
R3#configure terminal
R3(config)#router eigrp 100
R3(config-router)#no auto-summary
R3(config-router)#network 2.0.0.0 0.0.0.3
R3(config-router)#network 10.1.0.0 0.0.0.255
R3(config-router)#network 10.2.0.0 0.0.0.255
```

#### **Router R4 Configuration**
```bash
R4>enable
R4#configure terminal
R4(config)#router eigrp 100
R4(config-router)#no auto-summary
R4(config-router)#network 3.0.0.0 0.0.0.3
R4(config-router)#network 4.0.0.0 0.0.0.3
R4(config-router)#network 5.0.0.0 0.0.0.3
R4(config-router)#network 6.0.0.0 0.0.0.3
```

#### **Router R5 Configuration**
```bash
R5>enable
R5#configure terminal
R5(config)#router eigrp 100
R5(config-router)#no auto-summary
R5(config-router)#network 5.0.0.0 0.0.0.3
```

#### **Router R6 Configuration**
```bash
R6>enable
R6#configure terminal
R6(config)#router eigrp 100
R6(config-router)#no auto-summary
R6(config-router)#network 6.0.0.0 0.0.0.3
```

---

### 2. Configure Passive Interfaces

Passive interfaces are used to suppress EIGRP updates on specific interfaces, usually those connecting to end devices (like switches or PCs).

#### **Router R3 Passive Interface Configuration**
To suppress updates on the GigabitEthernet interfaces connected to the switches (LAN):
```bash
R3(config)#router eigrp 100
R3(config-router)#passive-interface GigabitEthernet 0/0
R3(config-router)#passive-interface GigabitEthernet 0/1
```

This ensures that no EIGRP updates are broadcast on these interfaces, improving security and reducing unnecessary traffic.

---

### 3. Verification Commands

#### View the Routing Table
Verify that the EIGRP routes have been learned:
```bash
R3#show ip route eigrp
```

#### Check EIGRP Neighbors
Ensure EIGRP has established adjacencies with its neighbors:
```bash
R3#show ip eigrp neighbors
```

#### Check Passive Interfaces
Confirm the passive interfaces are correctly configured:
```bash
R3#show ip protocols
```

#### Test Connectivity
Ping across the network to ensure reachability:
```bash
R3#ping 10.2.0.1
```

#### Debugging EIGRP
If troubleshooting is needed, enable debugging for EIGRP:
```bash
R3#debug eigrp packets
```

---

### 4. Notes
- **Metric Calculation:** Adjust metrics (bandwidth, delay) using the `metric weights` command if needed.
- **Scalability:** EIGRP is scalable, making it suitable for large networks compared to RIP.
- **Feasible Successors:** Ensure backup paths exist by checking feasible successors in `show ip eigrp topology`.

---

### Diagram Reference

Refer to the provided diagram (`EIGRP.PNG`) for a visual representation of the network topology and its IP addressing scheme.

---

