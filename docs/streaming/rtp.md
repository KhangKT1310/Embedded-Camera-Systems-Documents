


# Real-time Transport Protocol (RTP)
*A Technical Deep Dive for Camera and Embedded Streaming Systems*

---

## 1. Introduction

The **Real-time Transport Protocol (RTP)** is a transport-layer protocol designed for delivering **real-time audio and video** over IP networks. RTP is defined in **RFC 3550** and is widely used in IP cameras, VoIP systems, video conferencing, and streaming platforms.

In camera systems, RTP is responsible for transporting encoded media data (e.g., H.264/H.265 video, G.711/AAC audio) from the camera to clients such as NVRs, VMS software, browsers, or cloud services.

RTP itself does **not guarantee delivery, ordering, or duplication protection**. Instead, it is optimized for **low latency and real-time playback**, with quality monitoring handled by the companion protocol **RTCP**.

---

## 2. RTP in the Protocol Stack

RTP typically runs on top of **UDP**, although it can also be carried over TCP in constrained environments.

```

+---------------------------+
| Application (Camera App) |
+---------------------------+
| RTP / RTCP               |
+---------------------------+
| UDP / TCP                |
+---------------------------+
| IP                        |
+---------------------------+
| Ethernet / Wi-Fi / LTE   |
+---------------------------+

```

### Why UDP?
- No retransmission delays
- No head-of-line blocking
- Suitable for lossy but low-latency networks

---

## 3. RTP Session and Stream Concepts

### 3.1 RTP Session
An **RTP session** is a logical association among participants communicating using RTP.  
A session is identified by:
- Destination IP address
- Transport protocol (UDP/TCP)
- Destination port(s)

Typically:
- One RTP session per media type (video or audio)
- Separate RTP sessions for audio and video

### 3.2 RTP Stream
Within a session, each media source is identified by an **SSRC (Synchronization Source)**.

- One camera video encoder → one SSRC
- One camera audio encoder → one SSRC

---

## 4. RTP Packet Format

An RTP packet consists of:
- Fixed RTP header
- Optional CSRC list
- RTP payload (media data)

```

0                   1                   2                   3
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|V=2|P|X|  CC   |M|     PT      |       Sequence Number         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                           Timestamp                           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Synchronization Source (SSRC) Identifier             |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|            Contributing Source (CSRC) Identifiers              |
|                             ....                              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                           Payload                             |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

```

---

## 5. RTP Header Field Analysis

### 5.1 Version (V) – 2 bits
- Always set to **2** (current RTP version)

---

### 5.2 Padding (P) – 1 bit
- Indicates if padding bytes are added at the end of the packet
- Used mainly for encryption or block alignment
- Rarely used in camera streaming

---

### 5.3 Extension (X) – 1 bit
- Indicates the presence of an RTP header extension
- Used for vendor-specific metadata (e.g., timestamps, analytics)

---

### 5.4 CSRC Count (CC) – 4 bits
- Number of CSRC identifiers following the SSRC
- Typically **0** in camera systems
- Used in audio mixers or conferencing systems

---

### 5.5 Marker (M) – 1 bit
The **Marker bit** is profile-dependent.

In video streaming (H.264/H.265):
- `M = 1` often indicates **end of a frame**
- Helps the decoder identify frame boundaries

---

### 5.6 Payload Type (PT) – 7 bits

Identifies the format of the RTP payload.

#### Static Payload Types (RFC-defined)

| PT | Codec              | Clock Rate | Channels | Description                  |
| -- | ------------------ | ---------- | -------- | ---------------------------- |
| 0  | PCMU (G.711 µ-law) | 8000       | 1        | North American PSTN standard |
| 3  | GSM                | 8000       | 1        | GSM 06.10                    |
| 4  | G723               | 8000       | 1        | G.723.1 (obsolete)           |
| 5  | DVI4               | 8000       | 1        | ADPCM                        |
| 6  | DVI4               | 16000      | 1        | ADPCM                        |
| 7  | LPC                | 8000       | 1        | Linear Predictive Coding     |
| 8  | PCMA (G.711 A-law) | 8000       | 1        | European PSTN standard       |
| 9  | G722               | 8000*      | 1        | Wideband audio               |
| 10 | L16                | 44100      | 2        | Linear PCM                   |
| 11 | L16                | 44100      | 1        | Linear PCM                   |
| 12 | QCELP              | 8000       | 1        | Qualcomm CDMA codec          |
| 13 | CN                 | 8000       | 1        | Comfort Noise                |
| 15 | G728               | 8000       | 1        | Low-delay CELP               |
| 18 | G729               | 8000       | 1        | CS-ACELP (licensed)          |

