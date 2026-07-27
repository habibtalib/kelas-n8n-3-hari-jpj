# Lab Hari 1 — Asas AI, LLM & RAG + n8n Pertama

Lab ini mengiringi [`../README.md`](../README.md) Hari 1. Ikut latihan **secara berurutan** — setiap latihan bina di atas kefahaman latihan sebelumnya. Baca bahagian konsep yang sepadan dalam `README.md` dahulu, kemudian buat latihan di sini.

> **Peraturan lab:** Tulis/uji **sendiri** dahulu sebelum lihat jawapan atau minta bantuan. Cara paling berkesan belajar AI & n8n ialah **mencuba secara langsung** — bukan sekadar membaca.

> **Konvensyen:** Penerangan dalam **Bahasa Melayu**; nama *node* n8n, kunci JSON, prompt teknikal & istilah dikekalkan dalam **Bahasa Inggeris**.

---

## Senarai Semak Persediaan (Setup Checklist)

Sebelum mula Latihan 0, pastikan berikut sudah **✓** (banyak yang akan disediakan bersama semasa kelas):

- [ ] Komputer riba dengan pelayar web moden (Chrome/Edge/Firefox)
- [ ] Akses ke satu *instance* **n8n** — sama ada n8n Cloud (percubaan) **atau** n8n tempatan via Docker. Panduan pilih & sediakan kedua-dua jalan: [`../nota/10-setup-n8n.md`](../../nota/10-setup-n8n.md) (langkah penuh Docker: [`../nota/05-setup-docker.md`](../../nota/05-setup-docker.md), didalami Hari 2)
- [ ] Satu **kunci API OpenAI** *(atau)* **Ollama** tempatan yang berjalan — untuk node AI dalam Latihan 5
- [ ] Sambungan internet stabil

> Jika n8n belum sedia semasa Latihan 0–4, tak mengapa — Latihan 0–4 **tidak** perlukan n8n (ia latihan konsep, kertas & pelayar). Anda hanya perlukan n8n mulai **Latihan 5**. Minta bantuan fasilitator semasa rehat jika persediaan n8n bermasalah.

---

## Latihan 0 — Orientasi (Alatan & Istilah)

**Objektif:** Kenali alatan & istilah teras sebelum menyentuh apa-apa yang teknikal.

