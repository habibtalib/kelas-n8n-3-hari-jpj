# Templat Workflow n8n — Peta ke Lab

Semua *workflow* n8n rujukan untuk kursus **RAG-N8N-JPJ-101**, dipetakan kepada latihan dalam setiap `hari-*/snippets/lab.md`.

> **Cara import:** dalam n8n → menu **⋯** (kanan atas) → **Import from File** → pilih fail `.json`. Atau seret fail ke atas kanvas. Selepas import, pilih *credential* (OpenAI/Qdrant) pada node berkaitan sebelum jalankan.

> ⚠️ Node AI/HTTP/app membawa **credential placeholder** — anda pilih credential sendiri selepas import. Struktur & sambungan node sudah lengkap.


> ℹ️ **Nota:** fail `.json` workflow **tidak disimpan dalam git** (dibina/diimport terus dalam n8n). Senarai di bawah ialah rujukan nama & peta ke lab; minta fail daripada fasilitator atau eksport dari n8n anda sendiri.

---

## Hari 1 — Asas n8n & Workflow AI Pertama

### Pengenalan & warm-up
| Templat | Workflow | Jenis node diajar | Guna dalam lab |
|---------|----------|-------------------|----------------|
| `00-intro-5-node-types.json` | 5 Jenis Node n8n | *(kanvas rujukan)* | Pengenalan n8n — "5 Jenis Node" |
| `asas-1-hello.json` | Asas 1 — Hello n8n | Data / Edit Fields | Warm-up (sebelum Latihan 5) |
| `asas-2-http-request.json` | Asas 2 — Panggil API | HTTP Request | Warm-up |
| `asas-3-if-branch.json` | Asas 3 — Pilihan dengan IF | Logic / branching | Warm-up |
| `asas-4-chatbot.json` | Asas 4 — Chatbot FAQ JPJ | AI (Basic LLM Chain) | Warm-up |
| `asas-5-webhook-echo.json` | Asas 5 — Webhook Echo | Trigger / Webhook | Warm-up |
| `asas-6-n8n-form.json` | Asas 6 — Borang JPJ (n8n Form) | Trigger / Form | Warm-up |

### Rujukan tutorial "Master 80% of n8n" (3 build)
| Templat | Workflow | Padanan tutorial |
|---------|----------|------------------|
| `yt-1-weather-email.json` | YT 1 — Daily Weather Email | Build 1 (Schedule → Weather → Gmail) |
| `yt-2-sponsorship-form.json` | YT 2 — Sponsorship Intake Form | Build 2 (Form → IF → Gmail + Sheets) |
| `yt-3-ai-assistant.json` | YT 3 — AI Personal Assistant | Build 3 (Chat → Agent + tools) |

### Deliverable Hari 1
| Templat | Workflow | Lab |
|---------|----------|-----|
| `01-first-ai-workflow.json` | 01 — First AI Workflow (JPJ) | **Latihan 5** (Bina Workflow AI Pertama) |

---

## Hari 2 — Penyelesaian RAG Lengkap

| Templat | Workflow | Lab |
|---------|----------|-----|
| `02-ingestion-workflow.json` | 02 — JPJ Document Ingestion | **Latihan 2** (Ingestion Workflow) |
| `03-retrieval-workflow.json` | 03 — JPJ Knowledge Assistant | **Latihan 3** (Retrieval Workflow) |

---

## Hari 3 — AI Agents

| Templat / Workflow | Nama dalam n8n | Lab |
|---------------------|----------------|-----|
| `04-agent-workflow.json` | H3 · 01 JPJ Service Agent (Multi-Tool) | **Latihan 2–4** (bina ejen berbilang alat) |
| *(dalam n8n)* | H3 · 02 Demo — Baca Data DB (Postgres) | Rujukan [`nota/11-sumber-data.md`](../../nota/11-sumber-data.md) — baca DB dalam n8n |
| *(dalam n8n)* | H3 · 03 Mock API — Status Lesen (Webhook→DB) | **Latihan 3** (endpoint untuk tool *Check Licence Status*) |

> **Data demo:** H3·02 & H3·03 membaca pangkalan data sintetik `jpjdemo` (jadual `lesen`/`pemohon`/`saman`). Butiran & credential: [`nota/11-sumber-data.md`](../../nota/11-sumber-data.md).

---

> **Nota:** `00`, `asas-*`, dan `yt-*` ialah bahan **Hari 1** (pengenalan/warm-up/rujukan tutorial). `01`–`04` ialah **deliverable teras** kursus yang dibina langkah demi langkah dalam lab. Cuba **bina sendiri** dahulu ikut lab, kemudian import templat untuk **membanding**.
