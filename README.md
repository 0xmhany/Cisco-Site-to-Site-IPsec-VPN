# Cisco Site-to-Site IPsec VPN

> A practical Cisco Packet Tracer project covering Site-to-Site IPsec
> VPN implementation, OSPF routing, IKE Phase 1, IPsec Phase 2, Crypto
> ACLs, Crypto Maps, troubleshooting, and final tunnel verification.

## Overview

This project implements a Site-to-Site IPsec VPN between two protected
LANs using Cisco routers in Cisco Packet Tracer.

The project covers the complete workflow:

**Network Setup → OSPF Routing → IKE Phase 1 → IPsec Phase 2 →
Interesting Traffic → Crypto Map → Troubleshooting → Final
Verification**

The documentation intentionally includes the troubleshooting stage
encountered during the lab instead of presenting only the final
successful configuration.

## Topology

``` text
PC-A ── SW1 ── R1 ───────── R2 ───────── R3 ── SW3 ── PC-C
                  \________ IPsec VPN ________/
                         R1 ↔ R3
```

### Roles

  Device        Role
  ------------- -------------------------------------------------
  R1            IPsec VPN peer / protected LAN gateway
  R2            Transit router
  R3            IPsec VPN peer / protected LAN gateway
  SW1 / SW3     LAN switches
  PC-A / PC-C   End hosts used for connectivity and VPN testing

R1 and R3 are the IPsec peers. R2 acts only as the transit router
between them.

## Protected Networks

  Network            Purpose
  ------------------ ----------------------------
  `192.168.1.0/24`   PC-A / left protected LAN
  `192.168.3.0/24`   PC-C / right protected LAN

### VPN Peer Addresses

  Peer   Address
  ------ ------------
  R1     `10.1.1.1`
  R3     `10.2.2.1`

### Transit Links

  Link     Network
  -------- ---------------
  R1--R2   `10.1.1.0/30`
  R2--R3   `10.2.2.0/30`

## Technologies & Concepts

-   Cisco IOS
-   Cisco Packet Tracer
-   Site-to-Site IPsec VPN
-   IKE / ISAKMP
-   IPsec Phase 1 and Phase 2
-   OSPF
-   Extended ACL
-   Crypto Map
-   ESP
-   AES-256
-   SHA-HMAC
-   Pre-Shared Key authentication
-   PFS
-   Security Associations (SAs)
-   Tunnel Mode

## Implementation

### 1. Network Configuration

The routers, switches, interfaces, IP addressing, and end hosts were
configured first to establish the required network topology.

### 2. OSPF Routing

OSPF was configured as the underlying routing protocol so that the
routers could learn the remote protected LANs through the transit
network.

Typical verification:

``` text
show ip ospf neighbor
show ip route
```

### 3. IKE Phase 1

IKE/ISAKMP parameters were configured between R1 and R3 to establish the
secure control channel required before IPsec data protection.

### 4. IPsec Phase 2

The IPsec transform set was configured to protect the selected traffic
using ESP with AES-256 encryption and SHA-HMAC authentication.

### 5. Interesting Traffic

An extended ACL identifies the traffic that should be protected by
IPsec:

``` text
192.168.1.0/24  <-->  192.168.3.0/24
```

Traffic matching this ACL triggers and uses the IPsec protection.

### 6. Crypto Map

The Crypto Map connects the VPN components together:

-   Remote IPsec peer
-   Interesting-traffic ACL
-   IPsec transform set
-   PFS settings

The Crypto Map is applied to the appropriate WAN-facing interfaces.

## Troubleshooting & Verification Journey

The documentation records the troubleshooting process as part of the
project.

During an initial verification attempt, basic end-to-end connectivity
could be observed while the IPsec Security Associations had not yet been
established. At that stage, the IPsec counters did not provide evidence
of encrypted traffic.

The team continued troubleshooting and testing rather than treating
successful ICMP connectivity alone as proof that the VPN was encrypting
traffic.

After the issue was resolved and the configuration was retested, the
final verification showed an established IKE Security Association and
active IPsec Security Associations.

### Final IKE Verification

``` text
show crypto isakmp sa
```

Final state included:

``` text
QM_IDLE
ACTIVE
```

### Final IPsec Verification

``` text
show crypto ipsec sa
```

The final output showed active IPsec SAs and non-zero traffic counters,
including:

``` text
#pkts encaps
#pkts encrypt
#pkts decaps
#pkts decrypt
```

The final verification also showed active ESP protection using:

``` text
ESP
AES-256
SHA-HMAC
Tunnel Mode
```

### End-to-End Test

Traffic was generated between the protected LANs:

``` text
PC-A → PC-C
```

The final test confirmed successful end-to-end communication while IPsec
counters confirmed that traffic was being processed by the IPsec SAs.

## Verification Commands

``` text
show ip ospf neighbor
show ip route
show crypto isakmp policy
show crypto ipsec transform-set
show crypto map
show crypto isakmp sa
show crypto ipsec sa
```

## Project Structure

``` text
Cisco-Site-to-Site-IPsec-VPN/
│
├── README.md
│
├── Documentation/
│   └── IPsec_VPN_Lab_Report_Team3.pdf
│
├── Presentation/
│   └── IPsec_VPN_Presentation.pptx
│
└── Lab/
    └── IPsec_VPN_Lab.pka
```

> Keep the original `.pka` filename if your Packet Tracer file has a
> different name.

## Files

### Documentation

`Documentation/IPsec_VPN_Lab_Report_Team3.pdf`

Complete technical documentation covering the topology, addressing,
routing, IPsec configuration, verification, troubleshooting,
observations, and final results.

### Presentation

`Presentation/IPsec_VPN_Presentation.pptx`

Presentation covering the main IPsec concepts, architecture,
implementation, and verification.

### Packet Tracer Lab

`Lab/IPsec_VPN_Lab.pka`

The practical Cisco Packet Tracer topology and configuration.

## Learning Outcomes

This project provided practical experience with:

-   Designing a Site-to-Site IPsec VPN
-   Configuring Cisco IOS IPsec components
-   Understanding IKE Phase 1 and IPsec Phase 2
-   Defining interesting traffic with ACLs
-   Configuring Crypto Maps
-   Using OSPF as the underlying routing protocol
-   Verifying IKE and IPsec Security Associations
-   Interpreting IPsec encapsulation and encryption counters
-   Troubleshooting VPN establishment
-   Distinguishing basic network connectivity from verified encrypted
    traffic

## Project Highlights

-   **Practical implementation:** Built and configured in Cisco Packet
    Tracer.
-   **End-to-end verification:** PC-A successfully communicated with
    PC-C.
-   **Security verification:** IPsec SAs became active and
    encrypted/decrypted packet counters were observed.
-   **Troubleshooting included:** The documentation records the problem
    encountered during the initial verification and the subsequent
    resolution.
-   **Complete deliverables:** Lab file, technical documentation, and
    presentation are included in one repository.

## Repository Purpose

This repository is intended as a practical learning and portfolio
reference for Cisco networking, Network Security, and Site-to-Site IPsec
VPN implementation.

------------------------------------------------------------------------

**Project:** Cisco Site-to-Site IPsec VPN\
**Platform:** Cisco Packet Tracer\
**Focus:** Network Security / VPN / IPsec
