# Embedded Camera Systems

An open-source technical knowledge base for **embedded and IP camera systems**, with a strong focus on **video/audio compression, real-time streaming, and system-level architecture**.

This repository documents how modern camera devices are designed and implemented — covering the complete pipeline from **image sensors and ISP**, through **video/audio encoding**, to **network transport, embedded Linux firmware, and interoperability standards such as ONVIF**.

All content is written from a **practical engineering perspective**, emphasizing real-world constraints, trade-offs, and standards-based design commonly encountered in embedded camera products.

---

## 📌 Scope

This repository focuses on:

- Embedded camera architectures
- Video compression standards (H.264 / H.265)
- Audio processing in camera systems
- Image Signal Processing (ISP)
- Streaming and network protocols
- Embedded Linux firmware
- ONVIF and interoperability standards

All content is:

- Vendor-neutral
- Standards-oriented
- Suitable for open-source learning and reference

---

## 📂 Repository Structure

```
.
├── docs
│ ├── overview # Camera system overview and architecture
│ ├── isp # Image Signal Processing pipeline
│ ├── videocodec # Video compression standards (H.264 / H.265)
│ ├── audio # Audio container and codecs
│ ├── streaming # RTP / RTSP / WebRTC
│ ├── network # Network fundamentals for cameras
│ ├── firmware # Embedded Linux and firmware topics
│ └── onvif # ONVIF standards and profiles
├── LICENSE
└── README.md

``` 


Each documentation module contains:
- A main Markdown file (`*.md`)
- An `img/` directory for diagrams and illustrations

---

## 📚 Documentation Index

### 🔹 Check list


| Module      | Path                            | Content Draft  | Technical Review | References | Examples(optional) | Status      |
| ----------- | -----------------------------   | -------------  | ---------------- | ---------- | -------- | ----------- |
| Overview    | `docs/overview/overview.md`     | ⏳             | ⏳               | ⏳         | ⏳       | In progress |
| ISP         | `docs/isp/isp.md`               | ⬜             | ⬜               | ⬜         | ⬜       | Not started |
| Video Codec | `docs/videocodec/videocodec.md` | ✅             | ✅               | ✅         | ⏳       | Completed   |
| Audio       | `docs/audio/audio.md`           | ✅             | ✅               | ✅         | ⏳       | Completed   |
| Streaming   | `docs/streaming/streaming.md`   | ⬜             | ⬜               | ⬜         | ⬜       | Not started |
| Network     | `docs/network/network.md`       | ⬜             | ⬜               | ⬜         | ⬜       | Not started |
| Firmware    | `docs/firmware/firmware.md`     | ⬜             | ⬜               | ⬜         | ⬜       | Not started |
| ONVIF       | `docs/onvif/onvif.md`           | ⬜             | ⬜               | ⬜         | ⬜       | Not started |

| Symbol | Meaning               |
| ------ | --------------------- |
| ⬜     | Not started           |
| ⏳     | In progress           |
| ✅     | Completed             |
| ⚠️     | Needs review / update |

### 🔹 [Overview](docs/overview/overview.md)

- Embedded camera system architecture
- End-to-end camera data flow
- Hardware and software building blocks

---

### 🔹 [Image Signal Processing (ISP)](docs/isp/isp.md)

- ISP pipeline overview
- AE / AWB / AF
- Noise reduction, HDR, color processing
- ISP tuning concepts

---

### 🔹 [Video Codec](docs/videocodec/videocodec.md)

- H.264 / AVC fundamentals
- H.265 / HEVC improvements
- Macroblocks, CTU, NAL units
- Encoder pipeline diagrams

---

### 🔹 [Audio](docs/audio/audio.md)

- Audio codec: compression/decompression algorithm (e.g., G.711, AAC, MP3, Opus, FLAC)
- Container / file format: packaging structure for audio data (e.g., WAV, AIFF, OGG, MP4)
- Protocol / payload format: transport mapping (e.g., RTP payloads)

---

### 🔹 [Streaming](docs/streaming/streaming.md)

- RTP / RTCP fundamentals
- RTSP signaling
- TCP vs UDP streaming
- Secure streaming concepts
- WebRTC overview for cameras

---

### 🔹 [Network](docs/network/network.md)

- Ethernet basics for cameras
- Auto-negotiation and PHY
- Multicast vs unicast
- QoS considerations

---

### 🔹 [Firmware](docs/firmware/firmware.md)

- Camera boot process
- Embedded Linux components
- Device Tree basics
- Drivers and middleware
- OTA and reliability concepts

---

### 🔹 [ONVIF](docs/onvif/onvif.md)

- ONVIF architecture
- Profiles (S, G, T, M)
- Device discovery and control
- Streaming interoperability

---

## 🎯 Target Audience

- Embedded systems engineers
- Camera firmware developers
- Video and streaming engineers
- System architects
- Technical learners and students

---

## 🤝 Contributing

Contributions are welcome.

You can help by:

- Expanding existing documents
- Adding new diagrams
- Fixing technical inaccuracies
- Adding references to standards (RFC, ITU-T, ISO)

Guidelines:

- Keep content vendor-neutral
- Prefer standards-based explanations
- Use diagrams where helpful

---

## Disclaimer

This documentation is a **technical knowledge compilation** based on:

- Public standards (RFCs, ISO/IEC, ITU-T)
- Open technical references
- Practical engineering experience and personal evaluation

While best efforts have been made to ensure technical accuracy, this content:

- Reflects the author's understanding at the time of writing
- May not cover all edge cases or vendor-specific implementations
- Should not be considered as official specification or legal advice

Readers are encouraged to consult original standards and specifications
for authoritative definitions.

---

## 📜 License

This project is licensed under the **MIT License**.

See the [LICENSE](LICENSE) file for details.