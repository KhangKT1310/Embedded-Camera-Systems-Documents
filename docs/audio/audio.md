## Basic Understanding of Audio Formats and Codecs

Version: 1.0  
Date: 01-2026  

This section provides a practical overview of common audio formats and codecs, with emphasis on identification, compression efficiency, and relevance to embedded camera systems.

---

## Audio Codecs vs Containers

It is important to distinguish between:

- **Audio codec**: compression/decompression algorithm (e.g., G.711, AAC, MP3, Opus, FLAC)
- **Container / file format**: packaging structure for audio data (e.g., WAV, AIFF, OGG, MP4)
- **Protocol / payload format**: transport mapping (e.g., RTP payloads)

Many misunderstandings in embedded systems come from mixing these concepts.

---

## File Type Identification (Magic Bytes)

Many audio formats can be identified by their initial byte signatures.

| Format | Magic Bytes (Hex) | Notes |
|------|-------------------|------|
| WAV | `52 49 46 46` ("RIFF") | RIFF container, commonly PCM audio |
| MP3 | `49 44 33` ("ID3") or `FF FB / FF F3 / FF F2` | ID3 tag is optional; MPEG frame sync may appear first |
| AAC (ADTS) | `FF F1` or `FF F9` | ADTS header for raw AAC streams |
| FLAC | `66 4C 61 43` ("fLaC") | Lossless, open-source |
| OGG Vorbis | `4F 67 67 53` ("OggS") | OGG container, Vorbis codec |
| AIFF | `46 4F 52 4D` ("FORM") | Uncompressed PCM, Apple ecosystem |
| WMA | ASF GUID-based header | No simple short magic sequence |

### Notes

- AAC inside MP4 does **not** use ADTS headers.  
- AAC over RTP typically uses LATM or MPEG-4 generic payload formats, not raw ADTS framing.
- MP3 detection should consider both ID3 tags and MPEG frame sync bits.
- WMA typically uses ASF container headers rather than short magic bytes.

---

## Special Case: MIDI

MIDI (Musical Instrument Digital Interface) is **not an audio codec**.

- MIDI files do not contain waveform audio
- They store musical events (note on/off, velocity, control messages)
- Sound generation depends on synthesizers or software instruments

As such, MIDI is **not applicable** to camera audio pipelines.

---

## Compression Efficiency: AAC vs MP3

### AAC (Advanced Audio Coding)
- Higher compression efficiency than MP3
- Better quality at the same bitrate
- Widely used in streaming and mobile ecosystems
- Common in MP4-based recording and cloud storage

### MP3 (MPEG Audio Layer III)
- Older generation codec
- Broad device compatibility
- Less efficient than AAC at low bitrates
- Still used due to legacy support

**Summary:** AAC generally provides better quality-to-bitrate performance than MP3, especially at low and medium bitrates.

---

## Codec Quality and Performance Evaluation

| Codec | Compression Efficiency | Audio Quality | Notes |
|------|------------------------|---------------|------|
| Opus | Excellent | Excellent | Designed for low-latency and packet loss resilience |
| AAC | Very good | Very good | Popular for music and recording |
| MP3 | Good | Good | Legacy codec with wide support |
| FLAC | Lossless | Excellent | Large size, not suitable for streaming |
| PCM (WAV/AIFF) | None | Excellent | Uncompressed, high bandwidth/storage |
| G.711 (μ-law / A-law) | Low | Narrowband | Log-companded PCM, fixed 64 kbps |

### Important Clarification on G.711
G.711 is **not uncompressed PCM**.  
It uses **logarithmic companding** to reduce dynamic range while maintaining simple decoding and minimal latency.

---

## Standards and References for Audio Codecs and Formats

This section lists **authoritative RFC and ISO/IEC references** for each audio codec and format discussed in this document.
Only **primary specifications** and **widely accepted standards** are included.

---

## G.711 (μ-law / A-law)

