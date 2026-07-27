# Aturcara Rasmi — Membina Aplikasi *Agentic AI* & RAG Dengan n8n

> Sumber rasmi: **Cadangan Latihan** (Training Proposal) `RAG-N8N-JPJ-101`, disediakan untuk **Jabatan Pengangkutan Jalan Malaysia (JPJ)** — versi 1.0 (2026). Modul ini **mengikut** aturcara tersebut; jangan ubah skop hari tanpa menyemaknya.
>
> **Tajuk penuh:** *Building Retrieval-Augmented Generation (RAG) & Agentic AI Applications with n8n* — membina pembantu pintar berasaskan dokumen rasmi JPJ tanpa banyak pengekodan.

## Maklumat Sesi

| Perkara | Butiran |
|---------|---------|
| **Kod Kursus** | RAG-N8N-JPJ-101 |
| **Tempoh** | 3 Hari (21 Jam) |
| **Tahap** | Permulaan (Beginner) — *tiada pengalaman pengaturcaraan diperlukan* |
| **Tarikh** | *(indikatif)* 4 – 6 Ogos 2026 (Selasa – Khamis) — untuk disahkan dengan JPJ |
| **Masa** | 9.00 pagi – 5.00 petang |
| **Mod** | Fizikal / Maya / Hibrid |
| **Anjuran** | Jabatan Pengangkutan Jalan Malaysia (JPJ) |
| **Bilangan peserta disyorkan** | 15 – 25 orang |

> **Rentak harian:** Pendaftaran & minum pagi **8.30–9.00**; sesi pagi **9.00–1.00**; rehat & makan tengah hari **1.00–2.30**; sesi petang **2.30–5.00**; bersurai **5.00 petang**. Setiap hari ~7 jam kontak.

> **Konvensyen bahasa:** Nota & penerangan dalam **Bahasa Melayu**; nama *node* n8n, kunci JSON, kod & istilah teknikal dikekalkan dalam **Bahasa Inggeris** (amalan standard industri).

---

## HARI 1 — Asas AI, LLM & RAG + n8n Pertama

**Fokus:** Faham konsep sebelum bina. Habiskan hari dengan *workflow* AI pertama yang berjalan dalam n8n.

| Masa | Agenda |
|------|--------|
| 8.30 – 9.00 pagi | Pendaftaran Peserta & Minum Pagi |
| **9.00 – 10.30 pagi** | **SESI 1: Pengenalan Kecerdasan Buatan (AI)**<br>• Evolusi AI · Machine Learning vs Generative AI<br>• Landskap AI semasa · Penggunaan AI dalam sektor awam<br>• Peluang AI dalam pengangkutan jalan & pelesenan<br>• 🧠 **Bengkel:** Kenal pasti kes guna AI dalam JPJ (pertanyaan lesen, saman, carian SOP) |
| **10.30 – 1.00 tgh** | **SESI 2: Memahami Large Language Models (LLM)**<br>• Apa itu LLM? · GPT, Claude, Gemini & model sumber terbuka (Ollama)<br>• Token & context window · Asas *prompt engineering*<br>• Halusinasi AI & batasannya<br>• 💻 **Latihan:** Bengkel *prompt engineering* guna soalan gaya-JPJ |
| 1.00 – 2.30 petang | Rehat dan Makan Tengah Hari |
| **2.30 – 3.45 petang** | **SESI 3: Memahami Retrieval-Augmented Generation (RAG)**<br>• Kenapa LLM biasa tidak mencukupi · Batasan pengetahuan model<br>• Manfaat RAG (ketepatan, petikan sumber, terkini)<br>• 👥 **Aktiviti:** Reka pembantu pengetahuan untuk bahagian anda |
| **3.45 – 4.15 petang** | **SESI 4: Embeddings & Pangkalan Data Vektor**<br>• Apa itu *embeddings*? · Carian semantik · Persamaan kosinus<br>• Qdrant, Pinecone, Weaviate, Chroma, pgvector<br>• 🔎 **Demo:** Visualisasi carian semantik |
| **4.15 – 5.00 petang** | **SESI 5: Pengenalan n8n**<br>• Apa itu n8n? · Konsep automasi *workflow* · *Nodes* & *connections*<br>• Pengurusan *credentials* · Gambaran *AI nodes*<br>• 💻 **Lab:** Bina *workflow* AI pertama (Webhook → AI → Response) |
| 5.00 petang | Bersurai |

**Hasil Hari 1:** Peserta faham AI/LLM/RAG dan sudah membina *API endpoint* berkuasa AI pertama dalam n8n.

---

## HARI 2 — Membina Penyelesaian RAG Lengkap

**Fokus:** Bina *pipeline* pengambilan dokumen (ingestion) + pengambilan semula (retrieval) → **Pembantu Pelesenan & Prosedur JPJ**.

