# RTCP Feedback and Adaptive Streaming

---

## 1. RTCP Overview

RTCP is the control channel for RTP.

Provides:

- Packet loss reporting
- Jitter statistics
- Synchronization
- Feedback messages

---

## 2. RTCP Feedback Types in WebRTC

### NACK
Retransmit lost RTP packets.

a=rtcp-fb:96 nack

---

### PLI (Picture Loss Indication)

Requests new keyframe.

a=rtcp-fb:96 nack pli

Used when:
- Packet loss too high
- Decoder lost sync

---

### FIR (Full Intra Request)

Stronger keyframe request.

---

### REMB (Receiver Estimated Max Bitrate)

a=rtcp-fb:96 goog-remb

Used for dynamic bitrate control.

---

## 3. Camera Implications

Camera must:

- Handle PLI immediately
- Support keyframe generation on demand
- Adapt bitrate when requested

Failure to implement these causes:

- Frozen video
- Long recovery after packet loss
- High latency

---

## 4. Best Practices for Camera Firmware

- Enable NACK handling
- Respond to PLI within 1 frame
- Avoid B-frames in WebRTC mode
- Keep MTU-safe RTP payload size
