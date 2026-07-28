# Hari 2 — Membina Penyelesaian RAG Lengkap

Panduan langkah demi langkah untuk hari kedua kursus **Building Retrieval-Augmented Generation (RAG) & Agentic AI Applications with n8n** (kod kursus **RAG-N8N-JPJ-101**), disediakan untuk **Jabatan Pengangkutan Jalan Malaysia (JPJ)**. Nota ini mengikut **aturcara rasmi SESI 6–10** — lihat [`JADUAL.md`](../JADUAL.md) — bukan susunan bebas.

Projek kursus: **Pembantu Pintar JPJ** — pembantu AI yang menjawab soalan berdasarkan **dokumen JPJ** (prosedur lesen memandu LMM/CDL/GDL, pendaftaran & pindah milik kenderaan, saman/kompaun, SOP dalaman). Hari 1 anda faham konsep AI/LLM/RAG dan bina *workflow* AI pertama. **Hari ini** anda bina penyelesaian RAG **lengkap** — dari memasukkan dokumen (*ingestion*) sehingga menjawab soalan berpaksikan sumber (*retrieval*).

> **Nota untuk pemula:** Anda tidak perlu tahu mengekod. Setiap langkah diterangkan perlahan-lahan — termasuk **kenapa** setiap komponen wujud, bukan sekadar cara menyusun *node*. Semua dibina secara visual dalam n8n.

> **Konvensyen bahasa:** Penerangan dalam **Bahasa Melayu**; nama *node* n8n, kunci JSON, istilah teknikal (*embedding*, *chunk*, *collection*) dikekalkan dalam **Bahasa Inggeris** — amalan standard industri yang kita ikut sepanjang kursus.

> **Cara guna nota ini:** Bahagian di sini menerangkan **konsep** — kenapa sesuatu wujud dan apa hasilnya. Latihan hands-on **langkah demi langkah** ada di [`snippets/lab.md`](./snippets/lab.md). Baca bahagian konsep yang sepadan di sini dahulu, kemudian pindah ke lab untuk buat sendiri.

---

> 📖 **Bacaan lanjut (buku rujukan):** *Building Agent-Powered Applications* (V. Zvarydchuk, Packt 2026) — **Bab 6** (RAG: komponen, semantic retrieval, document preprocessing, ANN, vector databases, hybrid & RAG-as-a-service). Pemetaan penuh: [`../nota/00-rujukan-buku.md`](../nota/00-rujukan-buku.md).

## Fokus Hari Ini

