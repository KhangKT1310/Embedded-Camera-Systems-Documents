
# Description Protocol (SDP)
*Media Description, Codec Mapping, and Payload Negotiation for Camera Streaming*

---

## 1. Introduction

The **Session Description Protocol (SDP)** is a text-based format used to describe
**multimedia sessions** for the purpose of session negotiation, media initialization,
and capability exchange.

In camera systems, SDP is primarily used to:
- Describe **audio and video streams**
- Define **codecs and payload types**
- Negotiate **transport parameters**
- Establish **RTP session context**

SDP does **not transport media**.  
It acts as a **contract** between the camera (sender) and the client (receiver).

SDP is defined in **RFC 4566** and is widely used by:
- RTSP
- SIP
- WebRTC (via offer/answer model)

---

## 2. SDP Role in Camera Streaming

### 2.1 Where SDP Fits

Typical RTSP-based camera flow:

1. Client sends `DESCRIBE`
2. Camera responds with SDP
3. Client parses SDP
4. RTP sessions are configured based on SDP
5. Media streaming begins

SDP defines:
- **What media exists**
- **How it is encoded**
- **How it should be transported**

---

## 3. SDP Structure Overview

An SDP file is composed of **lines**, each starting with a **single-character field name**:

```

<type>=<value>

````

Example:
```sdp
v=0
o=- 1697048132 1 IN IP4 192.168.1.10
s=IP Camera Stream
t=0 0
m=video 5004 RTP/AVP 96
a=rtpmap:96 H264/90000
````

SDP is divided into:

* **Session-level fields**
* **Media-level fields**

---

## 4. Session-Level Fields

Session-level fields apply to **all media sections**.

### 4.1 Version (`v=`)

```sdp
v=0
```

* SDP version (always `0`)
* Not related to protocol or firmware version

---

### 4.2 Origin (`o=`)

```sdp
o=- 1697048132 1 IN IP4 192.168.1.10
```

Format:

```
o=<username> <session-id> <session-version> <nettype> <addrtype> <address>
```

Purpose:

* Uniquely identifies the session
* Used for session updates and caching

📌 In cameras:

* Often static or auto-generated
* Rarely updated dynamically

---

### 4.3 Session Name (`s=`)

```sdp
s=IP Camera Stream
```

Human-readable session description
No technical impact

---

### 4.4 Timing (`t=`)

```sdp
t=0 0
```

* Session is always active
* Common for live camera streaming

---

## 5. Media Descriptions (`m=`)

The **media description** is the core of SDP.

```sdp
m=<media> <port> <proto> <payload types>
```

Example:

```sdp
m=video 5004 RTP/AVP 96
```

Meaning:

* `media`: video
* `port`: 5004
* `proto`: RTP/AVP
* `payload type`: 96

Each `m=` line starts a **media section**.

---

## 6. Payload Type Mapping

### 6.1 Static Payload Types

Example:

```sdp
m=audio 5006 RTP/AVP 8
```

* Payload type `8` → PCMA (G.711 A-law)
* Codec is **implicitly known**
* No `rtpmap` required

---

### 6.2 Dynamic Payload Types

Modern codecs require **explicit mapping**.

```sdp
m=video 5004 RTP/AVP 96
a=rtpmap:96 H264/90000
```

Syntax:

```sdp
a=rtpmap:<pt> <encoding>/<clock-rate>[/channels]
```

📌 Dynamic payload types:

* Range: **96–127**
* Assigned by the sender
* Meaning defined only via SDP

---

## 7. Codec Parameter Negotiation (`fmtp`)

The `fmtp` attribute carries **codec-specific parameters**.

### 7.1 H.264 Example

```sdp
a=fmtp:96 packetization-mode=1;profile-level-id=42e01f
```

Common H.264 parameters:

* `packetization-mode`

  * `0`: single NAL unit
  * `1`: non-interleaved (most common) because some NAL type more than MTU, we need FU-A frame
  ```
  Example:
  NAL size = 5000 bytes
  MTU = 1200 byte (Maximum Transmission Uint)
  we need RTP packet fragment not using IP layer fragment 

    FU-A packet #1 (S=1, E=0)
    FU-A packet #2 (S=0, E=0)
    FU-A packet #3 (S=0, E=0)
    FU-A packet #4 (S=0, E=1)

  ```
* `profile-level-id`

  * Encodes profile + level constraints
  ```
  profile-level-id = XX YY ZZ   (hex)

    XX → profile_idc
    YY → constraint_set_flags
    ZZ → level_idc

    Example:
    
    42 e0 1f
    │  │  └─ Level
    │  └──── Constraints
    └─────── Profile

  ```

📌 Mismatch here is a **common cause of black video**.

---

### 7.2 H.265 Example

```sdp
a=rtpmap:98 H265/90000
a=fmtp:98 sprop-vps=...;sprop-sps=...;sprop-pps=...
```

Used to signal:

* VPS / SPS / PPS
* Decoder initialization data

---

## 8. Media Direction Attributes

```sdp
a=sendonly
a=recvonly
a=sendrecv
a=inactive
```

Camera usage:

* Camera → client: `sendonly`
* Client → camera (audio talkback): `recvonly`

---

## 9. Multiple Media Streams

Typical camera SDP:

```sdp
m=video 5004 RTP/AVP 96
a=rtpmap:96 H264/90000

m=audio 5006 RTP/AVP 8
```

Each media section:

* Has its own RTP session
* Own payload types
* Own transport ports

---

## 10. Transport Protocols in SDP

### 10.1 RTP over UDP

```sdp
m=video 5004 RTP/AVP 96
```

### 10.2 RTP over TCP (RTSP interleaved)

```sdp
m=video 0 RTP/AVP/TCP 96
a=interleaved=0-1
```

Used when:

* Firewall blocks UDP
* Mobile networks
* Cloud access

---

## 11. SDP in WebRTC (Brief Note)

WebRTC uses SDP differently:

* Offer/Answer model
* ICE candidates embedded
* DTLS-SRTP keys negotiated

However, **media-level concepts remain identical**:

* Payload types
* `rtpmap`
* `fmtp`
* Direction attributes

---

## 12. Common SDP Pitfalls in Camera Systems

| Issue                    | Root Cause                |
| ------------------------ | ------------------------- |
| Black video              | Wrong `profile-level-id`  |
| No audio                 | Payload type mismatch     |
| Choppy video             | Clock rate mismatch       |
| One-way audio            | Direction attribute error |
| RTSP works, WebRTC fails | Missing codec constraints |

---

## 13. Engineering Guidelines

* Treat SDP as a **strict contract**
* Never assume payload type meaning without `rtpmap`
* Validate SDP before starting RTP
* Log SDP in firmware for debugging
* Keep SDP minimal but explicit

---

## 14. Summary

SDP is the **foundation of interoperability** in camera streaming systems.

Correct SDP handling ensures:

* Codec compatibility
* Proper RTP packetization
* Predictable client behavior
* Easier debugging and maintenance

A well-formed SDP is often the difference between
*“it streams everywhere”* and *“it only works on our player”*.

---

## References

* RFC 4566 – Session Description Protocol
* RFC 3550 – RTP
* RFC 6184 – H.264 over RTP
* RFC 7798 – H.265 over RTP
