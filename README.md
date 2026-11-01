# High-Density Hybrid Audio-over-IP & Personal Monitor Infrastructure
### Enterprise-Grade Architecture Blueprint for Afro-Gospel & Big-Band Live Operations

![Dante Certified](https://shields.io)
![Topology](https://shields.io)
![Status](https://shields.io)

A robust, institutional-grade engineering design package outlining a fully redundant **Layer 2/3 Audio-over-IP (AoIP) Network** coupled with an integrated hardware personal monitoring matrix. This project models a high-demand, high-transient deployment scenario typical of elite **Nigerian Live Concerts and Mega-Churches (Afrobeats/Afro-Gospel)**, scaling across **34 channels at a high-fidelity 24-bit / 96kHz operational baseline**.

---

## 🛠️ System Architecture Overview

This design addresses severe spatial masking and transient management problems by bridging a primary **Dante Backplane** with localized **AES50 High-Speed Protocol Routing** to run a zero-latency, dual-mix monitoring eco-system.

### Key Infrastructure Highlights:
*   **Dual-Star Core Redundancy:** Air-gapped **Primary (VLAN 10)** and **Secondary (VLAN 20)** paths executing glitch-free hardware switching via standard managed IT infrastructure.
*   **Dante-to-AES50 Protocol Bridging:** Leveraging a **Klark Teknik DN9650 Network Converter** to split high-sample-rate audio stems from the console directly down into a **Midas HUB4 / DP48 Personal Monitor Network**.
*   **Localized Percussion Sub-Mixing:** Utilizing the dual-engine capacity of the **Midas DP48** to drive isolated, discrete stereo IEM mixes for multiple traditional players (*Gangan* and *Akuba*) from a single hardware node over one Cat6 drop.

---

## 🗺️ Logical Network & Signal Flow Diagram

```text
[ HIGH-TRANSIENT BAND INGEST ]
   │
   ├── [Vocal Section: Lead + 6 BGVs] ────────(Digital Wireless)──┐
   ├── [3-Guitar Matrix: Melody, Rhythm, Seiben] ──(Active DI/Mics)─┼─> [Stage Input Stageboxes]
   └── [Traditional Percussion Suite & Drums] ───(Transient Mics)──┘   (Yamaha Rio3224-D2 Nodes)
                                                                                  │
                ┌─────────────────────────────────────────────────────────────────┘
                │  (Primary Dante Ethernet Backbone - Blue Cat6e)
                ▼
====================================================================================================
[ NETWORK SWITCH LAYER: MANAGED PACKET ROUTING ]
====================================================================================================
   
    [ PRIMARY DANTE MANAGED SWITCH ] <─────── (PTPv1 / PTPv2 Grandmaster Clock Leader)
    (IGMP Snooping v2 Active, Hardware Querier Enabled, DSCP Egress Urgent Queue Mapping)
      │          │               │               │               │
      │(Eth)     │(Eth)          │(Eth)          │(Eth)          │(Eth)
      ▼          ▼               ▼               ▼               ▼
[Avid VENUE] [DiGiCo SD7]   [Central DSP]  [EAW Adaptive]  [Klark Teknik DN9650 Converter]
[S6L-Dante]  [MADI-Dante]   [Symetrix/BSS] [Anya Arrays]        (Dante-to-AES50 Bridging)
      ▲          ▲               ▲               ▲               │
      │(Eth)     │(Eth)          │(Eth)          │(Eth)          ▼ (AES50 Protocol Stream)
      │          │               │               │         [Midas HUB4 Stage Distributor]
    [ SECONDARY DANTE MANAGED SWITCH ] ───────────────────       │
    (Air-Gapped Network Topology / Seamless Redundancy - Red Cat6e)   ▼ (PoE / Cat5e)
                                                            [Midas DP48 Personal Mixers]
                                                            (Dual-Mix Engines: Mix A/B)

====================================================================================================
[ SYSTEM TUNING, MATRIX PROCESSING & AMBIENT SPATIALIZATION ]
====================================================================================================

 [Avid VENUE S6L Core Console] 
   │ 
   ├─── 34-Channel Matrix Routing Profiles
   │         │
   │         ▼ (Pre-Fader / Low-Latency Stems)
   └───> [Central DSP Matrix (Symetrix Radius NX)]
             │
             ├─── (Zone 1 System Tuning) ─────> [EAW Anya Adaptive Sound Array] ──> (Acoustic Steering)
             ├─── (Zone 2 Stage Monitors) ────> [Vocal/Horn Wedges]
             └─── (Zone 3 Broadcast Matrix) ──> [Recording / Multi-Track Suite via DVS]
```

---

## 📊 Core Dante Controller Configuration & Channel Schedule

This routing logic manages high-density syncopation while optimizing network traffic constraints:

| Transmitter (Source Device) | Tx Ch | Signal Type / Format | Latency Profile | Network Path | Receiver (Destination Device) | Rx Ch | Application Focus |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `Stagebox-Rio3224-01` | 01 | PCM 24b/96kHz | 0.25 ms | Primary (VLAN 10) | `Console-DiGiCo-SD7` | 01 | Lead Vocal Wireless |
| `Stagebox-Rio3224-01` | 15 | PCM 24b/96kHz | 0.25 ms | Primary (VLAN 10) | `Console-DiGiCo-SD7` | 15 | Guitar 1 (Melody) - Panned L |
| `Stagebox-Rio3224-01` | 16 | PCM 24b/96kHz | 0.25 ms | Primary (VLAN 10) | `Console-DiGiCo-SD7` | 16 | Guitar 2 (Rhythm) - Panned R |
| `Stagebox-Rio3224-01` | 17 | PCM 24b/96kHz | 0.25 ms | Primary (VLAN 10) | `Console-DiGiCo-SD7` | 17 | Guitar 3 (Seiben) - Center |
| `Stagebox-Rio3224-02` | 01 | PCM 24b/96kHz | 0.25 ms | Primary (VLAN 10) | `Console-DiGiCo-SD7` | 23 | Talking Drum (*Gangan*) |
| `Stagebox-Rio3224-02` | 02 | PCM 24b/96kHz | 0.25 ms | Primary (VLAN 10) | `Console-DiGiCo-SD7` | 24 | Akuba Drums (Low/Mid) |
| `Console-DiGiCo-SD7` | 01 | PCM 24b/96kHz | 0.50 ms | Pri & Sec (Redundant)| `Converter-DN9650`  | 01 | Stem Mix 1: Kick & Bass |
| `Console-DiGiCo-SD7` | 02 | PCM 24b/96kHz | 0.50 ms | Pri & Sec (Redundant)| `Converter-DN9650`  | 02 | Stem Mix 2: Full Percussion |
| `Console-DiGiCo-SD7` | 03 | PCM 24b/96kHz | 0.50 ms | Pri & Sec (Redundant)| `Converter-DN9650`  | 03 | Stem Mix 3: Stereo Guitars |
| `DSP-Symetrix-Core`  | 01 | PCM 24b/96kHz | 0.25 ms | Primary (VLAN 10) | `Amp-EAW-Anya-L01`  | 01 | Adaptive Left Column PA |

---

## ⚙️ Switch Configuration Optimization Protocols

To ensure glitch-free, sub-millisecond audio delivery across standard network switches, the following infrastructure scripts are applied:

1. **Multicast Packet Control:**
   * **IGMP Snooping v2** is active across all edge switches. 
   * A dedicated hardware **IGMP Querier** is established at the primary core switch to prevent multi-channel flood overhead to non-AV endpoints.

2. **Quality of Service (QoS) Priority Egress:**
   Strict Differentiated Services Code Point (DSCP) classification ensures high-priority bandwidth allocations:
   * **DSCP 56 (CS7):** Clock Synchronization / PTP (Precision Time Protocol V1/V2).
   * **DSCP 46 (EF):** Audio Stream Data Packets.
   * **DSCP 0:** Best Effort Network Control Data.

3. **In-Ear Monitor Spatialization Parameters:**
   * **The Guitar Trio Balance:** Hard-spatial separation (Guitar 1 left 30%, Guitar 2 right 30%, Guitar 3 dead-center) configured directly inside the monitor bus to eliminate sonic masking in headphones.
   * **Dual-Engine DP48 Deployment:** Mix Engine A maps isolated high-frequency transients and local spatial reverb for the *Gangan* player, while Mix Engine B runs an independent, low-frequency, pocket-locked mix for the *Akuba* player off the identical physical hardware station.

---
*Developed by Oluyomi Abayomi — Elite Audio Systems Engineer & Networked AV Integrator.*