**Purpose:** Narrowband speech codec for telephony, VoIP, RTSP cameras

* **ITU-T G.711** – *Pulse Code Modulation (PCM) of Voice Frequencies*
  Defines μ-law and A-law companding algorithms.
  [https://www.itu.int/rec/T-REC-G.711](https://www.itu.int/rec/T-REC-G.711)

* **RFC 3551** – *RTP Profile for Audio and Video Conferences with Minimal Control*
  Defines RTP payload type mappings for PCMU (0) and PCMA (8).
  [https://datatracker.ietf.org/doc/html/rfc3551](https://datatracker.ietf.org/doc/html/rfc3551)

**Notes for camera systems**

* Fixed bitrate: 64 kbps
* Very low latency
* High interoperability with RTSP/NVR clients

---

## PCM / WAV

**Purpose:** Uncompressed audio, diagnostics, local recording

* **RFC 2361** – *WAVE and AVI Codec Registrations*
  Registers WAVE audio format parameters.
  [https://datatracker.ietf.org/doc/html/rfc2361](https://datatracker.ietf.org/doc/html/rfc2361)

* **RIFF Specification (Microsoft)**
  Defines RIFF container structure used by WAV.
  [https://learn.microsoft.com/en-us/windows/win32/xaudio2/resource-interchange-file-format--riff-](https://learn.microsoft.com/en-us/windows/win32/xaudio2/resource-interchange-file-format--riff-)

* **Library of Congress – RIFF/WAVE Overview**
  [https://www.loc.gov/preservation/digital/formats/fdd/fdd000025.shtml](https://www.loc.gov/preservation/digital/formats/fdd/fdd000025.shtml)

**Notes**

* WAV is a container, not a codec
* Usually contains LPCM (Linear PCM)

---

## AAC (Advanced Audio Coding)

**Purpose:** High-efficiency lossy audio for recording and streaming

* **ISO/IEC 14496-3** – *MPEG-4 Audio*
  Defines AAC-LC, HE-AAC, HE-AAC v2.
  [https://www.iso.org/standard/53943.html](https://www.iso.org/standard/53943.html)

* **RFC 3640** – *RTP Payload Format for Transport of MPEG-4 Elementary Streams*
  Commonly used for AAC over RTP/RTSP.
  [https://datatracker.ietf.org/doc/html/rfc3640](https://datatracker.ietf.org/doc/html/rfc3640)

* **ETSI TS 102 563** (optional, broadcasting context)
  AAC transport considerations.

**Notes**

* AAC in MP4 ≠ AAC ADTS
* Licensing may apply depending on distribution

---

## MP3 (MPEG Audio Layer III)

**Purpose:** Legacy lossy audio format

* **ISO/IEC 11172-3** – *MPEG-1 Audio Layer III*
  [https://www.iso.org/standard/22412.html](https://www.iso.org/standard/22412.html)

* **ISO/IEC 13818-3** – *MPEG-2 Audio Extensions*
  [https://www.iso.org/standard/26797.html](https://www.iso.org/standard/26797.html)

* **RFC 2250** – *RTP Payload Format for MPEG1/MPEG2 Audio and Video*
  [https://datatracker.ietf.org/doc/html/rfc2250](https://datatracker.ietf.org/doc/html/rfc2250)

**Notes**

* Less efficient than AAC and Opus
* Still widely supported for playback

---

## Opus

**Purpose:** Low-latency, real-time speech and music (WebRTC, talk-back)

* **RFC 6716** – *Definition of the Opus Audio Codec*
  [https://datatracker.ietf.org/doc/html/rfc6716](https://datatracker.ietf.org/doc/html/rfc6716)

* **RFC 7587** – *RTP Payload Format for the Opus Codec*
  [https://datatracker.ietf.org/doc/html/rfc7587](https://datatracker.ietf.org/doc/html/rfc7587)

* **WebRTC Audio Specification**
  [https://www.w3.org/TR/webrtc/](https://www.w3.org/TR/webrtc/)

**Notes**

* Variable bitrate, variable frame size
* Designed for packet loss resilience
* Strongly recommended for interactive camera audio

---

## FLAC (Free Lossless Audio Codec)

**Purpose:** Lossless audio storage

* **FLAC Format Specification (Xiph.Org)**
  [https://xiph.org/flac/format.html](https://xiph.org/flac/format.html)

* **RFC 9639** – *RTP Payload Format for FLAC*
  [https://datatracker.ietf.org/doc/html/rfc9639](https://datatracker.ietf.org/doc/html/rfc9639)

**Notes**

* Lossless compression
* Not suitable for real-time camera streaming due to bandwidth

---

## OGG Vorbis

**Purpose:** Open-source lossy audio format

* **RFC 3533** – *The Ogg Encapsulation Format*
  [https://datatracker.ietf.org/doc/html/rfc3533](https://datatracker.ietf.org/doc/html/rfc3533)

* **RFC 5215** – *RTP Payload Format for Vorbis Encoded Audio*
  [https://datatracker.ietf.org/doc/html/rfc5215](https://datatracker.ietf.org/doc/html/rfc5215)

* **Xiph.Org Vorbis Specification**
  [https://xiph.org/vorbis/](https://xiph.org/vorbis/)

**Notes**

* OGG is a container
* Vorbis is the codec

---

## MIDI

**Purpose:** Musical control data (not audio)

* **MIDI 1.0 Detailed Specification** – MIDI Manufacturers Association
  [https://www.midi.org/specifications](https://www.midi.org/specifications)

**Notes**

* MIDI is **not an audio codec**
* Not applicable to camera audio pipelines

---

## WMA (Windows Media Audio)

**Purpose:** Proprietary audio codec family

* **ASF Specification (Microsoft)**
  [https://learn.microsoft.com/en-us/windows/win32/wmformat/advanced-systems-format--asf--specification](https://learn.microsoft.com/en-us/windows/win32/wmformat/advanced-systems-format--asf--specification)

**Notes**

* Proprietary
* Rarely used in embedded camera systems

---

## Summary Table (Quick Reference)

| Codec / Format | Type              | Main Standard   |
| -------------- | ----------------- | --------------- |
| G.711          | Codec             | ITU-T G.711     |
| PCM / WAV      | Codec / Container | RFC 2361, RIFF  |
| AAC            | Codec             | ISO/IEC 14496-3 |
| MP3            | Codec             | ISO/IEC 11172-3 |
| Opus           | Codec             | RFC 6716        |
| FLAC           | Codec             | RFC 9639        |
| OGG Vorbis     | Container + Codec | RFC 3533        |
| MIDI           | Control protocol  | MIDI 1.0 Spec   |
| WMA            | Codec             | ASF Spec        |

---

## Why These References Matter for Camera Systems

* **RFCs** define interoperability (RTP, RTSP, WebRTC)
* **ISO/IEC** defines codec bitstreams and decoding correctness
* **ITU-T** ensures telecom compatibility
* Using standards avoids:

  * Vendor lock-in
  * Interop bugs with NVR/VMS
  * Incorrect SDP / payload assumptions

---

## Codec Ranking for Camera Systems (Practical View)

1. **Opus** – Best overall for real-time, low-latency audio and talk-back
2. **AAC** – Best for recording, storage, and cloud streaming
3. **MP3** – Legacy compatibility
4. **FLAC** – Archival, non-real-time use
5. **PCM (WAV/AIFF)** – Debugging, lab capture
6. **G.711** – Legacy RTSP, VoIP-style audio

---

## Key Takeaways for Embedded Cameras

- Codec choice must balance **latency, CPU usage, bandwidth, and interoperability**
- For talk-back: Opus or G.711
- For recording: AAC
- For diagnostics: PCM/WAV
- Containers and payload formats matter as much as the codec itself

---