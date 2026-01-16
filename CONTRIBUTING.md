# Contributing Guide

Thank you for your interest in contributing to **Embedded Camera Systems**.

This repository is an open-source technical knowledge base focused on
**embedded camera architectures, video/audio codecs, streaming, and firmware systems**.
Contributions that improve technical accuracy, clarity, and practical value are welcome.

---

## 📌 Scope of Contributions

You can contribute in the following ways:

- Improve existing documentation
- Add new technical sections or chapters
- Add diagrams (PNG / SVG)
- Fix technical inaccuracies or outdated information
- Add references to standards (RFC, ISO/IEC, ITU-T)
- Improve wording, structure, or readability

Contributions should remain:

- Vendor-neutral
- Standards-oriented
- Focused on embedded and IP camera systems

---

## 📂 Repository Structure Overview

```

docs/
├── overview/
├── isp/
├── videocodec/
├── audio/
├── streaming/
├── network/
├── firmware/
└── onvif/

```

Each documentation module typically contains:

- One main Markdown file (`*.md`)
- An `img/` directory for diagrams and illustrations

---

## ✍️ Writing Guidelines

### 1. Technical Accuracy

- Verify all technical claims
- Prefer primary sources (RFCs, ISO/IEC, ITU-T, official specs)
- Avoid assumptions based on vendor-specific behavior

### 2. Tone and Style

- Use clear, technical, and neutral language
- Avoid marketing language
- Write from an engineering perspective
- Prefer concise explanations over long narratives

### 3. Structure (Recommended)

Each document or major section should follow this structure when possible:

``` shell

* Overview
* Background / Theory
* Practical Considerations
* Common Pitfalls
* References

```

---

## 📖 References and Citations

- Always cite authoritative sources when introducing:
  - Protocol behavior
  - Codec specifications
  - Standards-defined values
- Use direct links to:
  - IETF RFCs
  - ISO/IEC standards
  - ITU-T recommendations
- Do not copy proprietary or copyrighted content

---

## 🖼 Diagrams and Images

- Place images in the local `img/` directory of each module
- Use clear, descriptive filenames
- Prefer vector formats (PNG) when possible
- Ensure diagrams are understandable without excessive text

---

## 🧪 Examples and Code Snippets

- Examples should be minimal and illustrative
- Pseudo-code is preferred over large code dumps
- If adding code:
  - Clearly state whether it is example-only
  - Avoid vendor-specific SDK dependencies
  - Ensure licensing compatibility

---

## 🔍 Review Process

1. Fork the repository
2. Create a feature branch:

``` shell
feature/add-streaming-examples

```

3. Make your changes with clear commit messages  
4. Submit a Pull Request (PR)  

PRs will be reviewed for:

- Technical correctness
- Clarity and structure
- Consistency with repository scope

---

## 🚫 What Not to Submit

- Vendor marketing material
- Proprietary specifications
- Copy-pasted content from paid or restricted documents
- Large unrelated codebases

---

## 📜 License

By contributing to this repository, you agree that:

- Your contributions will be licensed under the **MIT License**
- You have the right to submit the content
- Your contributions do not violate third-party copyrights

---

## 💬 Questions and Discussions

If you are unsure whether a contribution fits the scope:

- Open an Issue for discussion
- Clearly describe your proposal and references

---

Thank you for helping improve this project and supporting the open-source
camera and embedded systems community.