| PT | Codec | Notes                    |
| -- | ----- | ------------------------ |
| 25 | CelB  | Obsolete video codec     |
| 26 | JPEG  | Defined in RFC 2435      |
| 28 | nv    | Legacy NeXT video format |
| 31 | H.261 | Early ITU-T video codec  |
| 32 | MPV   | MPEG-1 / MPEG-2 Video    |
| 33 | MP2T  | MPEG-2 Transport Stream  |
| 34 | H.263 | ITU-T H.263              |


#### Dynamic Payload Types
- Range: **96–127**
- Assigned via SDP during session setup

Example:
```

a=rtpmap:96 H264/90000
a=rtpmap:97 MPEG4-GENERIC/44100/2

```

In cameras, **H.264/H.265 almost always use dynamic payload types**.

---

### 5.7 Sequence Number – 16 bits
- Increments by 1 for each RTP packet
- Used by the receiver to:
  - Detect packet loss
  - Restore packet order

Example:
```

1001, 1002, 1004 → packet 1003 lost

```

---

### 5.8 Timestamp – 32 bits
- Represents the sampling instant of the first byte in the payload
- Clock rate depends on codec:
  - H.264 / H.265: **90,000 Hz**
  - Audio: codec-specific (e.g., 8 kHz, 44.1 kHz)

Timestamp is **not wall-clock time**  
Used for **playback timing and A/V sync**

---

### 5.9 SSRC (Synchronization Source) – 32 bits
- Randomly generated identifier
- Uniquely identifies a stream within an RTP session

Example:
- Video SSRC: `0x12AB34CD`
- Audio SSRC: `0x98EF2210`

---

## 6. RTP Payload: Media Encapsulation

### 6.1 General Concept
RTP payload carries **encoded media data**, not raw frames or samples.

Examples:
- H.264 NAL units
- H.265 NAL units
- AAC access units
- G.711 PCM blocks

---

### 6.2 H.264 over RTP (RFC 6184)

#### NAL Unit Types
- Single NAL Unit
- Fragmentation Unit (FU-A, FU-B)
- Aggregation Packet (STAP-A)

##### Fragmentation Example (FU-A)
Used when NAL unit > MTU (~1400 bytes):

```

[ RTP Header ][ FU Indicator ][ FU Header ][ Payload Fragment ]

```

- FU Indicator:
  - F | NRI | Type = 28
- FU Header:
  - S (Start)
  - E (End)
  - R
  - Original NAL type

Camera firmware must:
- Split large NAL units
- Set Marker bit correctly on last fragment

---

### 6.3 H.265 over RTP (RFC 7798)
Similar concept but with:
- Larger NAL unit header
- Different fragmentation rules (FU)

---

## 7. RTP Transmission in Camera Systems

### 7.1 Typical Flow

1. Sensor captures raw frame
2. Encoder compresses frame (H.264/H.265)
3. Encoder outputs NAL units
4. RTP packetizer:
   - Fragments NAL units
   - Builds RTP headers
5. Packets sent via UDP socket

---

### 7.2 MTU Considerations
- Ethernet MTU: 1500 bytes
- RTP payload size: ~1200–1400 bytes
- Avoid IP fragmentation

---

## 8. RTP vs Reliability

RTP does **not**:
- Retransmit lost packets
- Guarantee delivery order
- Handle congestion control

Instead, systems rely on:
- RTCP feedback
- Adaptive bitrate
- Error concealment in decoders

---

## 9. RTP in Practice (Camera Use Cases)

| Scenario | RTP Role |
|--------|---------|
| RTSP streaming | Media transport |
| NVR recording | High-throughput RTP |
| Multicast LAN | One-to-many RTP |
| WebRTC | RTP + SRTP |

---

## 10. Summary

RTP is the **foundation of real-time media transport** in camera systems.  
Understanding RTP at the packet, header, and payload level is critical for:

- Firmware development
- Performance optimization
- Debugging packet loss and latency
- Secure and scalable streaming design

---

## References

- RFC 3550 – RTP: A Transport Protocol for Real-Time Applications
- RFC 6184 – RTP Payload Format for H.264 Video
- RFC 7798 – RTP Payload Format for H.265 Video

---
