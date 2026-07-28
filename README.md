# Kelas n8n 3 Hari — Pembantu Pintar JPJ 🚦🤖

Bahan **Latihan Membina Aplikasi *Retrieval-Augmented Generation* (RAG) & *Agentic AI* Menggunakan n8n** — disediakan untuk **Jabatan Pengangkutan Jalan Malaysia (JPJ)**. Kod kursus **RAG-N8N-JPJ-101**.

Nota dalam **Bahasa Melayu**, nama *node*/kunci JSON/istilah teknikal dalam **Bahasa Inggeris**. Sepanjang 3 hari, peserta membina **Pembantu Pintar JPJ** — pembantu AI yang menjawab soalan berdasarkan **dokumen rasmi JPJ** (Akta Pengangkutan Jalan 1987, prosedur lesen memandu, pendaftaran & pindah milik kenderaan, saman/kompaun, SOP dalaman, pekeliling) — daripada konsep AI/LLM/RAG sehingga **ejen AI berbilang alat** yang boleh di-*deploy*. Sepanjang perjalanan itu, semuanya dibina secara **hands-on dalam n8n dengan minimum pengekodan**.

> 📅 **Aturcara rasmi:** lihat [`JADUAL.md`](./JADUAL.md) — 15 sesi merentas 3 hari, waktu sebenar, deliverable. **Modul ini mengikut aturcara tersebut.**

> **Kenapa ini penting untuk JPJ:** pegawai kaunter & orang awam kerap perlukan jawapan pantas & tepat daripada pekeliling, akta & manual SOP. Pembantu RAG memberikan jawapan **berpaksikan sumber rasmi** (dengan petikan) dalam beberapa saat — mengurangkan beban kaunter, menyeragamkan jawapan antara cawangan, dan **mengelak halusinasi AI** kerana jawapan diikat pada dokumen sebenar.

## Projek: Pembantu Pintar JPJ

Sepanjang kursus, peserta membina pembantu yang boleh:

- **Jawab pertanyaan lesen** — prosedur pembaharuan Lesen Memandu (LMM/CDL/GDL), kelayakan, dokumen diperlukan
- **Terangkan pendaftaran kenderaan** — pendaftaran baharu, pindah milik, tukar hak milik
- **Panduan saman & kompaun** — cara semak & selesai kompaun tertunggak, kadar, prosedur rayuan
- **Rujuk SOP & pekeliling dalaman** — bantu pegawai baharu belajar prosedur atas permintaan
- **Bertindak sebagai ejen** *(Hari 3)* — pilih *tool* secara dinamik: cari KB, semak status lesen (mock API), cipta tiket khidmat

Setiap latihan menggunakan **dokumen contoh berkonteks-JPJ** supaya pembelajaran terus terpakai kepada keperluan sebenar jabatan.

## Ringkasan Kursus

Urutan mengikut aturcara rasmi: **Asas AI/LLM/RAG + n8n pertama** (Hari 1) → **penyelesaian RAG lengkap: ingestion + retrieval** (Hari 2) → **AI Agents, teknik lanjutan, pengoptimuman & deployment** (Hari 3).

> **Rentak harian:** Pendaftaran 8.30–9.00 · sesi pagi 9.00–1.00 · rehat 1.00–2.30 · sesi petang 2.30–5.00 · bersurai 5.00.

