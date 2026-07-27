# Hari 1 — Asas AI, LLM & RAG + n8n Pertama

Panduan langkah demi langkah untuk **hari pertama** kursus *Building Retrieval-Augmented Generation (RAG) & Agentic AI Applications with n8n* (kod **RAG-N8N-JPJ-101**), disediakan untuk **Jabatan Pengangkutan Jalan Malaysia (JPJ)**. Nota ini mengikut **aturcara rasmi SESI 1–5** — lihat [`../JADUAL.md`](../JADUAL.md) — bukan susunan bebas.

Projek kursus: **Pembantu Pintar JPJ** — pembantu AI yang menjawab soalan berdasarkan **dokumen rasmi JPJ** (Akta Pengangkutan Jalan 1987, prosedur lesen memandu, pendaftaran & pindah milik kenderaan, saman/kompaun, SOP dalaman, pekeliling). Sepanjang 3 hari kita bina pembantu ini secara **hands-on dalam n8n dengan minimum pengekodan**.

> **Nota untuk pemula:** Anda **tidak perlu** tahu pengaturcaraan langsung. Setiap konsep diterangkan perlahan-lahan — termasuk **kenapa** ia wujud, bukan sekadar cara guna. Kita sentiasa mula dengan *why*, kemudian baru *how*.

> **Konvensyen bahasa:** Penerangan ditulis dalam **Bahasa Melayu**, tetapi nama *node* n8n, kunci JSON, kod & istilah teknikal dikekalkan dalam **Bahasa Inggeris** — amalan standard industri yang kita ikut sepanjang kursus.

> **Cara guna nota ini:** Bahagian di bawah menerangkan **konsep** setiap sesi — kenapa sesuatu wujud, apa ia buat, dan bagaimana ia terpakai kepada kerja JPJ. Latihan hands-on **langkah demi langkah** ada dalam [`snippets/lab.md`](./snippets/lab.md). Baca bahagian konsep yang sepadan di sini dahulu, kemudian pindah ke lab untuk cuba sendiri.

---

## Fokus Hari Ini

Hari 1 ialah hari **memahami** sebelum **membina**. Kita habiskan hari dengan satu *workflow* AI pertama yang benar-benar berjalan dalam n8n. Rujukan rasmi bagi setiap topik (untuk rujuk selepas kelas):

