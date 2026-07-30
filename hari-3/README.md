# Hari 3 — AI Agents, Pengoptimuman & Deployment

Selamat datang ke **hari terakhir** kursus *Building RAG & Agentic AI Applications with n8n* (RAG-N8N-JPJ-101) untuk **Jabatan Pengangkutan Jalan Malaysia (JPJ)**. Sepanjang **Hari 1** anda faham asas AI/LLM/RAG dan bina *workflow* AI pertama; sepanjang **Hari 2** anda bina *pipeline* lengkap — *ingestion* (Upload→Extract→Chunk→Embed→Store) dan *retrieval* (Question→Embed→Search→LLM→Answer) — sehingga menjadi **Pembantu Pengetahuan JPJ** yang menjawab soalan berpaksikan dokumen.

Hari ini ialah **kemuncaknya**: kita naik taraf pembantu RAG itu daripada sekadar "menjawab soalan" kepada sebuah **AI Agent berbilang alat** — pembantu yang boleh **membuat keputusan sendiri** tentang *tool* mana perlu digunakan, memanggil banyak sumber (KB vektor, API status lesen, sistem tiket), kemudian menyusun jawapan bersepadu. Selepas itu kita perhalusi kualiti (**RAG lanjutan** + **penilaian**), dan akhirnya *deploy* ke pengeluaran dengan pematuhan data — sebelum anda **membentangkan projek capstone** masing-masing.

> 📌 **Aturcara rasmi ialah sumber kebenaran.** Hari ini merangkumi **SESI 11–15 + Capstone** seperti dalam [`JADUAL.md`](../JADUAL.md). Jangan ubah skop tanpa menyemaknya.

> **Konvensyen:** Penerangan dalam **Bahasa Melayu** (kita panggil anda *anda*), tetapi nama *node* n8n, kunci JSON, dan istilah teknikal dikekalkan dalam **Bahasa Inggeris** — amalan standard industri yang kita ikut sepanjang kursus.

> **Cara guna dokumen ini:** Bahagian ini menerangkan **konsep & rasional** setiap sesi (kenapa sebelum bagaimana). Latihan *hands-on* langkah demi langkah ada di [`snippets/lab.md`](./snippets/lab.md). Baca bahagian konsep di sini dahulu, kemudian pindah ke lab untuk cuba sendiri.

---

> 📖 **Bacaan lanjut (buku rujukan):** *Building Agent-Powered Applications* (V. Zvarydchuk, Packt 2026) — **Bab 8** (arkitektur AI Agent, memori, orkestrasi, multi-agent), **Bab 9** (bina agent, function calling, MCP, planning, human-in-the-loop), **Bab 10** (penilaian RAG & agent, LLM-as-judge, Responsible AI). Pemetaan penuh: [`../nota/00-rujukan-buku.md`](../nota/00-rujukan-buku.md).

## Fokus Hari Ini