| Topik | Rujukan rasmi |
|-------|----------------|
| RAG architecture (ingestion vs retrieval) | [docs.n8n.io — RAG in n8n](https://docs.n8n.io/advanced-ai/rag-in-n8n/) |
| Qdrant (vector database) | [qdrant.tech/documentation](https://qdrant.tech/documentation/) |
| Qdrant collections & distance | [qdrant.tech — Collections](https://qdrant.tech/documentation/concepts/collections/) |
| Qdrant Vector Store node | [docs.n8n.io — Qdrant Vector Store](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.vectorstoreqdrant/) |
| Embeddings OpenAI node | [docs.n8n.io — Embeddings OpenAI](https://docs.n8n.io/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.embeddingsopenai/) |
| Default Data Loader | [docs.n8n.io — Default Data Loader](https://docs.n8n.io/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.documentdefaultdataloader/) |
| Recursive Character Text Splitter | [docs.n8n.io — Text Splitters](https://docs.n8n.io/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.textsplitterrecursivecharactertextsplitter/) |
| Question and Answer Chain | [docs.n8n.io — Q&A Chain](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.chainretrievalqa/) |
| Model *embedding* & dimensi | [platform.openai.com — Embeddings](https://platform.openai.com/docs/guides/embeddings) |
| n8n (umum) | [n8n.io/docs](https://docs.n8n.io/) |

---

## Jadual Hari Ini

| Masa | Agenda |
|------|--------|
| 8.30 – 9.00 pagi | Pendaftaran Peserta & Minum Pagi |
| **9.00 – 10.00 pagi** | **SESI 6: RAG Architecture Deep Dive** — *document ingestion pipeline*, penjanaan *embedding*, proses *retrieval*, *context injection*, *response generation* |
| **10.00 – 11.00 pagi** | **SESI 7: Menyediakan Pangkalan Data Vektor** — pengenalan Qdrant, *collections* & *vectors*, simpanan *metadata*, 💻 **Lab:** *deploy* Qdrant guna Docker |
| **11.00 – 1.00 tgh** | **SESI 8: Pemprosesan Dokumen & Chunking** — *PDF extraction*, pembersihan teks, strategi *chunking*, reka *metadata*, 💻 **Latihan:** import dokumen JPJ contoh |
| 1.00 – 2.30 petang | Rehat dan Makan Tengah Hari |
| **2.30 – 3.45 petang** | **SESI 9: Membina *Ingestion Workflow*** — `File Upload → Extract Text → Chunk → Embed → Store in Qdrant`, 💻 **Lab:** bina *workflow* pengindeksan lengkap |
| **3.45 – 5.00 petang** | **SESI 10: Membina *Retrieval Workflow*** — `Question → Embed → Vector Search → Context → LLM → Answer`, 💻 **Lab:** bina **Pembantu Pengetahuan JPJ** |
| 5.00 petang | Bersurai |

**Hasil Hari 2:** Pembantu RAG berfungsi yang menjawab soalan lesen, pendaftaran kenderaan, saman/kompaun & SOP daripada dokumen JPJ — dengan jawapan **berpaksikan sumber**.

---

## Persediaan (Sebelum 9.00 Pagi / Pra-syarat)

Persediaan **bukan** item agenda rasmi tetapi anda perlukan persekitaran yang **berjalan** sebelum kelas. Selesaikan sebelum SESI 6:

1. **Docker Desktop** dipasang & berjalan (dari Hari 1 / [`nota/05-setup-docker.md`](../nota/05-setup-docker.md)). Sahkan:

   ```bash
   docker --version
   docker compose version
   ```

2. **n8n** sudah boleh dibuka (dari stack Docker Hari 1) di `http://localhost:5678`.
3. **Kunci API OpenAI** sudah dimasukkan sebagai *credential* dalam n8n (untuk model *embedding* & *chat*). Sebagai alternatif *on-premise*, **Ollama** boleh digunakan — kita bincang posture data sensitif ini di **Hari 3**.

> Fail `docker compose` rasmi kursus ada di [`../templates/docker-compose.yml`](../templates/docker-compose.yml) — ia menyediakan **n8n + Qdrant + PostgreSQL (+ Ollama)** sekali gus. Kita **hidupkan Qdrant** daripada fail yang sama dalam SESI 7.

---

## SESI 6 (9.00 – 10.00) — RAG Architecture Deep Dive

Hari 1 anda belajar **kenapa** RAG diperlukan: LLM biasa hanya "tahu" apa yang ada dalam data latihannya, tidak tahu pekeliling & SOP dalaman JPJ, dan boleh **berhalusinasi** (reka jawapan yang kedengaran yakin tetapi salah). RAG selesaikan ini dengan **memberi LLM dokumen sebenar** untuk dirujuk sebelum menjawab. Hari ini kita bedah **bagaimana** ia berfungsi, komponen demi komponen.

### Dua fasa RAG: Ingestion & Retrieval

RAG sebenarnya **dua *pipeline* berasingan** yang berkongsi satu pangkalan data vektor. Ramai pemula keliru kerana menganggap ia satu aliran — sebenarnya ia dua:

- **Ingestion (pengindeksan)** — dijalankan **sekali** (atau bila dokumen berubah). Ia baca dokumen JPJ, potong kepada kepingan kecil, tukar setiap kepingan kepada *vector*, dan simpan dalam Qdrant. Inilah **SESI 9**.
- **Retrieval (menjawab)** — dijalankan **setiap kali** ada soalan. Ia tukar soalan kepada *vector*, cari kepingan paling serupa dalam Qdrant, suntik kepingan itu ke dalam *prompt*, dan minta LLM menjawab. Inilah **SESI 10**.

Analogi: *Ingestion* seperti **menyusun & mengindeks fail** dalam kabinet — kerja sekali di awal. *Retrieval* seperti **pegawai kaunter yang mencari fail yang betul** setiap kali ada pertanyaan awam.

### Pipeline Ingestion (SESI 9)

```
┌─────────┐   ┌──────────────┐   ┌──────────┐   ┌───────────┐   ┌──────────────────┐
│   PDF   │──▶│    Text      │──▶│ Chunking │──▶│ Embedding │──▶│  Vector Database │
│ (dokumen│   │  Extraction  │   │ (potong  │   │ (tukar ke │   │     (Qdrant)     │
│   JPJ)  │   │ (baca teks)  │   │  kepingan)│  │  vector)  │   │  simpan vektor + │
└─────────┘   └──────────────┘   └──────────┘   └───────────┘   │    metadata      │
                                                                └──────────────────┘
```

1. **PDF / Text Extraction** — baca teks daripada fail (PDF, DOCX, TXT). Yang kita mahu ialah **teks bersih**, bukan susun atur.
2. **Chunking** — potong dokumen panjang kepada kepingan (*chunk*) kecil yang bermakna. (Kenapa? — SESI 8.)
3. **Embedding** — tukar setiap *chunk* teks kepada **senarai nombor** (*vector*) yang mewakili maknanya.
4. **Vector Database (Qdrant)** — simpan setiap *vector* bersama teks asal & **metadata** (nama dokumen, jenis, muka surat) supaya boleh dicari semula.

### Pipeline Retrieval (SESI 10)

```
┌──────────┐   ┌───────────┐   ┌──────────────┐   ┌──────────────┐   ┌─────┐   ┌────────┐
│  Soalan  │──▶│ Embedding │──▶│ Vector Search │─▶│   Retrieve   │──▶│ LLM │──▶│ Jawapan│
│ pengguna │   │ (tukar ke │   │  (cari top-k  │   │   Context    │   │     │   │ (dengan│
│          │   │  vector)  │   │  paling serupa)│  │ (kepingan +  │   │     │   │ petikan│
│          │   │           │   │   dalam Qdrant)│  │  metadata)   │   │     │   │ sumber)│
└──────────┘   └───────────┘   └──────────────┘   └──────────────┘   └─────┘   └────────┘
```

1. **Soalan → Embedding** — soalan pengguna ditukar kepada *vector* menggunakan **model *embedding* yang SAMA** seperti fasa ingestion (ini kritikal — dua model berbeza menghasilkan ruang vektor tidak serasi).
2. **Vector Search** — Qdrant cari *chunk* yang *vector*-nya **paling hampir** dengan *vector* soalan (persamaan kosinus). Kita ambil **top-k** (contoh: 4) yang paling relevan.
3. **Retrieve Context** — teks *chunk* teratas dikeluarkan sebagai **konteks**.
4. **Context Injection** — konteks disuntik ke dalam *prompt* LLM, biasanya berbentuk: *"Berdasarkan petikan dokumen berikut sahaja, jawab soalan ini... [petikan] ... Soalan: [soalan]"*.
5. **Response Generation** — LLM menjawab **berpaksikan** konteks yang diberi, dengan petikan sumber — inilah yang mengelakkan halusinasi.

> **Kenapa dua fasa berkongsi satu Qdrant?** Fasa *ingestion* **menulis** ke Qdrant; fasa *retrieval* **membaca** daripadanya. Selagi kedua-dua guna **model embedding & collection yang sama**, mereka "bercakap bahasa vektor yang sama". Ini konsep paling penting hari ini — kita ulang berkali-kali.

Rujukan konsep penuh: [`../nota/03-apa-itu-rag.md`](../nota/03-apa-itu-rag.md) & [`../nota/04-embeddings-vector-db.md`](../nota/04-embeddings-vector-db.md).

---

## SESI 7 (10.00 – 11.00) — Menyediakan Pangkalan Data Vektor (Qdrant)

### Apa itu Qdrant?

**Qdrant** ialah **vector database** — pangkalan data yang direka khas untuk menyimpan & mencari *vector*. Pangkalan data biasa (seperti PostgreSQL) sangat baik mencari nilai **tepat** (`WHERE no_lesen = 'D1234567'`), tetapi lemah menjawab *"cari dokumen yang **maknanya serupa** dengan soalan ini"*. Itulah kepakaran Qdrant — **carian semantik** (*semantic search*).

Kita pilih Qdrant kerana: (1) **percuma & sumber terbuka**, (2) boleh dijalankan **sepenuhnya *on-premise*** dalam Docker — penting untuk data JPJ yang sensitif (rujuk [`../nota/08-governance-keselamatan.md`](../nota/08-governance-keselamatan.md)), dan (3) ada *node* siap sedia dalam n8n.

### Konsep: Collection, Point, Vector, Payload

| Istilah Qdrant | Maksud | Analogi kabinet fail |
|----------------|--------|----------------------|
| **Collection** | Bekas untuk satu set vektor yang berkaitan | Satu laci kabinet (cth. laci "SOP JPJ") |
| **Point** | Satu rekod: `id` + `vector` + `payload` | Satu fail dalam laci |
| **Vector** | Senarai nombor (cth. 1536 nombor) yang mewakili makna teks | "Cap jari" makna dokumen |
| **Payload** | Metadata + teks asal yang disimpan bersama vektor | Label & kandungan fail |

Dua tetapan **collection** yang mesti anda betulkan dari awal — salah satu ini adalah punca ralat paling biasa hari ini:

1. **Vector size (dimensi)** — mesti **sepadan** dengan model *embedding*. Model `text-embedding-3-small` OpenAI menghasilkan vektor **1536 dimensi**, jadi *collection* mesti dicipta dengan `size: 1536`. Jika tak sepadan, Qdrant tolak data.
2. **Distance metric** — cara Qdrant ukur "keserupaan". Untuk *embedding* OpenAI, guna **Cosine** (persamaan kosinus). Ini mesti sama antara ingestion & retrieval.

```
Collection "jpj_documents"
├─ size: 1536          ◀── mesti sama dengan dimensi model embedding
├─ distance: Cosine    ◀── cara ukur keserupaan
└─ points:
   ├─ Point { id: 1, vector: [0.021, -0.44, ...1536 nombor], payload: { text: "...", doc_type: "lesen" } }
   ├─ Point { id: 2, vector: [...], payload: { text: "...", doc_type: "saman" } }
   └─ ...
```

### Simpanan metadata & optimum carian

*Payload* (metadata) bukan sekadar label — ia membolehkan **penapisan** (*filtering*). Contohnya, anda boleh minta Qdrant *"cari chunk paling serupa **TETAPI hanya** yang `doc_type = 'lesen'`"*. Ini menjadikan carian lebih tepat & pantas. Kita reka *metadata* dengan teliti dalam SESI 8, dan guna penapis ini sebagai **Cabaran** dalam lab.

### Lab SESI 7: Deploy Qdrant guna Docker

Fail [`../templates/docker-compose.yml`](../templates/docker-compose.yml) sudah mengandungi servis `qdrant`. Menghidupkannya semudah:

```bash
docker compose up -d
```

Selepas Qdrant naik, buka **dashboard** dalam pelayar:

```
http://localhost:6333/dashboard
```

Dashboard ini ialah alat visual untuk melihat *collection*, bilangan *point*, dan menguji carian — kita akan kembali ke sini selepas ingestion untuk **mengesahkan** dokumen benar-benar tersimpan. Langkah penuh: [Latihan 0 & 1 dalam lab](./snippets/lab.md).

> **Nota port:** Qdrant dedah **6333** (REST API + dashboard) dan **6334** (gRPC). Dari dalam kontena n8n, Qdrant dicapai melalui nama servis (`http://qdrant:6333`), bukan `localhost` — kerana setiap kontena ada "localhost" sendiri. Kita jelaskan ini semasa menyambung *credential* dalam SESI 9.

---

## SESI 8 (11.00 – 1.00) — Pemprosesan Dokumen & Chunking

Ini sesi paling panjang hari ini kerana **kualiti chunking menentukan kualiti jawapan**. Dokumen yang di-*chunk* dengan buruk menghasilkan RAG yang buruk — walaupun LLM & Qdrant sempurna.

### PDF extraction & pembersihan teks

Dokumen JPJ selalunya PDF. Langkah pertama ialah **mengekstrak teks** daripadanya. Perkara yang perlu diberi perhatian:

- **Teks vs imej** — PDF yang **diimbas** (*scanned*) sebenarnya imej; ia perlukan OCR. PDF yang ditaip (teks sebenar) boleh diekstrak terus. Dokumen contoh kursus adalah teks sebenar.
- **Pembersihan (*text cleaning*)** — buang *header*/*footer* berulang, nombor muka surat, ruang kosong berlebihan, dan aksara pelik. Teks yang bersih menghasilkan *embedding* yang lebih tepat.

### Kenapa perlu *chunking*?

**Kenapa tidak simpan satu dokumen penuh sebagai satu vektor sahaja?** Tiga sebab:

1. **Ketepatan carian** — satu *embedding* untuk keseluruhan dokumen 20 muka surat menjadi "purata kabur" semua topik. Satu *chunk* fokus (cth. "prosedur pembaharuan lesen") menghasilkan vektor yang **tajam** dan mudah dipadankan dengan soalan.
2. **Had konteks LLM** — kita hanya suntik beberapa *chunk* ke dalam *prompt*, bukan seluruh perpustakaan. *Chunk* kecil membolehkan kita muatkan **beberapa** petikan relevan.
3. **Petikan sumber** — jawapan boleh menunjuk **tepat** ke bahagian mana dokumen, bukan "di suatu tempat dalam dokumen 20 muka surat".

### Strategi chunking: saiz & pertindihan (*overlap*)

Dua parameter utama:

- **Chunk size** — berapa besar setiap kepingan. Ukuran biasa: **±1000 aksara** (lebih kurang 150–250 patah perkataan, atau ±250 token). Terlalu kecil → konteks pecah & hilang makna; terlalu besar → vektor jadi kabur & tak tepat.
- **Chunk overlap** — berapa banyak teks **bertindih** antara dua *chunk* berjiran. Biasa: **10–20% daripada chunk size**, contoh **200 aksara** untuk *chunk* 1000 aksara. *Overlap* memastikan ayat yang terpotong di sempadan *chunk* tidak hilang maknanya.

```
Dokumen: "...Pembaharuan LMM boleh dibuat 3 bulan sebelum tarikh luput. | Bayaran RM30 setahun. Lesen tamat lebih 3 tahun perlu ujian semula..."

chunk size = 1000 aksara, overlap = 200 aksara

Chunk A: [....Pembaharuan LMM boleh dibuat 3 bulan sebelum tarikh luput. Bayaran RM30 setahun.]
                                                    └──── 200 aksara bertindih ────┐
Chunk B:                        [Bayaran RM30 setahun. Lesen tamat lebih 3 tahun perlu ujian semula...]
```

Perhatikan "Bayaran RM30 setahun" muncul dalam **kedua-dua** *chunk* — itulah *overlap*. Jika tiada, soalan tentang bayaran mungkin terlepas konteks "pembaharuan LMM".

> **Nilai permulaan yang selamat untuk dokumen JPJ:** `chunk size = 1000`, `chunk overlap = 200`. Ini titik mula yang baik — kita boleh ubah suai dan bandingkan kualiti pada **Hari 3** (SESI 14, pengoptimuman).

### Recursive Character Text Splitter

Cara **naif** memotong ialah setiap 1000 aksara secara buta — tetapi ini kerap memotong di tengah perkataan atau ayat. **Recursive Character Text Splitter** (yang kita guna dalam n8n) lebih bijak: ia cuba potong pada **sempadan semula jadi** mengikut keutamaan — dulu di perenggan (`\n\n`), kemudian baris (`\n`), kemudian ayat, kemudian perkataan (` `) — supaya setiap *chunk* kekal koheren selagi boleh. Ia "recursive" kerana jika satu bahagian masih terlalu besar, ia turun ke pemisah seterusnya.

### Reka *metadata*

Setiap *chunk* disimpan bersama *metadata* yang berguna untuk penapisan & petikan. Reka bentuk contoh untuk JPJ:

| Medan metadata | Contoh nilai | Kegunaan |
|----------------|--------------|----------|
| `source` | `sop-kaunter-lesen.pdf` | Petik sumber dalam jawapan |
| `doc_type` | `lesen` / `pendaftaran` / `saman` / `sop` / `faq` | Penapisan mengikut jenis |
| `title` | `SOP Pembaharuan Lesen Memandu` | Rujukan mesra pengguna |
| `updated` | `2026-01` | Kesan dokumen lapuk |

### Latihan SESI 8: Import dokumen JPJ contoh

Dokumen contoh untuk latihan ada di [`../templates/sample-docs/`](../templates/sample-docs/) — merangkumi prosedur lesen (LMM/CDL/GDL), pendaftaran & pindah milik kenderaan, saman/kompaun, SOP dalaman & FAQ. Langkah import & pemeriksaan chunking: [Latihan 2 dalam lab](./snippets/lab.md).

> ⚠️ **Penafian:** Semua "dokumen JPJ" dalam `sample-docs/` ialah **contoh sintetik untuk latihan sahaja** — bukan pekeliling/SOP rasmi. Untuk penggunaan sebenar, gantikan dengan dokumen rasmi JPJ yang sah & terkini.

---

## SESI 9 (2.30 – 3.45) — Membina *Ingestion Workflow* dalam n8n

Sekarang kita satukan SESI 6–8 menjadi **satu *workflow* n8n** yang mengindeks dokumen ke Qdrant. Deliverable rujukan penuh ada di [`../templates/workflows/02-ingestion-workflow.json`](../templates/workflows/02-ingestion-workflow.json) — anda boleh **import** untuk banding selepas cuba bina sendiri.

### Bentuk aliran

```
┌──────────────┐   ┌──────────────────────────────────────────────┐
│ Manual /     │   │            Qdrant Vector Store               │
│ File Upload  │──▶│              (mod: Insert)                   │
│ Trigger      │   │                                              │
└──────────────┘   │   ┌─────────────────┐   ┌──────────────────┐│
                   │   │ Embeddings      │   │  Default Data    ││
                   │   │ OpenAI (sub)    │   │  Loader (sub)    ││
                   │   └─────────────────┘   │        ▲         ││
                   │                         │        │         ││
                   │                         │ Recursive Char   ││
                   │                         │ Text Splitter    ││
                   │                         │     (sub)        ││
                   │                         └──────────────────┘│
                   └──────────────────────────────────────────────┘
```

Dalam n8n, *node* AI menggunakan corak **root node + sub-node**: *node* utama (Qdrant Vector Store) ialah "root", dan komponen lain (embedding model, data loader, text splitter) disambung ke bawahnya sebagai "sub-node". Ini mungkin kelihatan pelik pada mulanya — tetapi ia membolehkan anda **tukar satu bahagian** (cth. model embedding) tanpa membina semula seluruh aliran.

### Node demi node

| Node | Peranan | Tetapan penting |
|------|---------|-----------------|
| **Manual Trigger** (atau **Read/Write Files from Disk** / borang muat naik) | Mulakan aliran & bekalkan fail dokumen | Baca dari folder `sample-docs/` |
| **Qdrant Vector Store** (root, mod **Insert Documents**) | Tulis vektor + payload ke Qdrant | `Collection: jpj_documents`; *credential* Qdrant (`http://qdrant:6333`) |
| **Default Data Loader** (sub-node) | Tukar fail/teks kepada format "document" & lampirkan metadata | Set medan metadata: `source`, `doc_type`, `title` |
| **Recursive Character Text Splitter** (sub-node) | Potong teks kepada *chunk* | `Chunk Size: 1000`, `Chunk Overlap: 200` |
| **Embeddings OpenAI** (sub-node) | Tukar setiap *chunk* kepada vektor | Model: `text-embedding-3-small` (1536 dim) |

**Kenapa susunan ini penting:** Data Loader membaca dokumen → Text Splitter memotongnya kepada *chunk* → Embeddings OpenAI menukar setiap *chunk* kepada vektor 1536-dimensi → Qdrant Vector Store menyimpan vektor + teks + metadata sebagai *point* dalam *collection*. **Model embedding di sini mesti sama** dengan yang akan digunakan dalam SESI 10.

### Mengesahkan hasil

Selepas *workflow* berjalan, kembali ke [dashboard Qdrant](http://localhost:6333/dashboard) dan periksa *collection* `jpj_documents` — bilangan *point* patut > 0. **Collection kosong = ingestion gagal** (punca biasa: fail tidak dibaca, atau ralat *credential*). Lab penuh: [Latihan 3](./snippets/lab.md).

---

## SESI 10 (3.45 – 5.00) — Membina *Retrieval Workflow* (Pembantu Pengetahuan JPJ)

Inilah **Projek Hari 2**: pembantu yang menjawab soalan JPJ daripada dokumen yang baru diindeks. Deliverable rujukan: [`../templates/workflows/03-retrieval-workflow.json`](../templates/workflows/03-retrieval-workflow.json).

### Bentuk aliran

```
┌──────────────┐   ┌────────────────────────────────────────────────────┐
│ Chat Trigger │   │           Question and Answer Chain                │
│ (soalan      │──▶│                                                    │
│  pengguna)   │   │  ┌──────────────┐        ┌───────────────────────┐ │
└──────────────┘   │  │ OpenAI Chat  │        │  Qdrant Vector Store  │ │
                   │  │ Model (sub)  │        │   (mod: Retrieve)     │ │
                   │  └──────────────┘        │          ▲            │ │
                   │                          │  ┌───────────────────┐│ │
                   │                          │  │ Embeddings OpenAI ││ │
                   │                          │  │      (sub)        ││ │
                   │                          │  └───────────────────┘│ │
                   │                          └───────────────────────┘ │
                   └────────────────────────────────────────────────────┘
```

### Node demi node

| Node | Peranan | Tetapan penting |
|------|---------|-----------------|
| **Chat Trigger** (atau Manual Trigger + medan soalan) | Terima soalan pengguna | — |
| **Question and Answer Chain** (root) | Selaraskan: embed soalan → cari → suntik konteks → tanya LLM | *Prompt* diarah untuk jawab **hanya** dari konteks & petik sumber |
| **Qdrant Vector Store** (sub, mod **Retrieve Documents**) | Cari *chunk* paling relevan | `Collection: jpj_documents`; `Top K: 4` |
| **Embeddings OpenAI** (sub) | Tukar **soalan** kepada vektor | Model: `text-embedding-3-small` — **mesti SAMA** dengan ingestion |
| **OpenAI Chat Model** (sub) | Jana jawapan akhir | Model: `gpt-4o-mini` (atau setara) |

### Top-k & context injection

**Top K = 4** bermaksud Qdrant memulangkan **4 *chunk* paling serupa** dengan soalan. *Q&A Chain* kemudian menyusun *prompt* lebih kurang begini:

```
Anda pembantu JPJ. Jawab soalan HANYA berdasarkan konteks di bawah.
Jika jawapan tiada dalam konteks, katakan anda tidak pasti — JANGAN reka.
Sebutkan sumber (nama dokumen) yang anda rujuk.

Konteks:
[chunk 1 ...]  (source: sop-lesen.pdf)
[chunk 2 ...]  (source: faq-saman.pdf)
[chunk 3 ...]
[chunk 4 ...]

Soalan: Berapa lama tempoh untuk membaharui LMM?
```

Arahan *"jawab HANYA berdasarkan konteks"* + *"jangan reka"* inilah yang **mengikat** jawapan pada dokumen sebenar — teras anti-halusinasi RAG. Jika naikkan `Top K` terlalu tinggi (cth. 20), *prompt* jadi bising & mahal; terlalu rendah (1), mungkin terlepas konteks. **4 titik mula yang baik.**

### Contoh soalan JPJ untuk diuji

Selepas *workflow* siap, uji dengan soalan yang jawapannya **ada** dalam dokumen contoh:

- *"Berapa lama tempoh sah Lesen Memandu Malaysia (LMM) boleh dibaharui?"*
- *"Apa dokumen diperlukan untuk pindah milik kenderaan?"*
- *"Bagaimana cara semak saman/kompaun tertunggak?"*
- *"Apa syarat kelayakan untuk memohon GDL (Goods Driving Licence)?"*
- *"Apa prosedur pendaftaran kenderaan baharu?"*
- *"Apa langkah SOP pegawai kaunter untuk pertanyaan lesen tamat tempoh?"*

Dan **satu soalan luar skop** (cth. *"Berapa harga minyak petrol hari ini?"*) untuk mengesahkan pembantu menjawab *"tidak pasti / tiada dalam dokumen"* — **bukan** mereka jawapan. Lab penuh & pengesahan petikan sumber: [Latihan 3 & 4](./snippets/lab.md).

---

## Penutup — Ringkasan & Langkah Seterusnya

### Ringkasan

Hari ini anda telah:

1. ✅ Faham **dua fasa RAG** — *ingestion* (indeks sekali) vs *retrieval* (jawab setiap kali) — dan bagaimana kedua-duanya berkongsi satu Qdrant.
2. ✅ *Deploy* **Qdrant** dengan Docker & buka dashboard di `http://localhost:6333/dashboard`.
3. ✅ Faham **collection, point, vector, payload**, serta kepentingan **dimensi (1536)** & **distance (Cosine)** sepadan model.
4. ✅ Kuasai **chunking** — kenapa perlu, `chunk size ≈ 1000`, `overlap ≈ 200`, Recursive Character Text Splitter, & reka *metadata*.
5. ✅ Bina **Ingestion Workflow** (Upload → Data Loader → Splitter → Embeddings → Qdrant) & indeks dokumen JPJ contoh.
6. ✅ Bina **Retrieval Workflow / Pembantu Pengetahuan JPJ** (Question → Embed → Vector Search top-k → Context → LLM → Answer) dengan jawapan **berpaksikan sumber**.

### Simpan kerja anda

Dalam n8n, **eksport** kedua-dua *workflow* (menu ⋯ → Download) sebagai sandaran, dan banding dengan templat rujukan di [`../templates/workflows/`](../templates/workflows/).

### Apa seterusnya — Hari 3 (SESI 11–15)

Esok kita naik taraf RAG ini kepada **AI Agent berbilang alat** (agentic) — pembantu yang bukan sahaja mencari KB, tetapi juga **memilih *tool* secara dinamik** (semak status lesen via mock API, cipta tiket khidmat). Kita juga sentuh **RAG lanjutan** (metadata filtering, hybrid search, re-ranking), **penilaian & pengoptimuman**, dan **deployment** dengan pertimbangan residensi data (Ollama *on-premise*).

Sebelum tamat hari ini — pastikan Qdrant anda masih **berisi** (`docker compose` jangan `down` tanpa `-v` hilang data) dan kedua-dua *workflow* tersimpan!

---

> 🎤 **Nota penceramah/jurulatih:** [`nota-penceramah.md`](./nota-penceramah.md) — nota persembahan, tip demo langsung, titik kegagalan biasa & soalan lazim peserta untuk Hari 2.
