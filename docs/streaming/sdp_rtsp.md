# SDP in RTSP-Based Camera Systems

---

## 1. Role of SDP in RTSP

In RTSP:

1. Client sends DESCRIBE
2. Server responds with SDP
3. Client configures RTP sessions

SDP is static and server-generated.

---

## 2. Example RTSP SDP

v=0
o=- 1697048132 1 IN IP4 192.168.1.10
s=IP Camera Stream
t=0 0
m=video 5004 RTP/AVP 96
a=rtpmap:96 H264/90000
a=fmtp:96 packetization-mode=1;profile-level-id=42e01f

---

## 3. Transport Modes

### UDP
m=video 5004 RTP/AVP 96

### TCP Interleaved
m=video 0 RTP/AVP/TCP 96

---

## 4. Limitations of RTSP SDP

- No ICE
- No DTLS
- No encryption
- No adaptive feedback

Primarily used in LAN / NVR systems.