| Topik | Rujukan rasmi |
|-------|----------------|
| AI Agent node (n8n) | [docs.n8n.io/.../agent](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/) |
| Konsep *Advanced AI* dalam n8n | [docs.n8n.io/advanced-ai](https://docs.n8n.io/advanced-ai/) |
| *Tool / function calling* (LLM) | [platform.openai.com/docs/guides/function-calling](https://platform.openai.com/docs/guides/function-calling) |
| *Tools Agent* & penaakulan ejen | [docs.n8n.io/advanced-ai/examples/agent](https://docs.n8n.io/advanced-ai/examples/introduction/) |
| Qdrant sebagai *retriever tool* | [docs.n8n.io/.../vectorstoreqdrant](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.vectorstoreqdrant/) |
| HTTP Request Tool | [docs.n8n.io/.../toolhttprequest](https://docs.n8n.io/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.toolhttprequest/) |
| *Metadata filtering* Qdrant | [qdrant.tech/documentation/concepts/filtering](https://qdrant.tech/documentation/concepts/filtering/) |
| *Hybrid search* (dense + sparse) | [qdrant.tech/articles/hybrid-search](https://qdrant.tech/articles/hybrid-search/) |
| Penilaian sistem RAG | [docs.ragas.io](https://docs.ragas.io/) |
| Ollama (LLM *on-premise*) | [ollama.com](https://ollama.com/) |
| *Hosting* & *scaling* n8n | [docs.n8n.io/hosting](https://docs.n8n.io/hosting/) |
| Docker Compose | [docs.docker.com/compose](https://docs.docker.com/compose/) |

---

## Jadual Hari Ini

| Masa | Agenda |
|------|--------|
| 8.30 – 9.00 pagi | Pendaftaran Peserta & Minum Pagi |
| **9.00 – 10.15 pagi** | **SESI 11: AI Agents dengan n8n** — Apa itu *AI Agent* · *Agent* vs *Workflow* · *Tool calling* · Penaakulan ejen · 🔎 Demo ejen ringkas |
| **10.15 – 11.30 pagi** | **SESI 12: AI Agents Berbilang Alat** — Menyambung banyak *tools* · Pemilihan *tool* dinamik · Integrasi API & pangkalan data · 💻 Lab: **Ejen Perkhidmatan JPJ** |
| **11.30 – 1.00 tgh** | **SESI 13: Teknik RAG Lanjutan** — *Metadata filtering* · *Hybrid search* · *Re-ranking* · *Parent-child* & *multi-document retrieval* · 🔎 Demo banding kaedah |
| 1.00 – 2.30 petang | Rehat dan Makan Tengah Hari |
| **2.30 – 3.15 petang** | **SESI 14: Penilaian & Pengoptimuman RAG** — Ukur ketepatan · Kurangkan halusinasi · Optimum *prompt*/*retrieval*/kos · 💻 Latihan tingkatkan jawapan |
| **3.15 – 4.15 petang** | **SESI 15: Production Deployment** — Seni bina pengeluaran · Docker · *On-premise*/residensi data · Keselamatan · Pemantauan |
| **4.15 – 5.00 petang** | **Projek Capstone + Demo & Sijil** — Peserta bentang pembantu masing-masing · Penilaian · Sijil |
| 5.00 petang | Bersurai |

> **Hasil Hari 3:** Ejen AI JPJ berbilang alat yang boleh di-*deploy*, plus projek capstone yang dibentangkan.

---

## Imbas Kembali — Apa Anda Sudah Ada

Sebelum masuk topik baharu, sahkan anda sudah ada tiga perkara ini daripada Hari 2 (kita **guna semula** kesemuanya hari ini):

| Aset | Dari mana | Guna hari ini untuk |
|------|-----------|----------------------|
| **Qdrant collection** berisi dokumen JPJ (ter-*embed*) | SESI 8–9 (*ingestion*) | Dijadikan **tool** "Search Knowledge Base" untuk ejen |
| **Retrieval workflow** (Question→Embed→Search→LLM→Answer) | SESI 10 | Asas perbandingan dengan RAG lanjutan (SESI 13) |
| **Credentials** OpenAI / Ollama & Qdrant | Hari 1–2 | Disambung semula ke *Chat Model* & *Vector Store* ejen |

> Kalau *collection* Qdrant anda kosong atau *credentials* belum siap, selesaikan dahulu — ejen hari ini **bergantung** padanya. Rujuk [Hari 2](../hari-2/) jika perlu.

---

## SESI 11 (9.00 – 10.15) — AI Agents dengan n8n

### Kenapa "ejen", bukan sekadar *workflow*?

Setakat Hari 2, pembantu anda ialah sebuah **workflow tetap** (*fixed pipeline*): setiap soalan mengalir melalui **langkah yang sama, dalam susunan yang sama** — Embed → Search → LLM → Answer. Ia hebat untuk **satu jenis tugas** (menjawab dari KB), tetapi ia **tidak boleh membuat keputusan**. Jika pengguna bertanya *"Apa status lesen No. KP saya?"*, *workflow* RAG tetap akan cuba mencari jawapan dalam dokumen — walhal jawapan sebenar ada dalam **sistem lesen**, bukan dalam pekeliling.

**AI Agent** menyelesaikan masalah ini. Ejen ialah LLM yang diberi **satu set alat (*tools*)** dan **kebebasan memilih** alat mana hendak digunakan untuk menjawab soalan tertentu — dan boleh menggunakan **beberapa alat berturut-turut** sebelum menyusun jawapan akhir.

| | **Workflow (RAG Hari 2)** | **AI Agent (Hari 3)** |
|---|---|---|
| **Aliran** | Tetap — langkah sama setiap kali | Dinamik — ejen putuskan langkah |
| **Keputusan** | Anda (pereka) tetapkan lebih awal | LLM tetapkan pada masa jalan |
| **Bilangan sumber** | Biasanya satu (KB) | Banyak *tools* (KB, API, DB, tiket) |
| **Sesuai untuk** | Satu tugas jelas & berulang | Soalan pelbagai jenis, tak dijangka |
| **Boleh diramal** | Tinggi (deterministik) | Rendah (LLM buat pilihan) |

> **Bila guna yang mana?** Kalau tugas **tunggal dan tetap** (cth. "ringkaskan setiap PDF yang dimuat naik"), *workflow* biasa lebih mudah, murah dan **boleh diramal**. Kalau pengguna boleh tanya **pelbagai jenis soalan** yang perlukan **sumber berlainan**, barulah ejen berbaloi. Ejen bukan sentiasa "lebih baik" — ia **lebih fleksibel tetapi kurang boleh diramal**.

### Apa itu *tool calling*? (konsep teras hari ini)

Ini konsep paling penting SESI 11 — fahaminya, dan seluruh hari jadi mudah.

LLM biasa hanya boleh **menjana teks**. Ia tidak boleh, dengan sendirinya, mencari dalam Qdrant atau memanggil API JPJ. **Tool calling** (juga dipanggil *function calling*) ialah mekanisme yang membenarkan LLM **meminta** sistem luar menjalankan sesuatu bagi pihaknya.

Caranya begini:

1. **Anda daftarkan alat.** Setiap *tool* diberi tiga perkara: satu **nama** (cth. `search_knowledge_base`), satu **penerangan** dalam bahasa biasa (*"Cari maklumat dalam pekeliling, SOP & akta JPJ"*), dan **skema parameter** (apa input yang alat perlukan — cth. `query: string`).
2. **LLM baca penerangan itu.** Bila soalan masuk, model **membandingkan** soalan dengan penerangan setiap alat, dan memutuskan: adakah saya perlu alat? Yang mana? Apa argumennya?
3. **Model mengeluarkan "panggilan alat" berstruktur** — bukan jawapan kepada pengguna, tetapi arahan mesin seperti `search_knowledge_base({ query: "pembaharuan lesen GDL" })`.
4. **n8n menjalankan alat itu** (buat carian Qdrant sebenar) dan **memulangkan hasilnya** kepada model.
5. **Model membaca hasil**, dan sama ada **memanggil alat lain** (jika perlu lagi maklumat) atau **menyusun jawapan akhir** untuk pengguna.

> **Analogi JPJ:** Bayangkan seorang **pegawai kaunter baharu**. Dia tidak hafal segala-galanya, tetapi dia tahu **di mana** hendak cari: fail SOP di rak, terminal sistem lesen di meja, borang tiket aduan di laci. Bila orang awam bertanya, dia **pilih sumber yang betul**, ambil maklumat, dan baru jawab. LLM sebagai ejen berkelakuan sama — "penerangan alat" ialah label pada setiap rak/terminal/laci.

**Penting difahami:** yang membuat keputusan ialah **LLM**, bukan anda. Anda hanya **menyediakan alat + penerangan yang jelas**. Kualiti penerangan alat menentukan sama ada ejen memilih alat yang betul — kita akan lihat ini secara praktikal di SESI 12.

### Aliran seekor ejen (rajah)

```
                    ┌─────────────────────────────────────────────┐
                    │                 AI  AGENT                   │
                    │        (LLM + penerangan semua tool)        │
   User ──────────► │                                             │
   "soalan"         │   1. Fikir: perlukah tool? yang mana?       │
                    │   2. Select Tool ──► pilih 1 alat           │
                    │   3. Execute Tool ─► n8n jalankan alat       │
                    │   4. Baca hasil ──► cukup? tak cukup?       │
                    │        └── belum cukup: ulang 2–4 ◄──┐      │
                    │   5. Generate Answer (jawapan akhir) │      │
                    └──────────────────┬───────────────────┘      │
                                       │  (gelung sehingga cukup)  │
                                       ▼                           │
                                  Jawapan  ──────────► User        │
```

Ringkasnya: **User → AI Agent → Select Tool → Execute Tool → Generate Answer**, dengan langkah *Select→Execute* boleh **berulang** beberapa kali sebelum jawapan akhir dikeluarkan. Gelung inilah yang dipanggil **penaakulan ejen** (*agent reasoning*).

### Node n8n yang terlibat

Dalam n8n, sebuah ejen dibina daripada **satu *root node*** dan beberapa ***sub-node*** yang dipasang padanya (*cluster node*):

| Node | Peranan | Contoh |
|------|---------|--------|
| **AI Agent** (*root*) | Otak ejen — jalankan gelung penaakulan | jenis *Tools Agent* (lalai) |
| **Chat Model** (*sub*) | LLM yang buat keputusan & tulis jawapan | `OpenAI Chat Model` atau `Ollama Chat Model` |
| **Memory** (*sub*, pilihan) | Ingat perbualan sebelum ini | `Simple Memory` (*window buffer*) |
| **Tool** (*sub*, ≥1) | Alat yang ejen boleh panggil | `Vector Store` / `HTTP Request` / `Call n8n Workflow` Tool |

> **Nota teknikal (untuk yang ingin tahu):** *node* AI Agent lalai n8n menggunakan **Tools Agent**, yang bergantung pada keupayaan *tool/function calling* **asli** model (cth. GPT-4o-mini, atau model Ollama yang menyokong *tools* seperti `llama3.1`). Ini berbeza daripada gaya lama "ReAct" yang mengajar model menulis langkah *"Thought/Action"* dalam teks — *Tools Agent* lebih tepat kerana panggilan alat dijana secara berstruktur oleh model itu sendiri. Jika model anda **tidak** menyokong *tool calling*, ejen tidak akan berfungsi dengan betul — pilih model yang menyokongnya.

### 🔎 Demo SESI 11 — ejen paling ringkas (satu alat)

Kita mula seringkas mungkin: satu ejen dengan **satu** alat sahaja — sebuah **Calculator** — semata-mata untuk melihat gelung *tool calling* berlaku secara nyata. Susun aturnya:

```
Chat Trigger ──► AI Agent ──► (jawapan)
                    │
      ┌─────────────┼──────────────┐
      ▼             ▼              ▼
 OpenAI Chat    Simple Memory   Calculator
   Model                          Tool
```

Tanya ejen: *"Kalau kompaun asal RM300 dan diskaun 50%, berapa perlu dibayar?"* — perhatikan dalam paparan *execution*, ejen **memanggil Calculator** (bukan mengira dalam kepala LLM, yang kadang tersilap), dapat `150`, kemudian menyusun ayat jawapan. Anda baru saja menyaksikan **Select Tool → Execute Tool → Generate Answer**.

➡️ Langkah penuh: [Lab Latihan 1](./snippets/lab.md#latihan-1--ejen-pertama-satu-tool). Latar konsep tambahan: [`../nota/06-ai-agents.md`](../nota/06-ai-agents.md).

---

## SESI 12 (10.15 – 11.30) — AI Agents Berbilang Alat

### Daripada satu alat kepada banyak

Ejen sebenar JPJ perlukan **lebih daripada satu** sumber. Dalam sesi ini kita bina **Ejen Perkhidmatan JPJ** dengan **empat** alat — setiap satu mewakili sumber berlainan yang seorang pegawai perkhidmatan sebenar akan rujuk:

| # | Tool | Jenis node n8n | Sumber sebenar diwakili |
|---|------|----------------|--------------------------|
| a | **Search Knowledge Base** | `Vector Store` Tool (Qdrant) | Pekeliling, SOP, akta (KB Hari 2) |
| b | **Check Licence/Registration Status** | `HTTP Request` Tool (mock API) | Sistem lesen & pendaftaran kenderaan |
| c | **Retrieve Applicant Info** | `HTTP Request` / `Postgres` Tool (mock) | Rekod pemohon / pelanggan |
| d | **Create Service Ticket** | `HTTP Request` / `Call n8n Workflow` Tool | Sistem tiket / aduan |

> **Penting:** kesemua "sistem" (b, c, d) ialah **mock** (tiruan) untuk latihan — kita **tidak** sambung ke sistem JPJ sebenar. Sebuah *endpoint* tiruan (cth. guna [mockapi.io](https://mockapi.io/), [webhook.site](https://webhook.site/), atau *workflow* n8n lain sebagai API) memulangkan data contoh. Untuk penggunaan sebenar, sambungan mesti melalui saluran rasmi JPJ yang diluluskan (rujuk SESI 15 & [`../nota/08-governance-keselamatan.md`](../nota/08-governance-keselamatan.md)).

### Seni bina ejen berbilang alat

```
                       ┌──────────────────────────────┐
 Chat Trigger ───────► │          AI  AGENT           │ ──► Jawapan bersepadu
 "soalan JPJ"          │  (Chat Model + System Prompt)│
                       └──────────────┬───────────────┘
                                      │  ejen pilih alat ikut soalan
        ┌──────────────┬──────────────┼──────────────┬───────────────┐
        ▼              ▼              ▼              ▼               ▼
  OpenAI Chat     Simple Memory   (a) Search    (b) Check       (c) Retrieve   (d) Create
     Model                          KB Tool     Licence Tool    Applicant       Ticket
                                    (Qdrant)     (HTTP mock)     (HTTP mock)     (HTTP mock)
```

### Bagaimana ejen "tahu" alat mana untuk soalan mana?

Ejen **tidak** dikodkan dengan `if/else`. Ia memilih berdasarkan **penerangan alat** (langkah 2 dalam *tool calling* SESI 11). Contoh penerangan yang **baik**:

| Tool | Penerangan (yang ejen baca) |
|------|------------------------------|
| Search KB | *"Cari maklumat prosedur, syarat & polisi dalam pekeliling, SOP dan akta JPJ. Guna untuk soalan 'bagaimana', 'apa syarat', 'apa prosedur'."* |
| Check Licence | *"Semak status semasa lesen memandu atau pendaftaran kenderaan bagi satu No. KP atau No. Pendaftaran tertentu. Guna bila pengguna beri nombor & tanya status."* |
| Create Ticket | *"Cipta tiket khidmat pelanggan bila isu tidak dapat diselesaikan dengan maklumat sedia ada. Perlukan ringkasan isu & butiran hubungan."* |

Cuba tiga soalan berbeza dan perhatikan pilihan ejen (*dynamic tool selection*):

- *"Apa dokumen diperlukan untuk pembaharuan lesen GDL?"* → ejen pilih **Search KB**.
- *"Status lesen KP 900101-14-5566?"* → ejen pilih **Check Licence** dengan argumen `ic = "900101-14-5566"`.
- *"Sistem tak jumpa rekod saya, tolong buat aduan."* → ejen pilih **Create Ticket**.
- *"Saya nak perbaharui lesen tapi ada saman tertunggak — macam mana?"* → ejen mungkin pilih **Search KB** (prosedur) **dan** **Check Licence** (semak saman) sebelum menjawab — inilah **berbilang alat berturut-turut**.

> **Rahsia kualiti ejen ialah penerangan alat + *system prompt*.** Kalau ejen kerap pilih alat yang salah, jarang salah model — selalunya penerangan alat **terlalu kabur** atau bertindih. Kita perhalusi ini di SESI 14.

### System Prompt ejen — beri ia "watak" & peraturan

*System prompt* ejen menetapkan siapa dia dan peraturan mainnya. Contoh (dalam medan **System Message** node AI Agent):

```
Anda ialah Pembantu Perkhidmatan JPJ. Bantu pegawai & orang awam dengan
soalan lesen memandu, pendaftaran kenderaan, saman/kompaun dan SOP.

PERATURAN:
- Untuk soalan prosedur/polisi, SENTIASA guna tool "Search Knowledge Base"
  dahulu dan jawab HANYA berdasarkan hasilnya. Sertakan rujukan sumber.
- Untuk status lesen/kenderaan, guna tool "Check Licence Status".
- Jika maklumat tidak ditemui, KATAKAN anda tidak pasti — JANGAN reka jawapan.
- Jika pengguna minta bantuan lanjut atau isu tak selesai, tawarkan buat tiket.
- Jawab dalam Bahasa Melayu, ringkas dan sopan.
```

> Perhatikan baris *"JANGAN reka jawapan"* dan *"jawab HANYA berdasarkan hasil"* — ini **pertahanan anti-halusinasi** peringkat ejen, selari dengan prinsip RAG (jawapan berpaksikan sumber). Lebih lanjut: [`../nota/07-prompt-engineering.md`](../nota/07-prompt-engineering.md).

### 💻 Lab SESI 12 — Ejen Perkhidmatan JPJ

Anda akan bina ejen empat-alat ini langkah demi langkah dalam lab. **Deliverable rujukan** (*workflow* siap, dibina oleh pasukan bahan) ialah [`../templates/workflows/04-agent-workflow.json`](../templates/workflows/04-agent-workflow.json) — import untuk membanding hasil anda **selepas** mencuba sendiri.

➡️ Langkah penuh: [Lab Latihan 2–5](./snippets/lab.md#latihan-2--tambah-tool-search-knowledge-base-qdrant).

---

## SESI 13 (11.30 – 1.00) — Teknik RAG Lanjutan

RAG asas Hari 2 mengambil *chunk* paling "serupa" secara semantik. Ia cukup untuk soalan mudah, tetapi ada masa jawapan **kurang tepat** — *chunk* yang betul tersembunyi, atau ejen dapat maklumat daripada bahagian yang salah. Lima teknik berikut menaik taraf kualiti *retrieval*.

### 1. Metadata filtering — tapis ikut jenis/bahagian dokumen

Semasa *ingestion* (Hari 2) setiap *chunk* disimpan dengan **metadata** — cth. `doc_type: "SOP"`, `division: "Pelesenan"`, `year: 2024`. *Metadata filtering* bermaksud: **cari hanya dalam subset** yang sepadan tapisan, sebelum kira persamaan vektor.

```
Soalan: "SOP pembaharuan lesen bahagian Pelesenan"
        └─► Filter: division = "Pelesenan" AND doc_type = "SOP"
            └─► Carian vektor HANYA dalam chunk yang lulus tapisan
                └─► Hasil lebih tepat, kurang "bunyi" dari bahagian lain
```

Dalam node `Qdrant Vector Store`, tapisan diletak dalam medan **filter** (format Qdrant `must`/`should`). Ini **paling berkesan** untuk JPJ kerana dokumen jelas terbahagi ikut bahagian (Pelesenan, Pendaftaran, Penguatkuasaan) dan jenis (akta, SOP, pekeliling). Rujukan: [qdrant.tech/documentation/concepts/filtering](https://qdrant.tech/documentation/concepts/filtering/).

### 2. Hybrid search — gabung makna + kata kunci

- **Dense (vektor)** — pandai makna/semantik, tetapi kadang terlepas **istilah tepat** (nombor pekeliling, kod borang seperti `JPJK3`).
- **Sparse (kata kunci, cth. BM25)** — pandai padanan kata **tepat**, tetapi tak faham sinonim.

**Hybrid search** menjalankan kedua-duanya dan **menggabung** skor. Berguna bila soalan JPJ mengandungi **kod/nombor rasmi** yang mesti padan tepat *dan* konteks semantik. Rujukan: [qdrant.tech/articles/hybrid-search](https://qdrant.tech/articles/hybrid-search/).

### 3. Re-ranking — susun semula hasil dengan model kedua

Carian awal cepat tetapi kasar. **Re-ranking** mengambil (cth.) 20 hasil teratas, kemudian **model *cross-encoder*** menilai semula setiap satu terhadap soalan dengan lebih teliti, dan kekalkan hanya **3–5 terbaik** untuk dihantar ke LLM.

```
Soalan ──► Carian vektor (ambil top-20 laju) ──► Re-ranker (nilai teliti)
                                                     └─► Top-5 paling relevan ──► LLM
```

Kesannya: LLM dapat konteks **lebih padat & relevan** → jawapan lebih tepat & kos *token* lebih rendah.

### 4. Parent-child retrieval — cari kecil, jawab besar

Dilema *chunking*: *chunk* **kecil** = padanan tepat tetapi konteks tidak cukup; *chunk* **besar** = konteks kaya tetapi padanan kabur. **Parent-child** menyelesaikannya: **cari** guna *chunk* kecil (**child**), tetapi **hantar ke LLM** perenggan/seksyen penuh yang mengandunginya (**parent**).

```
Cari padanan pada: [child kecil: "yuran GDL RM30"]
Hantar ke LLM:      [parent besar: seluruh seksyen "Kadar Bayaran Lesen"]
```

### 5. Multi-document retrieval — kumpul dari banyak sumber

Sesetengah soalan perlukan maklumat merentas **beberapa dokumen** (cth. syarat dalam akta + prosedur dalam SOP + kadar dalam pekeliling). *Multi-document retrieval* memastikan hasil datang daripada **kepelbagaian sumber**, bukan semua daripada satu dokumen — kemudian LLM mensintesis. Ini juga apa yang **ejen berbilang alat** (SESI 12) lakukan pada peringkat lebih tinggi.

### 🔎 Demo SESI 13 — banding kualiti *retrieval*

Ambil **satu soalan sukar** (cth. *"kadar kompaun untuk memandu tanpa lesen yang sah"*) dan jalankan melalui: (i) RAG asas, (ii) + metadata filter, (iii) + re-ranking. Banding *chunk* yang dikembalikan dan ketepatan jawapan akhir. Peserta akan **nampak sendiri** teknik mana paling membantu untuk dokumen JPJ.

➡️ Cabaran lanjutan (metadata filter / re-ranker): [Lab bahagian "Cabaran"](./snippets/lab.md#cabaran).

---

## SESI 14 (2.30 – 3.15) — Penilaian & Pengoptimuman RAG

*"Kalau tidak boleh diukur, tidak boleh diperbaiki."* Sebelum *deploy* untuk JPJ, anda perlu **bukti** pembantu menjawab dengan tepat — bukan sekadar "nampak okay".

### Kenapa penilaian penting untuk JPJ

Jawapan salah tentang prosedur lesen atau kadar kompaun boleh **mengelirukan orang awam** atau **menyusahkan pegawai**. Maka kita mesti mengukur ketepatan secara **sistematik**, bukan pada perasaan.

### Set penilaian (evaluation set) — asas segala-galanya

Bina **set kecil soalan–jawapan JPJ** — 15–30 pasangan cukup untuk mula. Setiap baris: soalan, jawapan rujukan yang **betul**, dan dokumen sumber yang **sepatutnya** dirujuk:

| Soalan | Jawapan rujukan (ringkas) | Sumber sepatutnya |
|--------|----------------------------|--------------------|
| Berapa tempoh sah lesen GDL? | *(isi dari dokumen contoh anda)* | SOP Pelesenan |
| Bolehkah perbaharui lesen jika ada saman tertunggak? | *(isi)* | Pekeliling / FAQ |
| Dokumen untuk pindah milik kenderaan? | *(isi)* | SOP Pendaftaran |

> ⚠️ Isi jawapan rujukan daripada **dokumen contoh** dalam [`../templates/sample-docs/`](../templates/sample-docs/) — **bukan** rekaan, dan **bukan** dokumen rasmi JPJ sebenar. Set ini ialah "kertas jawapan" untuk menanda pembantu anda.

### Empat perkara untuk diukur

| Ukuran | Soalan yang dijawab | Cara semak (ringkas) |
|--------|----------------------|----------------------|
| **Retrieval accuracy** | Adakah *chunk* betul dijumpai? | *Chunk* yang dikembalikan mengandungi jawapan? |
| **Answer correctness** | Adakah jawapan akhir betul? | Banding dengan jawapan rujukan |
| **Groundedness / faithfulness** | Adakah jawapan **berpaksikan** *chunk*, atau direka? | Setiap dakwaan boleh dijejak ke sumber? |
| **Citation** | Adakah sumber dipetik? | Jawapan sebut dokumen/seksyen rujukan? |

**Groundedness** ialah pertahanan anti-halusinasi paling penting: jawapan mesti datang **daripada** dokumen yang dijumpai, bukan daripada "pengetahuan umum" LLM. Ujian mudah: jika anda **buang** *chunk* sumber, jawapan sepatutnya jadi *"saya tidak pasti"* — bukan tetap yakin. Rangka kerja formal untuk ini: [Ragas](https://docs.ragas.io/) (faithfulness, answer relevancy, context precision) — konsepnya berguna walau tanpa alat khusus.

### Gelung pengoptimuman (iteratif)

```
   Jalankan eval set ──► Kenal pasti jawapan LEMAH ──► Diagnos puncanya
        ▲                                                     │
        │                                          ┌──────────┴──────────┐
        │                                          ▼                      ▼
        │                                  Punca RETRIEVAL?         Punca PROMPT?
        │                                  (chunk salah/tiada)      (LLM abai konteks)
        │                                          │                      │
        └──────── Uji semula ◄─── Perbaiki ◄───────┴──────────────────────┘
                                  (chunk size, filter,          (system prompt,
                                   re-rank, top-k)               arahan sitasi)
```

**Diagnosis kunci:** bila jawapan salah, tanya — adakah *chunk* betul **dijumpai**? Jika **tidak** → masalah **retrieval** (baiki *chunking*/filter/*top-k*/re-rank). Jika **ya** tetapi jawapan tetap salah → masalah **prompt/generation** (baiki *system prompt*, minta petik sumber, kurangkan *temperature*).

### Pengoptimuman kos

- **Guna model kecil untuk kerja mudah** — cth. `gpt-4o-mini` untuk kebanyakan soalan; simpan model besar untuk kes sukar sahaja.
- **Kurangkan *top-k*** — hantar 3–5 *chunk* relevan (selepas re-rank), bukan 20 — jimat *token*.
- **Ollama tempatan** — untuk JPJ, model *on-premise* bukan sahaja **selamat** (data tak keluar) tetapi **tiada kos per-token** (rujuk SESI 15).
- **Cache** jawapan soalan lazim jika sesuai.

### 💻 Latihan SESI 14 — tingkatkan jawapan secara berulang

Ambil 3 soalan yang pembantu anda jawab **lemah**, jalankan gelung di atas, dan **catat sebelum/selepas**. Matlamat: buktikan penambahbaikan dengan **angka**, bukan perasaan.

➡️ [Lab Latihan 6](./snippets/lab.md#latihan-6--penilaian--kesan-halusinasi).

---

## SESI 15 (3.15 – 4.15) — Production Deployment

Pembantu berfungsi di komputer anda ≠ sedia untuk JPJ. Sesi ini tentang menjadikannya **boleh dipercayai, selamat & patuh**.

### Seni bina pengeluaran

```
   ┌────────┐    ┌──────────────┐    ┌──────────┐    ┌───────────────────┐
   │ Users  │──► │  Web App /   │──► │   n8n    │──► │  OpenAI  ATAU     │
   │(kaunter│    │  Chat UI     │    │ (agent + │    │  Ollama (on-prem) │
   │ /awam) │◄── │ (frontend)   │◄── │  RAG)    │◄── │  (LLM)            │
   └────────┘    └──────────────┘    └────┬─────┘    └───────────────────┘
                                          │
                          ┌───────────────┼────────────────┐
                          ▼               ▼                ▼
                    ┌──────────┐    ┌──────────┐    ┌──────────────┐
                    │  Qdrant  │    │ Postgres │    │  Storan fail  │
                    │ (vektor) │    │ (n8n DB, │    │  (dokumen,    │
                    │          │    │  log,    │    │   MinIO)      │
                    │          │    │  tiket)  │    │              │
                    └──────────┘    └──────────┘    └──────────────┘
```

- **Web App / Chat UI** — antara muka pengguna (boleh guna *Chat Trigger* n8n, atau *frontend* tersendiri melalui *Webhook*).
- **n8n** — tempat ejen + RAG berjalan.
- **LLM** — OpenAI (mudah) **atau** Ollama (*on-premise*, disyorkan untuk data sensitif JPJ).
- **Qdrant** — pangkalan vektor.
- **PostgreSQL** — pangkalan data n8n (perlu untuk mod pengeluaran/*queue mode*), log & rekod tiket.

### Docker deployment

Kesemua komponen berjalan sebagai kontena melalui **Docker Compose** — satu fail, satu arahan. Anda sudah guna ini di Hari 2 untuk Qdrant; untuk pengeluaran kita tambah n8n + Postgres (+ Ollama) dalam susunan yang sama.

```bash
docker compose up -d          # mula semua servis di latar belakang
docker compose ps             # semak status kontena
docker compose logs -f n8n    # pantau log n8n
```

Fail rujukan: [`../templates/docker-compose.yml`](../templates/docker-compose.yml) — n8n + Qdrant + PostgreSQL (+ Ollama). Langkah penuh: [`../nota/09-deployment.md`](../nota/09-deployment.md).

### VPS / on-premise & residensi data — kenapa penting untuk JPJ

Ini **titik paling kritikal** untuk JPJ. Dokumen JPJ (rekod lesen, kenderaan, penguatkuasaan) mengandungi **data peribadi** dan maklumat sensitif. Jika anda hantar teks itu ke API awan luar negara untuk *embedding*/jawapan, data **keluar** dari kawalan JPJ — isu **PDPA** & kedaulatan data.

> **Postur disyorkan untuk JPJ: Ollama + Qdrant + n8n, kesemuanya *on-premise*.** Model LLM berjalan pada pelayan JPJ sendiri; **tiada teks dokumen keluar** ke internet. Prestasi mungkin sedikit lebih rendah daripada GPT terkini, tetapi **kawalan & pematuhan** jauh lebih penting untuk data kerajaan. Ini konfigurasi yang kita tekankan sepanjang kursus. Perbincangan penuh: [`../nota/08-governance-keselamatan.md`](../nota/08-governance-keselamatan.md).

### Amalan keselamatan

- **Rahsia dalam *environment variables*** — jangan *hardcode* kunci API / kata laluan dalam *workflow*; guna *credentials* n8n & `.env`.
- **HTTPS + reverse proxy** (cth. Traefik/Nginx) di hadapan n8n; jangan dedah port mentah.
- **Kawalan akses** — lindungi UI n8n (authentication), hadkan siapa boleh edit *workflow*.
- **Prinsip keistimewaan minimum** — *credentials* hanya benarkan apa yang perlu.
- **Sanitasi input** — terutama untuk alat yang menulis (cipta tiket) — elak penyalahgunaan.
- **Audit trail** — log siapa tanya apa & jawapan apa (berguna untuk pertikaian & penambahbaikan).

### Pemantauan & logging

- **Log *execution* n8n** — jejak *workflow* gagal, masa tindak balas, penggunaan *tool*.
- **Metrik** (cth. Grafana) — bilangan pertanyaan, kadar kejayaan, kependaman, kos *token*.
- **Amaran** — beritahu pentadbir bila kadar ralat naik atau servis tumbang.
- **Maklum balas pengguna** — butang 👍/👎 pada jawapan → sumber terbaik untuk penambahbaikan berterusan.

---

## Projek Capstone (4.15 – 5.00)

Sesi terakhir: **anda membentangkan pembantu anda**. Capstone ialah **sintesis** tiga hari — bukan topik baharu, tetapi gabungan segala yang telah anda bina.

### Pilih satu opsyen capstone

| # | Opsyen | Fokus | Alat/RAG dicadangkan |
|---|--------|-------|-----------------------|
| 1 | **Licensing Assistant** | Prosedur & status lesen memandu (LMM/CDL/GDL) | KB + Check Licence tool |
| 2 | **Vehicle Registration / Transfer Assistant** | Pendaftaran baharu & pindah milik | KB + Check Registration tool |
| 3 | **Enforcement / Compound Assistant** | Saman/kompaun: semak, kadar, rayuan | KB + Check Compound tool |
| 4 | **Internal SOP / Circular Helpdesk** | Bantu pegawai baharu cari SOP/pekeliling | KB + metadata filter (bahagian) |
| 5 | **Public Counter KB** | Soalan lazim orang awam di kaunter | KB + Create Ticket tool |

Anda **bebas** memilih mana-mana satu (atau gabungan) — gunakan dokumen contoh berkonteks-JPJ yang sama, dan seni bina ejen/RAG yang telah dibina hari ini.

### Format pembentangan (2–3 minit setiap peserta)

1. **Masalah** — soalan JPJ apa yang pembantu ini selesaikan?
2. **Demo langsung** — tanya 2–3 soalan; tunjuk jawapan **berpaksikan sumber** (dengan sitasi).
3. **Tunjuk satu *tool*** beraksi (cth. ejen memilih Check Licence secara dinamik).
4. **Satu perkara** yang anda perbaiki melalui penilaian (SESI 14).
5. **Postur *deployment*** — kenapa *on-premise*/Ollama untuk data JPJ?

### Rubrik penilaian

| Kriteria | Wajaran | Apa yang dinilai |
|----------|---------|-------------------|
| **Reka bentuk *workflow*** | 20% | Susunan *node* kemas, ejen/RAG betul, guna *tool* sesuai |
| **Document ingestion** | 20% | Dokumen di-*chunk*, ter-*embed* & tersimpan dengan metadata |
| **Ketepatan *retrieval*** | 20% | Jawapan berpaksikan *chunk* betul; sitasi tepat |
| **Fungsi chatbot / ejen** | 20% | Pembantu menjawab pelbagai soalan; pilih *tool* dinamik |
| **Pembentangan & dokumentasi** | 20% | Demo jelas, faham sistem sendiri, boleh terang setiap bahagian |

> Peserta yang lengkapkan semua latihan, implementasi RAG Hari 2, projek capstone & pembentangan menerima **Sijil Penyertaan** — *Building RAG & Agentic AI Applications with n8n*.

---

## Penutup — Ringkasan & Langkah Seterusnya

Hari ini anda telah:

1. ✅ Faham **AI Agent** — beza *agent* vs *workflow*, dan **kenapa** ejen lebih fleksibel (tetapi kurang boleh diramal).
2. ✅ Faham **tool calling** — bagaimana LLM memilih & memanggil alat berdasarkan **penerangan alat**.
3. ✅ Bina **Ejen Perkhidmatan JPJ** berbilang alat (KB + status lesen + rekod pemohon + tiket) dengan **pemilihan alat dinamik**.
4. ✅ Pelajari **RAG lanjutan** — metadata filtering, hybrid search, re-ranking, parent-child & multi-document retrieval.
5. ✅ Pelajari **penilaian & pengoptimuman** — set eval, groundedness/sitasi, gelung diagnosis retrieval-vs-prompt, kos.
6. ✅ Pelajari **deployment pengeluaran** — Docker, *on-premise*/residensi data (Ollama untuk JPJ), keselamatan & pemantauan.
7. ✅ Bentangkan **projek capstone** anda.

### Selepas kursus

- **Gantikan dokumen contoh** dengan dokumen rasmi JPJ yang sah & terkini (melalui saluran yang diluluskan).
- **Ujian dengan pengguna sebenar** (pegawai kaunter) sebelum lancar meluas.
- **Kekalkan set penilaian** — jalankan semula setiap kali dokumen/prompt berubah.
- **Mulakan dengan *on-premise*** untuk data sensitif; kembangkan skop secara berperingkat.

> 🎤 **Nota penceramah/jurulatih:** [`nota-penceramah.md`](./nota-penceramah.md) — pemasaan, *talking points*, tip demo ejen & pengendalian capstone untuk Hari 3.

> **Prinsip penutup kursus:** *AI membantu, anda memandu.* Pembantu ini alat untuk **membantu** pegawai JPJ — bukan menggantikan pertimbangan mereka. Sentiasa **semak jawapan terhadap sumber rasmi**, dan **uji sebelum percaya**.
