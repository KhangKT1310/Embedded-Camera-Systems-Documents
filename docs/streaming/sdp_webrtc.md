# SDP in WebRTC Camera Systems

---

## 1. Offer/Answer Model

Unlike RTSP, WebRTC uses:

- Offer
- Answer
- Negotiated payload types

Both peers may modify the SDP.

---

## 2. Transport Security

WebRTC SDP includes:

m=video 9 UDP/TLS/RTP/SAVPF 96

This means:

- UDP transport
- DTLS handshake
- SRTP encryption

---

## 3. ICE Attributes

a=ice-ufrag:xxxx
a=ice-pwd:yyyy
a=candidate:...

Used for NAT traversal.

---

## 4. BUNDLE

a=group:BUNDLE video audio

Audio and video share one transport.

---

## 5. RTCP Feedback

a=rtcp-fb:96 nack
a=rtcp-fb:96 nack pli
a=rtcp-fb:96 goog-remb

Used for:

- Packet retransmission
- Keyframe request
- Bitrate adaptation

---

## 6. H.264 in WebRTC

Example:

a=fmtp:96 profile-level-id=42e01f;packetization-mode=1;level-asymmetry-allowed=1

Browser compatibility requires:

- Baseline profile
- No B-frames
- No CABAC

---

## 7. Differences from RTSP

| Feature | RTSP | WebRTC |
|-----------|--------|---------|
| Security | Optional | Mandatory |
| NAT traversal | Manual | ICE |
| Encryption | Rare | Default |
| Adaptive bitrate | No | Yes |