| Topik | Rujukan rasmi |
|-------|----------------|
| Automasi *workflow* n8n | [docs.n8n.io](https://docs.n8n.io) |
| *AI nodes* dalam n8n | [docs.n8n.io/advanced-ai](https://docs.n8n.io/advanced-ai/) |
| OpenAI API & model | [platform.openai.com/docs](https://platform.openai.com/docs) |
| Token & pengiraan token | [platform.openai.com/tokenizer](https://platform.openai.com/tokenizer) |
| *Prompt engineering* | [platform.openai.com/docs/guides/prompt-engineering](https://platform.openai.com/docs/guides/prompt-engineering) |
| Claude (Anthropic) | [docs.anthropic.com](https://docs.anthropic.com) |
| Gemini (Google) | [ai.google.dev](https://ai.google.dev) |
| Ollama (model tempatan/*on-premise*) | [ollama.com](https://ollama.com) |
| Embeddings (OpenAI) | [platform.openai.com/docs/guides/embeddings](https://platform.openai.com/docs/guides/embeddings) |
| Qdrant (pangkalan data vektor) | [qdrant.tech/documentation](https://qdrant.tech/documentation/) |
| pgvector | [github.com/pgvector/pgvector](https://github.com/pgvector/pgvector) |

---

## Jadual Hari Ini

Disalin daripada [`../JADUAL.md`](../JADUAL.md) — **HARI 1: Asas AI, LLM & RAG + n8n Pertama**.

| Masa | Agenda |
|------|--------|
| 8.30 – 9.00 pagi | Pendaftaran Peserta & Minum Pagi |
| **9.00 – 10.30 pagi** | **SESI 1: Pengenalan Kecerdasan Buatan (AI)** — Evolusi AI · ML vs Generative AI · Landskap AI semasa · AI dalam sektor awam · Peluang AI dalam pengangkutan jalan & pelesenan · 🧠 **Bengkel:** Kenal pasti kes guna AI dalam JPJ |
| **10.30 – 1.00 tgh** | **SESI 2: Memahami Large Language Models (LLM)** — Apa itu LLM? · GPT/Claude/Gemini/Ollama · Token & context window · Asas *prompt engineering* · Halusinasi & batasan · 💻 **Latihan:** Bengkel *prompt engineering* guna soalan gaya-JPJ |
| 1.00 – 2.30 petang | Rehat dan Makan Tengah Hari |
| **2.30 – 3.45 petang** | **SESI 3: Memahami Retrieval-Augmented Generation (RAG)** — Kenapa LLM biasa tidak cukup · Batasan pengetahuan model · Manfaat RAG (ketepatan, petikan, terkini) · 👥 **Aktiviti:** Reka pembantu pengetahuan untuk bahagian anda |
| **3.45 – 4.15 petang** | **SESI 4: Embeddings & Pangkalan Data Vektor** — Apa itu *embeddings*? · Carian semantik · Persamaan kosinus · Qdrant/Pinecone/Weaviate/Chroma/pgvector · 🔎 **Demo:** Visualisasi carian semantik |
| **4.15 – 5.00 petang** | **SESI 5: Pengenalan n8n** — Apa itu n8n? · *Nodes* & *connections* · Pengurusan *credentials* · *AI nodes* · 💻 **Lab:** Bina *workflow* AI pertama (Webhook → AI → Response) |
| 5.00 petang | Bersurai |

**Hasil Hari 1:** Peserta faham AI/LLM/RAG dan sudah membina *API endpoint* berkuasa AI pertama dalam n8n.

> Hari ini **belum** menyentuh Qdrant sebenar, *chunking* dokumen, atau *ingestion pipeline* penuh — semua itu **Hari 2**. Fokus hari ini **semata-mata** faham konsep + satu *workflow* AI ringkas yang berjalan.

---

## SESI 1 (9.00 – 10.30) — Pengenalan Kecerdasan Buatan (AI)

**Kenapa kita mula dengan konsep AI, bukan terus buka n8n?** Kerana tanpa faham *apa* yang AI boleh dan *tidak boleh* buat, mudah untuk salah guna — contohnya percaya bulat-bulat jawapan AI tentang kadar kompaun tanpa semak sumber. Sesi ini membina "peta mental" supaya keputusan teknikal sepanjang kursus masuk akal.

### Evolusi AI — dari peraturan tegar kepada model yang belajar

*Artificial Intelligence* (AI / Kecerdasan Buatan) ialah istilah payung untuk **komputer yang melakukan tugas yang biasanya perlukan kepintaran manusia** — memahami bahasa, mengecam corak, membuat keputusan. Ia berkembang dalam beberapa gelombang:

```
1) Sistem berasaskan peraturan (Rule-based, ~1960–1990an)
   Manusia tulis SEMUA peraturan: "JIKA umur < 17 MAKA tak layak lesen".
   Tepat untuk peraturan tetap, tapi rapuh — tak boleh urus kes luar senarai.

2) Machine Learning (ML, ~1990an–2010an)
   Komputer BELAJAR corak daripada data contoh, bukan diberi peraturan.
   Cth: ramal risiko dari sejarah data. Perlu data berlabel + ciri (features).

3) Deep Learning (~2012 ke atas)
   ML guna "neural network" berlapis-lapis — pandai kenal imej, suara, teks.

4) Generative AI (~2020 ke atas)
   Bukan sekadar KENAL corak, tapi HASILKAN kandungan baharu:
   ayat, ringkasan, jawapan, kod, imej. Inilah "AI" yang orang maksudkan hari ini.
```

**Analogi JPJ:** Sistem *rule-based* seperti borang semakan kelayakan yang tegar — jika soalan di luar kotak, ia buntu. *Machine Learning* seperti pegawai berpengalaman yang belajar mengenali corak permohonan bermasalah daripada ribuan kes lepas. *Generative AI* pula boleh **menulis semula** penjelasan prosedur dalam ayat mudah untuk orang awam — sesuatu yang dua generasi sebelumnya tidak mampu.

### ML vs Generative AI — beza yang penting

| | Machine Learning "klasik" | Generative AI (LLM) |
|---|---|---|
| **Tugas** | Ramal / klasifikasi (label, nombor) | Hasilkan teks/jawapan baharu |
| **Contoh output** | "Permohonan ini: berisiko / tidak" | "Berikut ringkasan prosedur pindah milik…" |
| **Input pengguna** | Data berstruktur (medan borang) | Bahasa biasa ("soalan macam bercakap") |
| **Contoh dalam JPJ** | Kesan corak saman tertunggak luar biasa | Jawab soalan kaunter dalam ayat penuh |

Kursus ini fokus pada **Generative AI**, khususnya *Large Language Models* (LLM) — enjin di sebalik pembantu yang menjawab dalam bahasa biasa.

### Landskap AI semasa & AI dalam sektor awam

Model AI hari ini datang daripada beberapa penyedia: **OpenAI** (GPT), **Anthropic** (Claude), **Google** (Gemini), dan model **sumber terbuka** (*open-source*) yang boleh dijalankan sendiri seperti **Llama** dan **Mistral** melalui **Ollama**. Perbezaan penting untuk agensi kerajaan: model *cloud* mudah tetapi data keluar ke pelayan luar; model tempatan (Ollama, *on-premise*) menyimpan data **dalam infrastruktur kawalan JPJ** — kritikal untuk dokumen sensitif. (Kita dalami residensi data pada **Hari 3**.)

Dalam sektor awam, AI mula digunakan untuk: pembantu maya menjawab soalan orang awam 24/7, ringkasan dokumen dasar yang panjang, carian pintar dalam pekeliling & SOP, serta bantuan latihan pegawai baharu.

### Peluang AI dalam pengangkutan jalan & pelesenan

Antara peluang paling jelas untuk JPJ:

- **Pertanyaan lesen** — orang awam & pegawai kaunter kerap tanya prosedur pembaharuan lesen (LMM/CDL/GDL), kelayakan, dokumen diperlukan.
- **Saman & kompaun** — cara semak kompaun tertunggak, kadar, prosedur rayuan.
- **Pendaftaran & pindah milik kenderaan** — langkah, borang, bayaran.
- **Carian SOP & pekeliling dalaman** — bantu pegawai baharu belajar prosedur atas permintaan, tanpa membelek fail tebal.

**Kenapa ini penting:** pembantu AI berpaksikan dokumen rasmi memberi jawapan **konsisten** antara cawangan, **pantas** (beberapa saat), dan **berpetikan sumber** — mengurangkan beban kaunter dan risiko jawapan bercanggah.

> 🧠 **Bengkel SESI 1** ada dalam [Latihan 1, lab](./snippets/lab.md#latihan-1--bengkel-kes-guna-ai-dalam-jpj) — anda akan senaraikan kes guna AI sebenar untuk bahagian anda dan nilai mana paling bernilai.

---

## SESI 2 (10.30 – 1.00) — Memahami Large Language Models (LLM)

### Apa itu LLM?

*Large Language Model* (LLM) ialah model AI yang dilatih pada **jumlah teks yang sangat besar** (buku, laman web, artikel) untuk melakukan satu perkara asas dengan sangat baik: **meramal perkataan seterusnya**. Daripada keupayaan mudah itu muncul keupayaan mengagumkan — menjawab soalan, meringkas, menterjemah, menulis.

**Analogi:** Bayangkan pegawai yang telah membaca berjuta-juta muka surat dan menjadi sangat mahir "melengkapkan ayat". Beri dia permulaan ayat, dia teruskan dengan perkataan yang **paling berkemungkinan** betul. LLM tidak "tahu fakta" seperti pangkalan data — ia meramal teks berdasarkan corak yang dipelajari. Inilah sebabnya ia kadang-kadang **yakin tetapi salah** (kita bincang "halusinasi" di bawah).

### GPT, Claude, Gemini & model sumber terbuka (Ollama)

| Model | Penyedia | Sifat |
|-------|----------|-------|
| **GPT** (cth. GPT-4o) | OpenAI | Popular, mudah guna melalui API — kita guna dalam lab hari ini |
| **Claude** | Anthropic | Kuat untuk teks panjang & arahan teliti |
| **Gemini** | Google | Bersepadu dengan ekosistem Google |
| **Llama, Mistral** (*open-source*) | Meta / Mistral, dijalankan via **Ollama** | Boleh jalan **tempatan/on-premise** — data tak keluar dari mesin JPJ |

**Untuk JPJ:** kita guna **OpenAI** semasa belajar (paling mudah), tetapi tunjuk **Ollama** sebagai pilihan *on-premise* untuk data sensitif. Stack rasmi kursus (lihat [`../README.md`](../README.md)) menetapkan kedua-duanya.

### Token & context window — "unit" & "ingatan" LLM

LLM tidak membaca perkataan seperti manusia; ia memecah teks kepada **token** — kepingan perkataan. Kasарnya, **1 token ≈ 4 aksara** teks Inggeris, atau **~¾ perkataan**. Contoh:

```
Teks:   "Lesen memandu tamat tempoh"
Token:  ["Les", "en", " mem", "andu", " tamat", " tempoh"]  (anggaran)
        ~6 token untuk 4 perkataan
```

Kenapa token penting?

- **Kos & kelajuan** — penyedia mengira bayaran mengikut **bilangan token** (input + output). Lebih panjang prompt/jawapan, lebih mahal & lambat.
- **Context window** — had **jumlah token** yang boleh diproses model dalam **satu** permintaan (prompt + jawapan sekali). Fikirkan ia sebagai **saiz meja kerja**: apa sahaja yang tidak muat di atas meja, model **tidak nampak**. Model moden ada *context window* besar (puluhan hingga ratusan ribu token), tetapi ia tetap **terhad**.

> **Kenapa ini penting untuk RAG (Hari 2):** kita tidak boleh "tampal" seluruh koleksi pekeliling JPJ ke dalam satu prompt — terlalu besar untuk *context window*, dan mahal. Sebaliknya, RAG hanya menyuntik **bahagian yang relevan** sahaja. Token & *context window* ialah sebab teknikal **kenapa RAG wujud**.

Uji sendiri berapa token sesuatu teks di [platform.openai.com/tokenizer](https://platform.openai.com/tokenizer).

### Asas prompt engineering

*Prompt* ialah arahan/soalan yang anda beri kepada LLM. *Prompt engineering* ialah seni menulis arahan supaya jawapan **tepat, konsisten & berguna**. Prinsip asas:

1. **Jelas & khusus** — nyatakan dengan tepat apa anda mahu. "Ringkaskan" < "Ringkaskan prosedur di bawah kepada 5 langkah bernombor untuk orang awam".
2. **Beri konteks** — beri maklumat latar yang diperlukan (dokumen, peranan). "Anda pembantu kaunter JPJ. Jawab berdasarkan teks berikut sahaja…".
3. **Tetapkan format** — minta bentuk output (senarai, jadual, ayat pendek).
4. **Beri contoh** (*few-shot*) — tunjuk 1–2 contoh soalan+jawapan yang baik.
5. **Tetapkan sempadan** — "Jika maklumat tiada dalam teks, katakan 'saya tidak pasti' — jangan reka."

**Struktur prompt yang lazim (gaya JPJ):**

```
[PERANAN]    Anda pembantu maklumat JPJ untuk pegawai kaunter.
[ARAHAN]     Jawab soalan berdasarkan KONTEKS sahaja. Bahasa mudah, ringkas.
[KONTEKS]    <petikan SOP/pekeliling diletak di sini>
[SOALAN]     Berapa lama tempoh pembaharuan lesen boleh dibuat lebih awal?
[FORMAT]     Jawab dalam 2–3 ayat + petik nombor perenggan sumber.
```

Prinsip nombor 2 & 5 di atas ialah **teras RAG** — kita "beri konteks" secara automatik dan "tetapkan sempadan" supaya AI tidak mereka. Lebih lanjut dalam [`../nota/07-prompt-engineering.md`](../nota/07-prompt-engineering.md).

### Halusinasi & batasan

**Halusinasi** ialah bila LLM memberi jawapan yang **kedengaran yakin & munasabah tetapi sebenarnya salah atau direka**. Ia berlaku kerana LLM **meramal teks berkemungkinan**, bukan mengambil fakta yang disahkan. Contoh berbahaya untuk JPJ:

```
Soalan:  "Berapa kadar kompaun bagi lesen tamat tempoh 3 tahun?"
LLM biasa (tanpa dokumen): "Kadarnya RM300."   ← nombor DIREKA, kelihatan yakin
```

Batasan LLM biasa yang perlu diingat:

- **Pengetahuan terhad tarikh** — model dilatih sehingga tarikh tertentu; tidak tahu pekeliling terbaru.
- **Tidak tahu dokumen dalaman JPJ** — mustahil ada SOP dalaman anda dalam data latihannya.
- **Tiada sumber** — tidak boleh tunjuk "dari mana" jawapan datang.
- **Boleh berubah-ubah** — soalan sama boleh dapat jawapan sedikit berbeza.

> **Inilah masalah yang RAG selesaikan** (SESI 3). Dengan mengikat jawapan pada **dokumen sebenar JPJ**, kita paksa AI menjawab **daripada sumber**, bukan daripada tekaan — dan boleh **petik** sumbernya.

> 💻 **Latihan prompt engineering SESI 2** ada dalam [Latihan 2, lab](./snippets/lab.md#latihan-2--bengkel-prompt-engineering-soalan-gaya-jpj) — anda perhalusi prompt guna soalan sebenar gaya-JPJ dan lihat halusinasi berlaku secara langsung.

---

## SESI 3 (2.30 – 3.45) — Memahami Retrieval-Augmented Generation (RAG)

### Kenapa LLM biasa tidak mencukupi

Kita sudah lihat batasannya di SESI 2. Ringkasnya, LLM biasa tidak tahu **dokumen JPJ anda**, tidak tahu **maklumat terkini**, dan boleh **mereka jawapan**. Untuk pembantu yang mesti tepat pada prosedur rasmi, ini tidak boleh diterima.

**Dua cara "mengajar" LLM tentang dokumen anda:**

| Kaedah | Idea | Untuk JPJ |
|--------|------|-----------|
| *Fine-tuning* (latih semula) | Latih model dengan dokumen anda | Mahal, lambat, perlu ulang setiap kali dokumen berubah — **tidak sesuai** |
| **RAG** (Retrieval-Augmented Generation) | **Cari** petikan relevan, **suntik** ke prompt semasa ditanya | Murah, pantas, sentiasa ikut dokumen terkini — **inilah pilihan kita** |

### Apa itu RAG?

**RAG = Retrieval (cari) + Augmented (tambah) + Generation (jana jawapan).** Idea mudah: apabila pengguna bertanya, kita **cari** dahulu bahagian dokumen yang relevan, **tambah** petikan itu ke dalam prompt sebagai "konteks", dan barulah LLM **menjana** jawapan — berdasarkan petikan itu, bukan tekaan.

**Analogi peperiksaan:** LLM biasa seperti pelajar menjawab **dari ingatan** (mungkin lupa/salah). RAG seperti peperiksaan **buku terbuka** — pelajar yang sama dibenarkan **rujuk buku teks rasmi** dahulu sebelum menjawab. Jawapan jadi jauh lebih tepat kerana **berpaksikan sumber**.

### Pipeline RAG (gambaran besar)

Inilah aliran yang kita bina sepanjang Hari 2. Fahami bentuknya sekarang:

```
                              ┌─────────────────────────────────────┐
   Soalan Pengguna            │  DOKUMEN JPJ (disediakan lebih awal) │
   "Dokumen apa perlu         │  SOP · pekeliling · prosedur lesen   │
    untuk pindah milik?"      └───────────────┬─────────────────────┘
        │                                     │ (Hari 2: pecah kepada
        ▼                                     ▼  kepingan + embed + simpan)
   [1] Embedding  ─────────►  [2] Vector Search ────►  Vector DB (Qdrant)
   (tukar soalan jadi                │
    vektor nombor)                   │ padankan makna, bukan perkataan
        │                            ▼
        │                  [3] Relevant Context
        │                  (petikan dokumen paling hampir maknanya)
        │                            │
        └──────────────┬─────────────┘
                       ▼
              [4] LLM (GPT/Claude/Ollama)
              prompt = soalan + petikan konteks
                       │
                       ▼
              [5] Jawapan + petikan sumber
              "Anda perlukan borang JPK 3, kad pengenalan asal…
               (Sumber: SOP Pindah Milik, perenggan 4.2)"
```

- **[1] Embedding** — soalan ditukar kepada senarai nombor (*vektor*) yang mewakili **maknanya**. (Butiran SESI 4.)
- **[2] Vector Search** — cari petikan dokumen yang vektornya **paling hampir** dengan vektor soalan → padanan **makna**, bukan padanan perkataan.
- **[3] Relevant Context** — petikan teratas diambil sebagai bahan rujukan.
- **[4] LLM** — diberi soalan **dan** petikan, dan diarah menjawab **daripada petikan itu sahaja**.
- **[5] Jawapan bercitasi** — jawapan tepat yang boleh **menunjuk sumbernya**.

### Manfaat RAG

- **Ketepatan** — jawapan diikat pada dokumen sebenar → kurang halusinasi.
- **Petikan sumber** — boleh tunjuk "dari SOP mana, perenggan berapa" → pegawai boleh sahkan.
- **Sentiasa terkini** — tukar/tambah dokumen, pembantu terus tahu; tak perlu latih semula model.
- **Kawalan data** — dokumen kekal dalam sistem anda; dengan Ollama, langsung tidak keluar.
- **Kos rendah** — jauh lebih murah daripada *fine-tuning*.

> **Prinsip teras kursus:** *AI membantu, anda memandu.* Setiap jawapan RAG mesti boleh disemak terhadap dokumen sumber. Lebih lanjut: [`../nota/03-apa-itu-rag.md`](../nota/03-apa-itu-rag.md).

> 👥 **Aktiviti SESI 3** ada dalam [Latihan 3, lab](./snippets/lab.md#latihan-3--reka-pembantu-pengetahuan-untuk-bahagian-anda) — anda reka di atas kertas satu pembantu pengetahuan untuk bahagian anda sendiri.

---

## SESI 4 (3.45 – 4.15) — Embeddings & Pangkalan Data Vektor

Sesi ini menjawab satu soalan teknikal daripada pipeline RAG: **bagaimana komputer tahu dua ayat "sama makna" walaupun perkataannya berbeza?** Jawapannya: **embeddings**.

### Apa itu embeddings?

**Embedding** ialah cara menukar teks kepada **senarai nombor** (satu *vektor*) yang mewakili **maknanya**. Teks yang **serupa maknanya** menghasilkan vektor yang **berdekatan**; teks berbeza makna menghasilkan vektor yang berjauhan.

```
"pembaharuan lesen memandu"   → [0.12, -0.88, 0.31, ... ]  (cth. 1536 nombor)
"renew driving licence"       → [0.11, -0.85, 0.29, ... ]  ← hampir sama! (makna serupa)
"kadar kompaun saman"         → [0.77,  0.04, -0.52, ...]  ← jauh berbeza (makna lain)
```

Perhatikan: **perkataan berbeza bahasa** ("pembaharuan lesen" vs "renew licence") tetapi **vektor hampir sama** kerana **maknanya** sama. Inilah kuasa embeddings — ia menangkap **makna**, bukan ejaan. Model embedding (cth. `text-embedding-3-small` dari OpenAI) yang melakukan penukaran ini.

### Carian semantik vs carian kata kunci

| | Carian kata kunci (biasa) | Carian **semantik** (embeddings) |
|---|---|---|
| Padankan | Perkataan **sama** | **Makna** yang sama |
| "renew licence" jumpa "pembaharuan lesen"? | ❌ Tidak (ejaan beza) | ✅ Ya (makna sama) |
| Untuk JPJ | Terlepas soalan berbeza ayat | Faham maksud walau ayat berbeza |

Orang awam bertanya dengan **pelbagai cara** ("nak tukar nama atas geran", "pindah milik kereta", "transfer ownership"). Carian semantik memadankan **maksud** — jadi pembantu jumpa SOP yang betul walaupun perkataan pengguna tak sama dengan dokumen.

### Persamaan kosinus (cosine similarity)

Bagaimana komputer mengukur "sehampir mana" dua vektor? Cara paling lazim ialah **cosine similarity** — ia mengukur **sudut** antara dua vektor, bukan jaraknya.

```
   cosine similarity = 1.0   →  arah SAMA          →  makna sangat serupa
   cosine similarity = 0.0   →  serong 90°         →  tiada kaitan
   cosine similarity = -1.0  →  arah BERTENTANGAN  →  makna berlawanan
```

Bayangkan setiap vektor sebagai **anak panah** dari titik pusat. Jika dua anak panah menghala **arah yang sama** (sudut kecil), maknanya serupa (skor hampir 1.0). Jika serong jauh, maknanya berbeza. *Vector search* dalam pipeline RAG pada asasnya ialah: "cari petikan yang **cosine similarity**-nya dengan soalan **paling tinggi**".

> **Kenapa "sudut", bukan "jarak"?** Sudut menumpu pada **arah** (makna) dan tidak terpengaruh oleh panjang teks. Ini membuatkannya sesuai untuk membandingkan makna ayat pendek dengan perenggan panjang.

### Vector indexing & pangkalan data vektor

Koleksi dokumen JPJ boleh menghasilkan **ribuan hingga jutaan** vektor. Membandingkan soalan dengan **setiap** vektor satu-satu terlalu lambat. **Pangkalan data vektor** (*vector database*) menyimpan vektor + membina **indeks** khas supaya carian "vektor paling hampir" jadi **sangat pantas** (sepersekian saat), walaupun untuk jutaan vektor.

| Vector DB | Nota |
|-----------|------|
| **Qdrant** | **Pilihan rasmi kursus** — sumber terbuka, boleh *self-host* (on-prem), mesra Docker |
| Pinecone | Perkhidmatan *cloud* terurus (managed) |
| Weaviate | Sumber terbuka, kaya ciri |
| Chroma | Ringan, sesuai untuk prototaip/belajar |
| pgvector | Sambungan (*extension*) untuk PostgreSQL — guna DB sedia ada |

Kita guna **Qdrant** kerana ia boleh dijalankan **sepenuhnya dalam infrastruktur JPJ** (penting untuk data sensitif) dan mudah dipasang via Docker. Kita **sediakan Qdrant sebenar pada Hari 2** — hari ini cukup faham *peranannya*. Lebih lanjut: [`../nota/04-embeddings-vector-db.md`](../nota/04-embeddings-vector-db.md).

> 🔎 **Demo carian semantik SESI 4** ada dalam [Latihan 4, lab](./snippets/lab.md#latihan-4--demo-visualisasi-carian-semantik) — anda susun ayat mengikut persamaan makna secara manual untuk "merasai" cara embeddings berfikir.

---

## SESI 5 (4.15 – 5.00) — Pengenalan n8n + Workflow AI Pertama

### Apa itu n8n?

**n8n** (sebut "n-eight-n") ialah alat **automasi *workflow*** visual. Daripada menulis kod, anda **sambung kotak-kotak** (dipanggil *nodes*) pada satu kanvas untuk mereka bentuk aliran kerja automatik: "bila **X** berlaku, buat **Y**, kemudian **Z**". Ia sesuai untuk kursus ini kerana kita boleh bina pembantu AI **secara visual** dengan minimum pengekodan.

**Analogi:** n8n seperti **carta alir yang hidup**. Anda lukis carta alir (kotak & anak panah), dan ia **benar-benar berjalan** — setiap kotak melakukan satu tugas sebenar, dan data mengalir mengikut anak panah dari satu kotak ke kotak seterusnya.

### Nodes & connections

- **Node** — satu "kotak" yang melakukan **satu** tugas: cetuskan aliran, panggil AI, hantar balasan, panggil pangkalan data, dsb.
- **Connection** — "wayar" (anak panah) yang menyambung *output* satu node ke *input* node seterusnya. Data (dalam bentuk JSON) mengalir melalui wayar ini.
- **Trigger node** — node **pertama** yang memulakan *workflow*. Contoh: *Manual Trigger* (tekan butang untuk uji), atau *Webhook* (bila ada permintaan HTTP masuk).

```
   ┌──────────────┐      ┌──────────────┐      ┌──────────────┐
   │  Webhook      │ ───► │  AI / OpenAI  │ ───► │  Respond to   │
   │  (trigger:    │ data │  (proses      │ data │  Webhook      │
   │   soalan masuk)│      │   soalan)     │      │  (hantar      │
   └──────────────┘      └──────────────┘      │   jawapan)     │
                                               └──────────────┘
       node            connection (wayar)           node
```

### Pengurusan credentials

*Credentials* ialah **kunci/kata laluan** untuk menyambung ke perkhidmatan luar (cth. **kunci API OpenAI**). n8n menyimpannya **secara berasingan & disulitkan** — anda masukkan **sekali**, kemudian **guna semula** dalam banyak node **tanpa** menaip semula kunci itu, dan **tanpa** ia terpapar dalam *workflow*.

> **Kenapa ini penting untuk JPJ:** kunci API & kata laluan **tidak** patut ditulis terus dalam *workflow* atau dikongsi dalam mesej. Sistem *credentials* n8n memisahkan rahsia daripada logik — amalan keselamatan asas. Lebih lanjut tentang tadbir urus & keselamatan: [`../nota/08-governance-keselamatan.md`](../nota/08-governance-keselamatan.md).

### AI nodes dalam n8n

n8n mempunyai **node khas AI** yang memudahkan kerja dengan LLM — anda hanya pilih model, sambung *credentials*, dan tetapkan prompt. Untuk lab hari ini kita guna node AI/OpenAI ringkas: ia menerima soalan, menghantarnya ke model, dan memulangkan jawapan. (Node AI lanjutan seperti *AI Agent* & sambungan ke *vector store* datang **Hari 2 & 3**.)

### Lab: Workflow AI Pertama (Webhook → AI → Response)

Inilah puncak Hari 1 — satu *API endpoint* berkuasa AI yang **benar-benar berjalan**. Bentuk akhirnya:

```
   Webhook  ───►  OpenAI / AI  ───►  Respond to Webhook
   (terima         (jana jawapan       (pulangkan jawapan
    soalan HTTP)     daripada soalan)    sebagai respons HTTP)
```

Apa yang berlaku: seseorang menghantar soalan ke *URL webhook* → n8n menyerahkan soalan kepada model AI → jawapan model dipulangkan sebagai respons. Ini **rangka asas** setiap pembantu yang kita bina — Hari 2 kita **selitkan langkah RAG** (embedding + vector search) di antara *Webhook* dan *AI* supaya jawapan berpaksikan dokumen JPJ.

> ⚠️ **Nota hari ini:** *workflow* pertama ini menjawab **dari pengetahuan umum model sahaja** — ia **belum** RAG, jadi **belum** patut dipercayai untuk jawapan JPJ sebenar. Tujuannya semata-mata **membiasakan** anda dengan n8n: cipta *workflow*, tambah node, set *credentials*, sambung wayar, dan **execute**. Ketepatan berpaksikan dokumen datang Hari 2.

**Langkah demi langkah penuh** (cipta *workflow* → tambah *Manual Trigger*/*Webhook* → tambah node AI → set *credentials* → sambung node → *execute* → baca output) ada dalam [Latihan 5, lab](./snippets/lab.md#latihan-5--bina-workflow-ai-pertama-webhook--ai--response). Templat *workflow* rujukan penuh yang boleh diimport: [`../templates/workflows/01-first-ai-workflow.json`](../templates/workflows/01-first-ai-workflow.json). Kenapa n8n & bila ia sesuai: [`../nota/01-kenapa-n8n.md`](../nota/01-kenapa-n8n.md).

---

## Penutup — Ringkasan & Langkah Seterusnya

### Ringkasan Hari 1

Hari ini anda telah:

1. ✅ Faham **evolusi AI** — *rule-based* → *machine learning* → *deep learning* → *generative AI* — dan beza **ML vs Generative AI**.
2. ✅ Kenal pasti **peluang AI dalam JPJ** (pertanyaan lesen, saman, carian SOP) dalam bengkel.
3. ✅ Faham **LLM** — model meramal teks, **token & context window**, penyedia (GPT/Claude/Gemini/Ollama), asas **prompt engineering**, dan bahaya **halusinasi**.
4. ✅ Faham **RAG** — kenapa LLM biasa tak cukup, dan bagaimana "cari → suntik konteks → jana jawapan bercitasi" menyelesaikannya.
5. ✅ Faham **embeddings, carian semantik, cosine similarity** & peranan **vector DB** (Qdrant & rakan-rakan).
6. ✅ Faham asas **n8n** (nodes, connections, credentials, AI nodes) dan **membina *workflow* AI pertama** yang berjalan.

### Konsep paling penting untuk dibawa ke Hari 2

- **RAG mengikat jawapan pada dokumen** → ketepatan + petikan sumber → **anti-halusinasi**.
- **Embeddings + cosine similarity** membolehkan carian **makna**, bukan perkataan.
- **n8n** membolehkan kita bina semua ini **secara visual**, node demi node.

### Apa Seterusnya — Hari 2 (SESI 6–10)

Esok kita **bina penyelesaian RAG lengkap**: pasang **Qdrant** sebenar via Docker, proses dokumen JPJ (*PDF extraction*, *chunking*, *metadata*), bina **ingestion workflow** (`Upload → Extract → Chunk → Embed → Store in Qdrant`) dan **retrieval workflow** (`Question → Embed → Vector Search → Context → LLM → Answer`) → menghasilkan **Pembantu Pengetahuan JPJ** yang berfungsi. Lihat [`../JADUAL.md`](../JADUAL.md).

Sebelum tamat kelas hari ini — pastikan *workflow* AI pertama anda **berjaya execute** tanpa ralat, dan kunci API OpenAI/Ollama anda sudah disimpan sebagai *credentials* dalam n8n.

---

> 🎤 **Nota jurulatih:** [`nota-penceramah.md`](./nota-penceramah.md) — nota persembahan (timing, poin utama, analogi, soalan lazim) untuk Hari 1.

> ⚠️ **Penafian dokumen contoh:** Semua "dokumen JPJ" yang dirujuk dalam kursus ini ialah **contoh sintetik untuk latihan sahaja** — bukan pekeliling/SOP rasmi. Kadar, nombor & prosedur dalam contoh **tidak** boleh dijadikan rujukan sebenar. Untuk penggunaan sebenar, gantikan dengan dokumen rasmi JPJ yang sah & terkini.