1. Buka [platform.openai.com/tokenizer](https://platform.openai.com/tokenizer) dalam pelayar — alat ini kita guna di Latihan 2 untuk "melihat" token.
2. Buka [docs.n8n.io](https://docs.n8n.io) dan lihat sekilas antara muka kanvas n8n dalam dokumentasi — kenali perkataan **node**, **connection**, **trigger**, **credentials**.
3. Padankan istilah dengan maksud (jawab dalam kepala atau tulis):

   | Istilah | Maksud (padankan) |
   |---------|-------------------|
   | LLM | a. Kunci/rahsia untuk sambung perkhidmatan luar |
   | Token | b. Model AI yang meramal & menjana teks |
   | Embedding | c. "Kotak" tugas dalam n8n |
   | Node | d. Kepingan perkataan yang LLM proses |
   | Credentials | e. Teks ditukar jadi vektor nombor (makna) |

4. **Soalan renungan:** Kenapa kita belajar konsep AI/LLM/RAG **dahulu**, sebelum membina dalam n8n? (Jawapan: supaya anda faham **kenapa** setiap node wujud & **bila** untuk mempercayai jawapan AI — bukan sekadar menyalin langkah.)

> Jawapan padanan: LLM→b, Token→d, Embedding→e, Node→c, Credentials→a.

✅ **Semakan:** Anda boleh menerangkan dalam satu ayat setiap istilah: LLM, token, embedding, node, credentials.

---

## Latihan 1 — Bengkel: Kes Guna AI dalam JPJ

**Objektif:** Kenal pasti di mana AI benar-benar berguna dalam kerja harian JPJ (padanan 🧠 Bengkel SESI 1).

Buat secara **berkumpulan kecil** (2–3 orang) atau individu, di atas kertas/papan putih:

1. Senaraikan **5 soalan** yang orang awam / pegawai kaunter kerap tanya di bahagian anda. Contoh pencetus:
   - Pertanyaan **lesen** (pembaharuan LMM/CDL/GDL, kelayakan, dokumen diperlukan)
   - **Saman/kompaun** (cara semak tertunggak, kadar, rayuan)
   - **Pendaftaran/pindah milik** kenderaan (borang, langkah, bayaran)
   - Carian **SOP/pekeliling** dalaman (bantu pegawai baharu)
2. Bagi **setiap** soalan, tandakan:
   - **Sumber jawapan** — dari dokumen mana ia sepatutnya datang? (SOP? pekeliling? akta?)
   - **Kekerapan** — berapa kerap ditanya (tinggi/sederhana/rendah)?
   - **Risiko silap** — apa jadi jika jawapan salah (tinggi/rendah)?
3. Bulatkan **1–2 kes** paling bernilai untuk automasi: **kerap ditanya + jawapan ada dalam dokumen + faedah besar bila betul**.

| Soalan lazim | Sumber dokumen | Kekerapan | Risiko silap | Calon automasi? |
|--------------|----------------|-----------|--------------|-----------------|
| *(contoh)* Berapa lama boleh perbaharui lesen lebih awal? | SOP pelesenan | Tinggi | Sederhana | ✅ |
| … | … | … | … | … |

4. **Perbincangan:** Kenapa kes yang "kerap + berpaksikan dokumen" ialah calon terbaik? (Petunjuk: RAG paling bersinar bila jawapan **sudah ada** dalam dokumen rasmi — kita cuma perlu **mencari & memetiknya**.)

✅ **Semakan:** Kumpulan anda menghasilkan **sekurang-kurangnya 5 soalan JPJ** dengan sumber dokumen, dan telah memilih **1–2 kes teratas** untuk automasi dengan alasan yang jelas.

---

## Latihan 2 — Bengkel Prompt Engineering (Soalan Gaya-JPJ)

**Objektif:** Rasa sendiri bagaimana **cara menulis prompt** mengubah kualiti jawapan — dan lihat **halusinasi** berlaku (padanan 💻 Latihan SESI 2). Gunakan **mana-mana** LLM yang ada akses: ChatGPT, Claude, Gemini, atau node AI dalam n8n.

### 2.1 — Lihat token

1. Buka [platform.openai.com/tokenizer](https://platform.openai.com/tokenizer).
2. Taip ayat gaya-JPJ, cth: `Prosedur pembaharuan lesen memandu kelas D yang telah tamat tempoh melebihi tiga tahun`.
3. Perhatikan **bilangan token** dan cara perkataan dipecah. Cuba ayat pendek vs panjang — sahkan sendiri: **lebih panjang = lebih banyak token**.

> **Kaitan RAG:** inilah sebab kita **tak boleh** tampal semua dokumen JPJ ke satu prompt — terlalu banyak token untuk *context window*. RAG hanya suntik **petikan relevan**.

### 2.2 — Prompt lemah lawan prompt kuat

Tanya LLM soalan gaya-JPJ dengan **dua** cara, banding jawapannya:

**Prompt LEMAH (kabur):**
```
Macam mana nak tukar geran kereta?
```

**Prompt KUAT (jelas + peranan + format + sempadan):**
```
Anda pembantu maklumat JPJ untuk pegawai kaunter.
Terangkan langkah pindah milik kenderaan persendirian kepada individu lain,
dalam 5 langkah bernombor, bahasa mudah untuk orang awam.
Jika ada langkah yang anda tidak pasti, nyatakan "sila sahkan dengan kaunter JPJ"
— jangan reka nombor bayaran atau nama borang.
```

Perhatikan perbezaan: prompt kuat beri **peranan**, **format** (5 langkah bernombor), dan **sempadan** (jangan reka).

### 2.3 — Saksikan halusinasi

Tanya LLM **tanpa** memberi apa-apa dokumen:
```
Berapa kadar kompaun tepat bagi lesen memandu yang tamat tempoh selama 3 tahun di Malaysia?
```

Perhatikan: LLM mungkin beri **nombor yang kelihatan yakin**. **Anda tiada cara untuk tahu ia betul** — ia mungkin **direka** (halusinasi). Inilah bahayanya LLM biasa untuk kerja JPJ.

### 2.4 — Kesan "buku terbuka" (pra-RAG secara manual)

Sekarang **tampal petikan** dahulu, kemudian tanya — tiru cara RAG bekerja secara manual:
```
KONTEKS (petikan SOP contoh — data latihan, bukan rasmi):
"Pembaharuan lesen boleh dibuat seawal 3 bulan sebelum tarikh tamat.
 Lesen yang tamat melebihi 3 tahun memerlukan ujian semula (JPJ L2/L5)."

ARAHAN: Jawab soalan berikut BERDASARKAN KONTEKS DI ATAS SAHAJA.
Jika jawapan tiada dalam konteks, katakan "maklumat tidak dinyatakan dalam sumber".

SOALAN: Adakah lesen yang tamat 4 tahun perlu ujian semula?
```

Perhatikan: sekarang jawapan **berpaksikan petikan** yang anda beri, dan boleh dirujuk balik. **Inilah intipati RAG** — Hari 2 kita automatikkan langkah "cari & tampal petikan" ini.

✅ **Semakan:** Anda dapat: (1) tunjuk prompt kuat menghasilkan jawapan lebih baik daripada prompt lemah, (2) saksikan satu contoh halusinasi (nombor direka), dan (3) buktikan jawapan jadi lebih boleh-dipercayai apabila petikan sumber diberi dahulu.

---

## Latihan 3 — Reka Pembantu Pengetahuan untuk Bahagian Anda

**Objektif:** Lakar reka bentuk pembantu RAG untuk bahagian **anda sendiri** — di atas kertas, belum dalam n8n (padanan 👥 Aktiviti SESI 3).

Isi kanvas reka bentuk ringkas ini:

1. **Nama pembantu:** _(cth. "Pembantu Pelesenan Cawangan")_
2. **Pengguna sasaran:** _(pegawai kaunter? orang awam? pegawai baharu?)_
3. **Dokumen sumber** (3–5): _(SOP mana? pekeliling? prosedur? — guna dokumen contoh, jangan dokumen sulit sebenar)_
4. **5 soalan contoh** yang pembantu mesti boleh jawab: _(ambil dari Latihan 1)_
5. **Bentuk jawapan yang diharapkan:** _(ringkas? bernombor? mesti petik sumber?)_
6. **Lukis pipeline RAG anda** — isi kotak dengan konteks bahagian anda:

```
Soalan pengguna:  "________________________________"
      │
      ▼
   [Embedding]  →  [Vector Search dalam Qdrant]  →  [Petikan relevan]
      │                                                    │
      └───────────────────────┬────────────────────────────┘
                              ▼
                       [LLM + prompt]
                              │
                              ▼
   Jawapan + petikan sumber:  "________________________________
                               (Sumber: __________, perenggan ___)"
```

7. **Semakan risiko:** Adakah dokumen sumber anda sensitif? Jika ya — tandakan bahawa **Ollama + on-premise** (bukan OpenAI cloud) ialah pilihan yang lebih selamat (didalami Hari 3).

✅ **Semakan:** Anda menghasilkan **satu lakaran pembantu** lengkap: nama, pengguna, 3–5 dokumen sumber, 5 soalan, bentuk jawapan, dan pipeline RAG yang dilukis dengan konteks bahagian anda.

---

## Latihan 4 — Demo: Visualisasi Carian Semantik

**Objektif:** "Rasai" cara embeddings menyusun teks mengikut **makna**, bukan perkataan (padanan 🔎 Demo SESI 4). Latihan kertas — tiada kod.

Diberi satu **ayat rujukan** dan beberapa **calon**. Tugas anda: susun calon dari **paling serupa makna** hingga **paling berbeza**, seperti yang embeddings akan lakukan (skor cosine similarity tinggi → rendah).

**Ayat rujukan:** `"Bagaimana cara memperbaharui lesen memandu?"`

**Calon:**
```
A. "Prosedur pembaharuan lesen memandu"
B. "How do I renew my driving licence?"
C. "Cara membayar saman trafik tertunggak"
D. "Langkah menukar hak milik kenderaan"
E. "Renew driving license steps"
```

1. Susun A–E dari **paling hampir** maknanya kepada **paling jauh** berbanding ayat rujukan.
2. Perhatikan: **B & E dalam bahasa Inggeris** tetapi maknanya **hampir sama** dengan rujukan — embeddings sepatutnya letak mereka **tinggi**, walaupun perkataannya berlainan bahasa. Inilah **carian semantik**.
3. **C & D** tentang topik JPJ lain (saman, pindah milik) — makna **berbeza**, jadi skor **rendah** walaupun sama-sama domain JPJ.

> **Susunan jangkaan** (paling serupa → paling jauh): **A, B, E** (semua tentang pembaharuan lesen — merentas bahasa) → kemudian **D**, **C** (topik JPJ berbeza). Carian kata kunci biasa akan **terlepas** B & E kerana ejaan berbeza — carian semantik **tidak**.

4. **Perbincangan:** Kenapa keupayaan memadan **merentas bahasa & ayat berbeza** ini penting untuk kaunter JPJ? (Petunjuk: orang awam bertanya dengan seribu satu cara & bahasa berbeza.)

✅ **Semakan:** Anda boleh menerangkan **mengapa** ayat Inggeris B & E patut mendapat skor persamaan tinggi dengan soalan BM — iaitu embeddings memadankan **makna**, bukan perkataan/ejaan.

---

## Latihan 5 — Bina Workflow AI Pertama (Webhook → AI → Response)

**Objektif:** Bina *API endpoint* berkuasa AI pertama anda dalam n8n (padanan 💻 Lab SESI 5). Inilah puncak Hari 1.

> **Ingat:** *workflow* ini menjawab dari **pengetahuan umum model sahaja** — ia **belum RAG**, jadi **belum** untuk jawapan JPJ sebenar. Tujuannya membiasakan anda dengan **mekanik n8n**: cipta workflow, tambah node, set credentials, sambung, execute. RAG datang Hari 2.

### 5.1 — Cipta workflow baharu

1. Buka n8n (Cloud atau tempatan).
2. Klik **+ (New Workflow)** / **Add workflow**.
3. Namakan workflow: klik tajuk di atas → taip `Hari 1 - First AI Workflow` → simpan (**Save**).

> 📸 Skrin di bawah dirakam daripada **n8n tempatan sebenar** (`localhost:5678`). Selepas klik **Build a workflow**, anda tiba di **kanvas kosong** ("Add first step…"):

![Kanvas workflow kosong n8n — "Add first step"](../../nota/img/lab1-01-canvas.jpg)

### 5.2 — Tambah node pencetus (Trigger)

Kita mula dengan **Manual Trigger** dahulu (paling mudah untuk uji), kemudian tukar ke **Webhook**.

1. Pada kanvas kosong, klik **+** (atau "Add first step").
2. Pilih **Manual Trigger** (*"Trigger manually"*) — ini membolehkan anda tekan butang untuk menguji tanpa perlu URL luaran dahulu.

Apabila anda klik **+**, panel **"What triggers this workflow?"** muncul — pilih **Trigger manually**:

![Panel pemilih trigger n8n — Trigger manually, On app event, On a schedule, dll.](../../nota/img/lab1-02-trigger-picker.jpg)

Node **Manual Trigger** ("When clicking 'Execute workflow'") kini berada di kanvas:

![Node Manual Trigger pada kanvas n8n](../../nota/img/lab1-03-manual-trigger.jpg)

> **Kenapa Manual Trigger dulu?** Ia memudahkan ujian semasa membina. Selepas workflow berjalan, kita tukar kepada **Webhook** supaya boleh dipanggil dari luar (langkah 5.7).

### 5.3 — Tambah node AI / OpenAI

1. Klik **+** pada output *Manual Trigger*.
2. Cari & pilih node **OpenAI** (atau **AI**/**Basic LLM Chain** bergantung versi n8n anda).

Taip `OpenAI` dalam carian node — anda nampak **OpenAI** & **OpenAI Chat Model**:

![Cari & tambah node OpenAI dalam n8n](../../nota/img/lab1-04-add-openai-node.jpg)

3. Pilih operasi **Message a model** / **Chat** (menghantar mesej ke model & terima jawapan).

### 5.4 — Set credentials (kunci API)

1. Dalam node OpenAI, pada medan **Credential**, klik **Create New Credential**.
2. Tampal **kunci API OpenAI** anda → **Save**.
3. n8n menyimpan kunci ini **disulitkan & berasingan** — anda tak perlu taip semula; ia boleh **guna semula** dalam node lain kemudian.

> **Guna Ollama sebaliknya?** Pilih model Ollama tempatan dan tetapkan *base URL* Ollama (cth. `http://localhost:11434`) — jawapan tak keluar dari mesin anda. Sesuai untuk data sensitif JPJ.

> **Keselamatan:** **Jangan** taip kunci API terus dalam mana-mana medan teks/prompt atau kongsi dalam mesej — sentiasa guna sistem **Credential**. Rujuk [`../nota/08-governance-keselamatan.md`](../../nota/08-governance-keselamatan.md).

### 5.5 — Tetapkan model & prompt

1. **Model:** pilih model chat (cth. `gpt-4o-mini` — murah & pantas untuk belajar).
2. **Prompt / Message:** masukkan satu prompt bergaya-JPJ, cth:
   ```
   Anda pembantu maklumat mesra untuk kaunter JPJ.
   Jawab soalan pengguna dalam Bahasa Melayu yang mudah, ringkas (2-4 ayat).
   Jika anda tidak pasti fakta rasmi (kadar, nombor borang), nasihatkan pengguna
   sahkan dengan kaunter JPJ — jangan reka nombor.

   Soalan: Apakah maksud pindah milik kenderaan?
   ```

### 5.6 — Sambung node & execute

1. Pastikan wayar (*connection*) menyambung **Manual Trigger → OpenAI** (tarik dari bulatan output trigger ke node OpenAI jika belum tersambung).
2. Klik **Execute Workflow** (atau **Test workflow**).
3. Klik node **OpenAI** → tab **Output**. Anda patut lihat **jawapan model** dalam bentuk JSON — cari medan seperti `message.content` / `text` / `response`.

**Apa yang anda patut nampak:** satu jawapan teks daripada AI menerangkan maksud "pindah milik kenderaan" dalam BM ringkas. Jika ada **ralat merah** — baca mesejnya (selalunya *credentials* salah, kunci API tiada kredit, atau tiada internet).

### 5.7 — Naik taraf ke Webhook + Response (jadikan ia "endpoint")

Sekarang jadikan ia *API endpoint* sebenar yang boleh dipanggil dari luar:

1. **Padam** *Manual Trigger*, tambah node **Webhook** sebagai pencetus. Set **HTTP Method** = `POST`, dan salin **URL webhook** (mod *Test*).
2. Sambung **Webhook → OpenAI**.
3. Ubah prompt supaya soalan datang dari data webhook — guna ungkapan n8n untuk ambil medan yang dihantar, cth: `{{ $json.body.question }}` sebagai soalan (bukannya soalan tetap).
4. Tambah node **Respond to Webhook** selepas OpenAI → sambung **OpenAI → Respond to Webhook**. Ini memulangkan jawapan AI sebagai respons HTTP.
5. Bentuk akhir:
   ```
   Webhook  ───►  OpenAI  ───►  Respond to Webhook
   (POST: {question})   (jana jawapan)    (pulangkan jawapan)
   ```
6. Klik **Execute/Listen**, kemudian hantar soalan ke URL webhook (guna butang "Test" n8n, atau alat seperti Postman/`curl`) dengan badan JSON:
   ```json
   { "question": "Berapa lama tempoh sah lesen memandu penuh?" }
   ```
7. Sahkan anda menerima **jawapan AI** sebagai respons.

> **Templat rujukan:** Bandingkan workflow anda dengan templat penuh yang boleh diimport: [`../templates/workflows/01-first-ai-workflow.json`](../../templates/workflows/01-first-ai-workflow.json). Import melalui menu n8n (**⋮ → Import from File**) untuk melihat versi lengkap.

✅ **Semakan:** Workflow anda **execute tanpa ralat merah**, node OpenAI memaparkan jawapan AI dalam Output, dan (bahagian Webhook) menghantar soalan JSON ke URL memulangkan jawapan sebagai respons HTTP. Anda telah membina *API endpoint* berkuasa AI pertama anda. 🎉

---

## Cabaran

Pilih **sekurang-kurangnya satu** untuk cuba selepas Latihan 5 siap:

1. **System prompt anti-halusinasi** — Ubah prompt node OpenAI supaya ia **sentiasa** menolak mereka nombor rasmi: tambah arahan tegas *"Jika soalan meminta kadar bayaran, nombor borang, atau tarikh khusus yang tiada dalam arahan ini, jawab: 'Sila sahkan dengan kaunter JPJ' dan JANGAN nyatakan angka."* Uji dengan soalan yang meminta kadar kompaun — sahkan ia **enggan mereka** nombor.
2. **Balasan berstruktur JSON** — Minta model memulangkan jawapan sebagai JSON dengan medan `{ "jawapan": "...", "keyakinan": "tinggi/rendah", "perlu_sahkan_kaunter": true/false }`. (Petunjuk: nyatakan format JSON yang tepat dalam prompt.) Ini corak berguna untuk Hari 3 (ejen).
3. **Dua bahasa** — Tambah arahan supaya pembantu **mengesan bahasa soalan** dan menjawab dalam bahasa yang sama (BM soalan → BM jawapan; English question → English answer). Uji dengan kedua-dua bahasa.
4. **Bandingkan model** — Jalankan workflow yang sama dengan **dua** model berbeza (cth. `gpt-4o-mini` vs model Ollama tempatan seperti `llama3`). Banding kelajuan, gaya jawapan, dan (jika Ollama) fakta bahawa data **tidak keluar** dari mesin anda. Catat pemerhatian.
5. **Pra-RAG manual dalam n8n** — Tambah satu node **Set/Edit Fields** **sebelum** OpenAI yang menyimpan satu petikan SOP contoh (data latihan) sebagai medan `context`. Ubah prompt supaya menyuntik `{{ $json.context }}` sebagai konteks dan mengarah model jawab **daripada konteks itu sahaja**. Ini **pratonton RAG** — Hari 2 kita gantikan node Set ini dengan **carian vektor sebenar** dalam Qdrant.

> Tiada jawapan "betul" tunggal untuk Cabaran — matlamatnya berlatih menggabungkan konsep. Tunjuk hasil kepada fasilitator sebelum tamat kelas.

---

## Rujukan & Langkah Seterusnya

| Latihan | Rujukan konsep (README) | Rujukan luar |
|---------|-------------------------|--------------|
| Token (Latihan 2) | [SESI 2 — Token & context window](../README.md#sesi-2-1030--100--memahami-large-language-models-llm) | [platform.openai.com/tokenizer](https://platform.openai.com/tokenizer) |
| Prompt engineering (Latihan 2) | [SESI 2 — Asas prompt engineering](../README.md) | [platform.openai.com/docs/guides/prompt-engineering](https://platform.openai.com/docs/guides/prompt-engineering) |
| Carian semantik (Latihan 4) | [SESI 4 — Embeddings](../README.md) | [platform.openai.com/docs/guides/embeddings](https://platform.openai.com/docs/guides/embeddings) · [qdrant.tech](https://qdrant.tech/documentation/) |
| Workflow AI (Latihan 5) | [SESI 5 — Pengenalan n8n](../README.md) | [docs.n8n.io/advanced-ai](https://docs.n8n.io/advanced-ai/) |
| Templat workflow pertama | — | [`../templates/workflows/01-first-ai-workflow.json`](../../templates/workflows/01-first-ai-workflow.json) |

> **Esok (Hari 2):** kita gantikan "pengetahuan umum model" dengan **dokumen JPJ sebenar** — pasang Qdrant, proses & *chunk* dokumen, bina *ingestion* + *retrieval workflow* → **Pembantu Pengetahuan JPJ**. Pastikan credentials OpenAI/Ollama anda masih tersimpan dalam n8n sebelum tamat kelas hari ini.