| Hari | Sesi | Fokus | Hasil |
|------|------|-------|-------|
| [**Hari 1**](./hari-1/) | SESI 1–5 | Evolusi AI · **LLM** (GPT/Claude/Gemini/**Ollama**), token & context window, *prompt engineering*, halusinasi · **RAG** (kenapa & manfaat) · **Embeddings & vector DB** (Qdrant, pgvector) · **Pengenalan n8n** (nodes, credentials, AI nodes) | Faham AI/LLM/RAG + *workflow* AI pertama berjalan |
| [**Hari 2**](./hari-2/) | SESI 6–10 | **RAG deep dive** · Sediakan **Qdrant** (Docker) · **Chunking & metadata** dokumen JPJ · **Ingestion workflow** (Upload→Extract→Chunk→Embed→Store) · **Retrieval workflow** (Question→Embed→Search→LLM→Answer) | Pembantu Pengetahuan JPJ berfungsi |
| [**Hari 3**](./hari-3/) | SESI 11–15 | **AI Agents** (agent vs workflow, tool calling) · **Ejen berbilang alat** (KB + status lesen + tiket) · **RAG lanjutan** (metadata filter, hybrid, re-rank) · **Penilaian & optimum** · **Deployment** (Docker, on-prem, keselamatan) + capstone | Ejen AI JPJ boleh-deploy + demo |

> **Nota:** Setiap hari **membina di atas** hari sebelumnya (kumulatif). Templat *workflow* rujukan lengkap ada di [`templates/workflows/`](./templates/workflows/). Prinsipnya sama sepanjang kursus: **AI membantu, anda memandu** — sentiasa semak jawapan yang dijana terhadap dokumen sumber, dan uji sebelum percaya.

## Nota Konsep (Latar Belakang)

Sebelum & sepanjang lab, folder [`nota/`](./nota/) mengandungi nota konsep ringkas dalam Bahasa Melayu:

- [**Buku Rujukan Utama**](./nota/00-rujukan-buku.md) 📖 — *Building Agent-Powered Applications* (V. Zvarydchuk, Packt 2026) + pemetaan bab → sesi kursus

- [**Kenapa n8n?**](./nota/01-kenapa-n8n.md) — automasi *workflow* visual, bila sesuai, berbanding kod
- [**Apa itu LLM?**](./nota/02-apa-itu-llm.md) — token, context window, GPT/Claude/Gemini/Ollama, halusinasi
- [**Apa itu RAG?**](./nota/03-apa-itu-rag.md) — kenapa LLM biasa tidak cukup, seni bina RAG
- [**Embeddings & Vector DB**](./nota/04-embeddings-vector-db.md) — carian semantik, persamaan kosinus, Qdrant
- [**Tutorial Hands-On: Qdrant DB**](./nota/12-tutorial-qdrant.md) 🟣 — belajar Qdrant secara langsung (collection, point, carian vektor, filter) dengan toy vectors
- [**Persediaan Docker**](./nota/05-setup-docker.md) 🐳 — n8n + Qdrant + PostgreSQL (+ Ollama) via `docker compose`
- [**Persediaan n8n — Cloud vs Tempatan**](./nota/10-setup-n8n.md) ☁️🐳 — dua jalan mula (Cloud atau Docker), dengan syor JPJ *(baca dahulu sebelum Hari 1)*
- [**AI Agents**](./nota/06-ai-agents.md) — ejen vs *workflow*, *tool calling*, penaakulan
- [**Prompt Engineering**](./nota/07-prompt-engineering.md) — teknik *prompt* untuk RAG & ejen
- [**Tadbir Urus & Keselamatan AI**](./nota/08-governance-keselamatan.md) 🔒 — residensi data, Ollama *on-premise*, PDPA
- [**Sumber Data — Latihan vs Pengeluaran**](./nota/11-sumber-data.md) 🗄️ — data sintetik vs sebenar; baca API/DB/fail dalam n8n; jangan *scrape* portal
- [**Deployment**](./nota/09-deployment.md) 🚀 — Docker, VPS/on-prem, pemantauan, senarai semak keluaran

## Prasyarat Peserta

- Celik komputer asas & biasa dengan aplikasi web
- **Tiada pengalaman pengaturcaraan diperlukan**
- Komputer riba dengan hak *admin* untuk memasang Docker + sambungan internet stabil

## Keperluan Sistem (Per Peserta)

- **Windows 10/11, macOS, atau Linux** dengan **Docker Desktop** (hak admin untuk pasang)
- Minimum **8GB RAM** (disyorkan; menjalankan n8n + Qdrant + Postgres dalam kontena)
- **10GB+** ruang cakera kosong (imej Docker + data vektor)
- Pelayar web moden (Chrome/Edge/Firefox)
- Kunci API **OpenAI** *(atau)* **Ollama** tempatan untuk model *on-premise*

> **Pengesahan:** Selepas pasang Docker, jalankan `docker --version` dan `docker compose version`. Langkah penuh (termasuk `docker compose up`) ada dalam [`nota/05-setup-docker.md`](./nota/05-setup-docker.md) & [Hari 2](./hari-2/).

## Susunan Teknologi (Tech Stack)

| Lapisan | Teknologi |
|---------|-----------|
| Automasi *workflow* | **n8n** |
| Penyedia LLM | **OpenAI** |
| LLM alternatif (*on-premise*/tempatan) | **Ollama** |
| Pangkalan data vektor | **Qdrant** |
| Pangkalan data | **PostgreSQL** |
| Storan fail | MinIO |
| *Deployment* | **Docker** / Docker Compose |
| Pemantauan | Grafana |
| *Reverse proxy* | Traefik |

> Untuk *deployment* JPJ yang mengendalikan data sensitif, konfigurasi **Ollama + *on-premise*** disyorkan supaya dokumen kekal dalam infrastruktur kawalan JPJ. Ini dibincang secara khusus pada **Hari 3** (lihat [`nota/08-governance-keselamatan.md`](./nota/08-governance-keselamatan.md)).

## Deliverable Latihan

- **Templat *workflow*** — `01-first-ai-workflow`, `02-ingestion-workflow`, `03-retrieval-workflow`, `04-agent-workflow` ([`templates/workflows/`](./templates/workflows/))
- **Fail *deployment*** — [`templates/docker-compose.yml`](./templates/docker-compose.yml)
- **Data contoh berkonteks-JPJ** — prosedur lesen, SOP, FAQ ([`templates/sample-docs/`](./templates/sample-docs/))
- **Nota penceramah** setiap hari (`hari-*/nota-penceramah.md`) & lab (`hari-*/snippets/lab.md`)
- **Slaid pembentangan** — dek gabungan 3 hari *Kursus Agentic AI dengan N8N* ([`slides/kursus-agentic-ai-n8n.html`](./slides/kursus-agentic-ai-n8n.html) / `.pptx`, 54 slaid), plus dek berasingan per hari ([`slides/`](./slides/))

## Sasaran Peserta (JPJ)

Pegawai Teknologi Maklumat · Pentadbir Sistem · *Business Analyst* · Pembangun · Pasukan Transformasi Digital · Pasukan Pengurusan Pengetahuan · Pegawai Kaunter & Khidmat (sebagai *power user*/penguji).

---

> ⚠️ **Penafian dokumen contoh:** Semua "dokumen JPJ" dalam [`templates/sample-docs/`](./templates/sample-docs/) adalah **contoh sintetik untuk latihan sahaja** — bukan pekeliling/SOP rasmi. Untuk penggunaan sebenar, gantikan dengan dokumen rasmi JPJ yang sah dan terkini.
