
# Streaming Subsystem Overview
*Real-time Media Transport for Camera Systems*

---

## 1. Purpose of This Section

This section documents the **streaming architecture, protocols, and design decisions**
used in camera systems, ranging from **classic RTSP/RTP pipelines** to **modern WebRTC-based streaming**.

The goal is to provide:
- A **clear conceptual overview** for newcomers
- **Direct access to deep technical documents** for engineers
- A **reference structure** suitable for firmware, backend, and client developers

This documentation focuses on **real-time, low-latency streaming**, not file-based media delivery.

---

## 2. Streaming in a Camera System (High-Level View)

Typical camera streaming flow:

1. Image sensor captures raw frames
2. ISP processes frames (debayer, noise reduction, color correction)
3. Encoder compresses media (H.264 / H.265 / AAC / G.711)
4. Streaming stack packetizes and transports data
5. Client receives, decodes, and renders media

Streaming is responsible for **steps 4–5**, where real-time constraints dominate design choices.

---

## 3. Protocol Stack Overview

```

Application Layer
├─ RTSP (control, signaling)
├─ WebRTC Signaling (HTTP / WebSocket)
│
Transport / Media Layer
├─ RTP (media transport)
├─ RTCP (quality feedback)
├─ SRTP (secure media)
│
Network Layer
├─ UDP / TCP
├─ ICE / STUN / TURN (WebRTC)

```

Each layer is documented independently to keep concerns separated and modular.

---

## 4. Documentation Catalog

### 4.1 Core Media Transport

| Topic | Description |
|-----|------------|
| [`RTP`](./rtp.md) | RTP fundamentals, packet format, header fields, payload types |
| [`RTCP`](./rtcp.md) | Sender/Receiver Reports, jitter, packet loss, timing |
| [`Payload Formats`](./payload-formats.md) | H.264 / H.265 / AAC packetization over RTP |

---

### 4.2 Streaming Control & Signaling

| Topic | Description |
|-----|------------|
| [`RTSP`](./rtsp.md) | RTSP methods, SDP negotiation, session lifecycle |
| [`SDP`](./sdp.md) | Media description, codec mapping, payload negotiation |

---

### 4.3 Transport Choices

| Topic | Description |
|-----|------------|
| [`UDP vs TCP`](./transport.md) | Latency, reliability, firewall traversal |
| [`Multicast`](./multicast.md) | One-to-many streaming in LAN/NVR systems |

---

### 4.4 Security

| Topic | Description |
|-----|------------|
| [`Secure Streaming`](./security.md) | RTSPS, SRTP, DTLS, encryption models |
| [`Key Management`](./key-management.md) | SDES vs DTLS-SRTP |

---

### 4.5 WebRTC for Cameras

| Topic | Description |
|-----|------------|
| [`WebRTC Overview`](./webrtc/overview.md) | Motivation, architecture, constraints |
| [`ICE / STUN / TURN`](./webrtc/ice.md) | NAT traversal mechanics |
| [`WebRTC vs RTSP`](./webrtc/comparison.md) | Trade-offs and use cases |

---

## 5. Intended Audience

This documentation is designed for:

- **Firmware engineers**
- **Embedded Linux developers**
- **Backend / cloud engineers**
- **Client-side developers (web / mobile / VMS)**

Each document is written to be:
- RFC-aligned
- Implementation-aware
- Debug-friendly

---

## 6. Reading Guide (Suggested Order)

For new readers:

1. Streaming Overview (this document)
2. RTP Fundamentals
3. RTSP Signaling
4. Payload Formats (H.264/H.265)
5. RTCP and Quality Monitoring
6. Secure Streaming
7. WebRTC Architecture

For experienced engineers, each document can be read independently.

---

## 7. Design Philosophy

- **Low latency over reliability**
- **Explicit protocol boundaries**
- **RFC compliance first**
- **Practical camera constraints (CPU, memory, bandwidth)**

This section avoids vendor-specific APIs and focuses on **portable, protocol-level knowledge**.

---

## 8. Scope and Non-Goals

### In Scope
- Real-time streaming protocols
- Camera-to-client delivery
- Security and NAT traversal
- Performance and reliability trade-offs

### Out of Scope
- File-based streaming (HLS, DASH)
- Media editing or transcoding pipelines
- UI/UX playback details

---

## 9. References

- RFC 3550 – RTP
- RFC 3551 – RTP Profile
- RFC 6184 – H.264 over RTP
- RFC 7798 – H.265 over RTP
- RFC 8825 – WebRTC Architecture

---

