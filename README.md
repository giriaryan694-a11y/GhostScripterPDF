# GhostScripterPDF

**Client-Side PDF Action Injection & Obfuscation Engine**

🔗 **Live Instance:** [https://giriaryan694-a11y.github.io/GhostScripterPDF](https://giriaryan694-a11y.github.io/GhostScripterPDF)

*Made By Aryan Giri | giriaryan694-a11y*

---

## ⚙️ Overview

GhostScripterPDF is a browser-native payload compiler that manipulates the Cross-Reference (XRef) table and object dictionaries of PDF files to inject executable JavaScript. Built entirely on client-side Web APIs, it requires zero backend infrastructure, making it highly portable for local red team operations, CTF tooling, and controlled sandbox research.

Unlike standard HTML-to-PDF rasterizers that strip active content, this tool uses low-level PDF object manipulation to intentionally embed `/JS` (JavaScript) and `/AA` (Additional Actions) dictionaries directly into the document structure.

### Core Capabilities
*   **Zero-Trust Processing:** 100% in-memory `ArrayBuffer` manipulation. Source files never leave the local browser sandbox.
*   **Low-Level Object Mutation:** Utilizes `pdf-lib` to bypass high-level sanitizers and directly mutate `/Catalog`, `/Page`, and `/Annot` dictionaries.
*   **Client-Side Obfuscation:** Integrated `javascript-obfuscator` pipeline applies StringArray encoding, Base64 wrapping, and string splitting to bypass static YARA/AV signatures before compilation.
*   **Chrome PDFium Targeting:** Includes specialized interactive Widget injection to ensure payload execution triggers within Chrome's restrictive native PDF viewer.

----

## 🎯 Injection Vectors (ISO 32000-1)

The tool supports three distinct injection vectors, mapped to specific PDF dictionary keys. Select the vector based on the target's PDF rendering engine and your evasion requirements.

| Vector | PDF Dictionary Key | Execution Context | Target Compatibility |
| :--- | :--- | :--- | :--- |
| **Document Load** | `/Catalog /OpenAction` | Auto-executes the moment the document is parsed and rendered. High impact, but frequently flagged by modern EDRs. | Adobe Acrobat, Foxit, Nitro |
| **Page View** | `/Page /AA /O` | Triggers when the specific page containing the dictionary is drawn to the viewport. Useful for staged execution. | Adobe Acrobat, Foxit |
| **Interactive Trigger** | `/Annot /Widget /AA /D` | Binds execution to a mouse-down event on a form field. **The most reliable vector for Chrome (PDFium)** and heavily sandboxed readers. | Universal (Chrome, Edge, Safari) |

---

## ⚠️ Educational & Operational Disclaimer

GhostScripterPDF is an offensive security tool designed exclusively for authorized red teaming, penetration testing, and academic research in controlled environments. The techniques demonstrated here exploit documented features of the ISO 32000-1 (PDF) specification. 

*   **Authorization Required:** Do not use this tool to process, modify, or generate files intended for distribution to unauthorized targets, clients, or public infrastructure. 
*   **Evasion Research:** The obfuscation and injection techniques are provided to help defenders understand client-side evasion and to allow auditors to test the efficacy of endpoint detection and response (EDR) and email filtering solutions in sanctioned lab environments.
*   **Liability:** The author assumes no liability for the misuse of this software. You are responsible for ensuring compliance with all local, federal, and international laws regarding computer intrusion, payload delivery, and authorized testing.

---

## 💬 Feedback & Discussion

This is a specialized, single-purpose utility. There is no formal contribution pipeline, PR template, or feature roadmap. 

If you have found a bug, want to suggest a new ISO 32000 injection vector, or have questions about the PDF object tree manipulation, **feel free to create an issue**.

---

## 🚀 Local Deployment (Optional)

While the tool is hosted on GitHub Pages, it is entirely static. You can run it locally or air-gap it in a secure lab environment without a web server.

1. Clone the repository.
2. Open `index.html` directly in Chrome/Edge. *(Note: If using `file://` protocol, some browsers may restrict Web Workers. Use a simple local python server `python3 -m http.server` if you encounter CORS/Worker errors).*