| Masa | Agenda |
|------|--------|
| 8.30 – 9.00 pagi | Pendaftaran Peserta & Minum Pagi |
| **9.00 – 10.00 pagi** | **SESI 6: RAG Architecture Deep Dive**<br>• *Document ingestion pipeline* · Penjanaan *embedding*<br>• Proses *retrieval* · *Context injection* · *Response generation* |
| **10.00 – 11.00 pagi** | **SESI 7: Menyediakan Pangkalan Data Vektor**<br>• Pengenalan Qdrant · *Collections* & *vectors* · Simpanan *metadata*<br>• 💻 **Lab:** *Deploy* Qdrant menggunakan Docker |
| **11.00 – 1.00 tgh** | **SESI 8: Pemprosesan Dokumen & Chunking**<br>• *PDF extraction* · Pembersihan teks · Strategi *chunking* · Reka *metadata*<br>• 💻 **Latihan:** Import pekeliling, SOP & dokumen dasar JPJ |
| 1.00 – 2.30 petang | Rehat dan Makan Tengah Hari |
| **2.30 – 3.45 petang** | **SESI 9: Membina *Ingestion Workflow* dalam n8n**<br>• `File Upload → Extract Text → Chunk → Embed → Store in Qdrant`<br>• 💻 **Lab:** Bina *workflow* pengindeksan dokumen yang lengkap |
| **3.45 – 5.00 petang** | **SESI 10: Membina *Retrieval Workflow***<br>• `Question → Embed → Vector Search → Context → LLM → Answer`<br>• 💻 **Lab:** Bina **Pembantu Pengetahuan JPJ** (Projek Hari 2) |
| 5.00 petang | Bersurai |

**Hasil Hari 2:** Pembantu RAG berfungsi yang menjawab soalan lesen, pendaftaran kenderaan, saman/kompaun & SOP daripada dokumen JPJ.

---

## HARI 3 — *AI Agents*, Pengoptimuman & *Deployment*

**Fokus:** Naik taraf RAG kepada **ejen AI berbilang alat** (agentic), tingkatkan kualiti, dan *deploy* ke pengeluaran dengan pematuhan data.

| Masa | Agenda |
|------|--------|
| 8.30 – 9.00 pagi | Pendaftaran Peserta & Minum Pagi |
| **9.00 – 10.15 pagi** | **SESI 11: *AI Agents* dengan n8n**<br>• Apa itu *AI Agent*? · *Agent* vs *Workflow* · *Tool calling* · Penaakulan ejen<br>• 🔎 **Demo:** Membina ejen AI ringkas |
| **10.15 – 11.30 pagi** | **SESI 12: *AI Agents* Berbilang Alat**<br>• Menyambung banyak *tools* · Pemilihan *tool* dinamik · Integrasi API & pangkalan data<br>• 💻 **Lab:** Bina **Ejen Perkhidmatan JPJ** (carian KB + semak status lesen mock + tiket) |
| **11.30 – 1.00 tgh** | **SESI 13: Teknik RAG Lanjutan**<br>• *Metadata filtering* (ikut jenis dokumen/bahagian) · *Hybrid search*<br>• *Re-ranking* · *Parent-child* & *multi-document retrieval*<br>• 🔎 **Demo:** Banding kualiti *retrieval* antara kaedah |
| 1.00 – 2.30 petang | Rehat dan Makan Tengah Hari |
| **2.30 – 3.15 petang** | **SESI 14: Penilaian & Pengoptimuman RAG**<br>• Mengukur ketepatan · Mengurangkan halusinasi · Optimum *prompt* & *retrieval*<br>• Optimum kos · 💻 **Latihan:** Tingkatkan kualiti jawapan secara berulang |
| **3.15 – 4.15 petang** | **SESI 15: *Production Deployment***<br>• Docker · Hosting VPS/*on-premise* (residensi data) · *Cloud*<br>• Amalan keselamatan · Pemantauan & *logging* (Ollama untuk data sensitif) |
| **4.15 – 5.00 petang** | **Projek Capstone + Demo & Sijil**<br>• Peserta persembah pembantu masing-masing · Penilaian · Penyampaian sijil |
| 5.00 petang | Bersurai |

**Hasil Hari 3:** Ejen AI JPJ berbilang alat yang boleh di-*deploy*, plus projek capstone yang dibentang.

---

## Pemetaan Sesi → Deliverable

| Sesi | Deliverable / Artifak |
|------|------------------------|
| SESI 5 | `templates/workflows/01-first-ai-workflow.json` |
| SESI 9 | `templates/workflows/02-ingestion-workflow.json` |
| SESI 10 | `templates/workflows/03-retrieval-workflow.json` |
| SESI 12 | `templates/workflows/04-agent-workflow.json` |
| SESI 7, 15 | `templates/docker-compose.yml` |
| SESI 8 | `templates/sample-docs/` (dokumen JPJ contoh) |

## Kriteria Penilaian (Capstone)

| Kriteria | Wajaran |
|----------|---------|
| Reka bentuk *workflow* | 20% |
| *Document ingestion* | 20% |
| Ketepatan *retrieval* | 20% |
| Fungsi chatbot / ejen | 20% |
| Pembentangan & dokumentasi | 20% |

> Peserta yang lengkap semua latihan, implementasi RAG Hari 2, projek capstone & pembentangan akan menerima **Sijil Penyertaan** — *Building RAG & Agentic AI Applications with n8n*.
