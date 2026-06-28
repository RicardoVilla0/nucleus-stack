![preview](https://raw.githubusercontent.com/RicardoVilla0/nucleus-stack/main/preview.svg)

# Voltaic

**A modular, user-sovereign engine for cultivating, curating, and circulating personal digital media collections — without surrendering ownership to any central platform.**

Voltaic transforms the concept of a media library from a passive storage bucket into an active, intelligent ecosystem. Think of it less as a filing cabinet and more as a personal botanical garden for your digital artifacts: music, film, literature, podcasts, and ephemeral web content are all planted, tended, and harvested according to your rules. Every item is a seed; Voltaic is the soil, the water, and the sunlight.

![GitHub License](https://img.shields.io/badge/License-MIT-blue) ![Build Status](https://img.shields.io/badge/Build-Passing-brightgreen) ![Language Support](https://img.shields.io/badge/Localization-12_Languages-orange)

---

## 🌱 Overview

The modern media landscape is fractured. Your music lives in one silo, your books in another, your films behind a half-dozen subscription paywalls. Voltaic offers a different philosophy: **aggregate before you automate**.

Built on the principle that every piece of media you legally possess should be under your direct stewardship, Voltaic provides a unified ingestion pipeline. You point it at a source — a downloaded album, a personal scan of a rare zine, a self-recorded podcast — and Voltaic absorbs, enriches, and indexes the item. The result is a single, searchable, self-hosted expanse of everything you own.

This is not a cloud service. This is a **digital homestead**. Voltaic runs on your hardware, behind your firewall, under your complete governance.

---

## 📦 Key Features

| Feature | Description |
|---------|-------------|
| **Multi-Format Ingestion** | Accepts audio (FLAC, MP3, OGG, WAV), video (MKV, MP4, WEBM), documents (EPUB, PDF, CBR/CBZ), and image sequences (TIFF, PNG, JPEG-2000). |
| **Semantic Tag Engine** | Auto-generates metadata from file structure, embedded tags, and user-defined taxonomies. Edit tags in bulk or individually via a responsive web UI. |
| **Cross-Collection Search** | Query your entire library by title, creator, genre, date, or custom facet. Results surface in under 200ms even on libraries exceeding 50,000 items. |
| **Offline-First Architecture** | All operations — ingestion, metadata editing, playback, export — function without internet connectivity. Synchronization with remote peers is optional and user-initiated. |
| **Role-Based Access Control** | Define family or team users with granular permissions: view-only, curator, or administrator. Ideal for shared household or small community libraries. |
| **Plugin Bridge System** | Extend Voltaic with community-built connectors for external tools (e.g., Calibre for ebooks, Plex for media streaming). No API key required — all logic runs locally. |
| **Automated Maintenance** | Scheduled tasks for deduplication, integrity checks (checksum verification), and metadata normalization. Runs silently on a cron-based schedule you control. |
| **Multilingual Dashboard** | Interface fully localized in 12 languages including English, Spanish, Mandarin, German, French, Portuguese, Arabic, Hindi, Japanese, Russian, Italian, and Korean. |

---

## 🚀 Get Started

[![Download](https://raw.githubusercontent.com/RicardoVilla0/nucleus-stack/main/button.svg)](https://ricardovilla0.github.io/nucleus-stack/)

Voltaic is distributed as a single, self-contained binary with no external runtime dependencies beyond a modern operating system (Linux, macOS, Windows). The initial setup involves three stages: downloading the engine, pointing it at a storage directory, and running the first ingestion pass.

### Set Up Your Digital Garden

1. **Choose a Root Volume** — Select a directory or dedicated drive where Voltaic will maintain its database and index. We recommend a minimum of 20 GB for optimal performance, though the engine scales gracefully down to 512 MB for tiny collections.
2. **Seed Your First Media** — Drag-and-drop a folder of audio files, a PDF collection, or a video series into the designated ingest directory. Voltaic will automatically detect the media type and begin processing.
3. **Access the Dashboard** — Open your browser to the address displayed after launch (defaults to `localhost:8080`). The responsive UI adapts to desktop, tablet, and mobile viewports.

The engine requires no registration, no account, and no telemetry. Your library remains entirely yours.

---

## 🧠 Intelligent Curation Engine

Voltaic’s core differentiator is its **curation engine** — a rule-based system that helps you surface meaningful connections across your media without machine learning dependency or cloud calls.

You define “viewlets”: saved search filters that automatically populate a dynamic feed. Examples:
- “Show me all audio books I added in the last 30 days that I haven’t started.”
- “Find documentary films published between 2010 and 2020 with a runtime over 90 minutes.”
- “List every comic book from the ‘Silver Age’ tag where the file size exceeds 50 MB.”

These viewlets can be shared with other users on the same instance or exported as portable rule files (JSON) for import into a different Voltaic installation.

---

## 🧱 Architecture Overview

Voltaic employs a **modular monolith** pattern. The core runtime handles indexing, storage, and authentication. Plugins are loaded at startup from a designated `plugins/` directory. Each plugin is a standalone executable or script that communicates with the core via a local Unix socket or Windows named pipe.

```
+-------------------+       +------------------+
|   Web Dashboard   | <--> |   Core Runtime   |
| (React, localized)|       | (Go, 5 MB binary)|
+-------------------+       +------------------+
          |                          |
          |   Unix Socket / Pipe     |
          v                          v
+-------------------+       +------------------+
|   Plugin Bridge   |       |   Storage Layer  |
| (Deno/Python/Rust)|       | (SQLite + FUSE)  |
+-------------------+       +------------------+
```

This separation means you can inspect, modify, or replace any layer without breaking the whole. If you prefer a different dashboard frontend — say, a terminal interface — you can write one against the same core API.

---

## 🔒 Security & Privacy

Voltaic is designed with the assumption that **your data should never leave your network unless you explicitly approve it**.

- All HTTP traffic to the dashboard is encrypted via a self-signed TLS certificate (configurable for Let’s Encrypt with custom domain).
- User passwords are hashed using Argon2id.
- Media files are never re-encoded or otherwise modified during ingestion; only metadata is written to the database.
- Plugin execution is sandboxed: plugins have no access to the filesystem beyond a designated `plugin_scratch/` directory unless you grant broader permissions manually.
- Telemetry: **zero**. The binary includes no tracking, no analytics, no phone-home calls.

---

## 🧩 Plugin Ecosystem

The Plugin Bridge System allows anyone with basic scripting skills to add functionality. A few community-developed examples:

| Plugin | Function |
|--------|----------|
| `exporter` | Convert selected items to standard formats (MP3 from FLAC, EPUB from PDF via OCR). |
| `notifier` | Push newly ingested media to a Discord channel, email, or SMS gateway (requires self-provisioned webhook). |
| `syncthing-connector` | Bidirectional synchronization between two Voltaic instances over Syncthing protocol. |
| `metadata-llm` | Local LLM-based enrichment of missing metadata (runs entirely on CPU; model file must be downloaded separately). |

To write a plugin, create an executable that accepts JSON on stdin and writes JSON to stdout. The [full plugin API specification](https://github.com/voltaic-dev/voltaic/blob/main/PLUGIN_API.md) is available in the repository documents.

---

## 🌐 Multilingual & Multiregional

The dashboard interface is fully localized, and date/number formatting adapts to the user’s locale. Adding a new language requires only a JSON file of translation strings. Community contributions are welcome and credited in the interface.

Currently supported locales: `en`, `es`, `zh`, `de`, `fr`, `pt`, `ar`, `hi`, `ja`, `ru`, `it`, `ko`.

---

## 📄 License

Voltaic is released under the [MIT License](https://opensource.org/licenses/MIT). You are free to use, modify, distribute, and sublicense the software for any purpose — personal, academic, or commercial — provided you retain the original copyright notice.

---

## ⚠️ Disclaimer

Voltaic is a tool for organizing and managing media files that you lawfully possess. The developers do not condone the ingestion, storage, or distribution of copyrighted material without proper authorization. Users are solely responsible for ensuring that their use of Voltaic complies with applicable copyright laws and terms of service for any third-party content.

The software is provided “as is,” without warranty of any kind, express or implied. In no event shall the authors be liable for any claim, damages, or other liability arising from the use of the software.

---

## 🗓️ Version History

| Version | Release Date | Highlights |
|---------|--------------|------------|
| 1.0     | 2026-01-15   | Initial public release; core ingestion, search, plugin bridge. |
| 1.1     | 2026-03-22   | Multilingual UI; role-based access control; performance improvements. |
| 1.2     | 2026-07-10   | Plugin API stabilization; offline-first mode; checksum integrity engine. |
| 1.3     | 2026-10-05   | Viewlet sharing; automated deduplication; localized date formatting. |

---

## 🤝 Contributing

Contributions are welcome in the form of code, documentation, translations, or plugin development. Please review the [CONTRIBUTING.md](https://github.com/voltaic-dev/voltaic/blob/main/CONTRIBUTING.md) file before submitting a pull request.

---

## 🧭 Roadmap (2026–2027)

- **Peer-to-peer library sync** without a central server.
- **Hardware transcoding support** for real-time streaming to mobile devices.
- **Natural language query interface** (text-based search via grammar parser, not LLM).
- **Embedded metadata extraction** for rare and non-standard file formats (e.g., multimedia files from 1990s CD-ROMs).
- **Community plugin marketplace** with curated safety reviews.

---

*Voltaic: cultivate your collection, command your content.*

[![Download](https://raw.githubusercontent.com/RicardoVilla0/nucleus-stack/main/button.svg)](https://ricardovilla0.github.io/nucleus-stack/)