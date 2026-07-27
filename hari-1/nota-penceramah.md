# Nota Penceramah — Hari 1: Asas AI, LLM & RAG + n8n Pertama (SESI 1–5)

> Nota rujukan untuk **penceramah/jurulatih**. Setiap bahagian sepadan dengan satu sesi rasmi dalam [`../JADUAL.md`](../JADUAL.md). Disusun mengikut **timing sebenar** supaya mudah pandu rentak sepanjang hari. Bahan peserta: [`README.md`](./README.md) (konsep) & [`snippets/lab.md`](./snippets/lab.md) (hands-on).

**Rentak keseluruhan Hari 1:** Hari ini **konsep-berat, bina-ringan** — puncaknya satu workflow AI ringkas di SESI 5. Jangan tergesa-gesa masuk n8n; pelaburan kefahaman AI/LLM/RAG hari ini menjadikan Hari 2 (RAG sebenar) jauh lebih lancar. Ulang mantra kursus sepanjang hari: **"AI membantu, anda memandu."**

---

## SESI 1 · Pengenalan AI (9.00 – 10.30, 90 min)

**Pecahan masa:** Evolusi AI & ML vs GenAI (~30 min) · Landskap & AI sektor awam (~20 min) · Peluang dalam JPJ (~15 min) · 🧠 Bengkel kes guna (~25 min).

**Poin utama:**
- Bina "peta mental": *rule-based* → *ML* → *deep learning* → *generative AI*. Peserta hanya perlu ingat **GenAI = hasilkan kandungan baharu**, ML klasik = ramal/klasifikasi.
- Tekankan awal: LLM **meramal teks**, tidak "tahu fakta" — benih untuk perbincangan halusinasi di SESI 2.

**Analogi yang berkesan:**
- *Rule-based* = borang semakan tegar (buntu bila luar kotak); *ML* = pegawai berpengalaman kenal corak; *GenAI* = pegawai yang boleh **tulis semula** penjelasan dalam ayat mudah.

**Hook JPJ:** Buka dengan soalan sebenar kaunter: *"Berapa kali sehari soalan sama tentang pembaharuan lesen ditanya?"* — tunjukkan beban berulang inilah sasaran AI. Kaitkan setiap gelombang AI dengan JPJ.

**Soalan lazim peserta:**
- *"AI akan ganti pegawai kaunter?"* → Tidak — ia **mengangkat** pegawai: kerja berulang diautomasi, pegawai fokus kes rumit & sentuhan manusia. Mantra: AI membantu, anda memandu.
- *"Adakah ini seperti ChatGPT?"* → Ya, ChatGPT ialah satu contoh LLM. Kita akan bina versi yang **berpaksikan dokumen JPJ**, bukan pengetahuan umum.

**Bengkel (Latihan 1):** Jaga masa — 25 min sahaja. Matlamat: setiap kumpulan keluar dengan 1–2 kes "kerap + berpaksikan dokumen". Ini benih projek 3 hari mereka.

---

## SESI 2 · Memahami LLM (10.30 – 1.00, 150 min)

**Pecahan masa:** Apa itu LLM + penyedia (~30 min) · Token & context window (~30 min) · Prompt engineering (~35 min) · Halusinasi & batasan (~20 min) · 💻 Latihan prompt (~35 min).

**Poin utama:**
- **Token:** guna [platform.openai.com/tokenizer](https://platform.openai.com/tokenizer) secara **langsung** — tampal ayat BM gaya-JPJ, tunjuk pecahan. Peserta ingat lebih baik bila "nampak" token.
- **Context window = saiz meja kerja** — apa tak muat, model tak nampak. Ini **sebab teknikal RAG wujud** — tanam benih ini kuat-kuat untuk SESI 3.
- **Prompt engineering:** 5 prinsip (jelas, konteks, format, contoh, sempadan). Tegaskan prinsip **"beri konteks"** & **"tetapkan sempadan"** ialah teras RAG.
- **Halusinasi:** demokan **secara langsung** — tanya kadar kompaun tanpa dokumen, tunjuk nombor direka yang "yakin".

**Analogi yang berkesan:**
- LLM = pegawai yang baca berjuta muka surat, sangat mahir "lengkapkan ayat" — tapi dari **ingatan**, bukan pangkalan data.
- Token ≈ 4 aksara / ¾ perkataan — cukup untuk intuisi kos & had.

**Hook JPJ:** Halusinasi kadar kompaun ialah contoh **paling menggerunkan** untuk audiens JPJ — jawapan salah tentang bayaran/prosedur boleh sesatkan orang awam. Inilah "kenapa" emosi untuk RAG.

**Soalan lazim peserta:**
- *"Kenapa AI beri jawapan berbeza untuk soalan sama?"* → LLM meramal secara berkebarangkalian; ada unsur rawak. RAG + prompt ketat mengurangkan variasi.
- *"Model mana terbaik — GPT, Claude, Gemini?"* → Semua mampu; pilihan bergantung kos, privasi, integrasi. Untuk JPJ, **Ollama on-prem** penting bila data sensitif.
- *"Bila guna Ollama vs OpenAI?"* → OpenAI untuk belajar/mudah; Ollama bila data tak boleh keluar. Kita dalami Hari 3.

**Latihan (Latihan 2):** Pastikan setiap peserta **saksikan halusinasi sendiri** (2.3) dan kesan "buku terbuka" (2.4). Bahagian 2.4 ialah **jambatan** langsung ke RAG — highlight-kan.

---

## SESI 3 · Memahami RAG (2.30 – 3.45, 75 min)

**Pecahan masa:** Kenapa LLM biasa tak cukup + fine-tune vs RAG (~20 min) · Apa itu RAG + pipeline ASCII (~30 min) · Manfaat RAG (~10 min) · 👥 Aktiviti reka pembantu (~15 min).

**Poin utama:**
- Sambung terus dari halusinasi SESI 2: RAG = penyelesaian. **Jangan** perkenalkan RAG sebagai konsep baharu terpencil — ia **jawapan** kepada masalah yang mereka baru saksikan.
- Lukis **pipeline RAG** (User Question → Embedding → Vector Search → Relevant Context → LLM → Answer) di papan putih secara langsung — jangan hanya tunjuk slaid. Peserta akan bina bentuk **sama** ini di Hari 2.
- Fine-tune vs RAG: RAG menang untuk JPJ (murah, pantas, ikut dokumen terkini, boleh petik sumber).

**Analogi yang berkesan (paling penting hari ini):**
- **Peperiksaan buku terbuka.** LLM biasa = jawab dari ingatan (mungkin salah). RAG = pelajar sama dibenarkan **rujuk buku teks rasmi** dahulu. Analogi ini "melekat" untuk hampir semua peserta — labur masa padanya.

**Hook JPJ:** Bayangkan pegawai baharu di kaunter dengan **akses segera** ke semua SOP + petikan perenggan tepat. Itulah janji RAG untuk JPJ: jawapan **konsisten antara cawangan**, **bercitasi**, boleh disahkan.

**Soalan lazim peserta:**
- *"RAG hilangkan halusinasi 100%?"* → Tidak sepenuhnya, tapi **kurangkan drastik** kerana jawapan diikat pada teks sumber + boleh disemak. Sebab itu kita sentiasa **petik sumber**.
- *"Kena latih semula model bila dokumen berubah?"* → **Tidak** — itu kelebihan RAG. Tukar dokumen dalam vector DB, pembantu terus tahu.
- *"Di mana dokumen disimpan?"* → Dalam vector DB (Qdrant), boleh on-prem — dokumen kekal dalam kawalan JPJ.

**Aktiviti (Latihan 3):** Peserta lakar pembantu untuk bahagian **sendiri**. Galakkan guna kes dari Bengkel SESI 1. Ini menjadi draf projek capstone mereka.

---

## SESI 4 · Embeddings & Vector DB (3.45 – 4.15, 30 min)

**Pecahan masa:** Embeddings + carian semantik (~12 min) · Cosine similarity (~8 min) · Vector DB (Qdrant dll) (~5 min) · 🔎 Demo (~5 min). **Sesi paling pendek — jaga masa ketat.**

**Poin utama:**
- Jawab **satu** soalan sahaja: "bagaimana komputer tahu dua ayat sama makna walau perkataan beza?" → **embeddings** (teks → vektor makna).
- **Carian semantik vs kata kunci:** contoh terkuat — "renew licence" (English) padan "pembaharuan lesen" (BM). Merentas bahasa = kuasa embeddings.
- **Cosine similarity = sudut antara anak panah.** Arah sama (sudut kecil) → makna serupa (skor ~1.0). Jangan masuk matematik dalam — intuisi arah sudah cukup untuk pemula.
- Qdrant = pilihan rasmi (on-prem, Docker). Yang lain (Pinecone/Weaviate/Chroma/pgvector) sebut sebagai perbandingan sahaja. **Jangan** tenggelamkan mereka dengan pilihan.

**Analogi yang berkesan:**
- Embedding = "koordinat makna" dalam ruang. Ayat serupa duduk **berdekatan**; ayat berbeza duduk **berjauhan**.
- Cosine = anak panah dari pusat; kita ukur **arah**, bukan panjang.

**Hook JPJ:** Orang awam tanya "seribu satu cara" — BM, English, slanga, singkatan. Carian kata kunci terlepas; carian semantik **faham maksud**. Kritikal untuk kaunter pelbagai bahasa.

**Soalan lazim peserta:**
- *"1536 nombor untuk satu ayat — kenapa banyak?"* → Setiap dimensi tangkap aspek makna berbeza; lebih dimensi = lebih halus bezakan makna. Peserta tak perlu faham setiap dimensi.
- *"Perlu faham matematik cosine?"* → Tidak — n8n & Qdrant kira automatik. Cukup faham "skor tinggi = lebih serupa".

**Demo (Latihan 4):** Latihan kertas susun ayat A–E. Highlight B & E (English) patut skor tinggi. Cepat & padat — 5 min.

---

## SESI 5 · Pengenalan n8n + Workflow AI Pertama (4.15 – 5.00, 45 min)

**Pecahan masa:** Konsep n8n (nodes/connections/credentials/AI nodes) (~12 min) · 💻 Lab bina workflow (~30 min) · penutup & simpan kerja (~3 min).

**Poin utama:**
- n8n = **carta alir yang hidup**. Nodes = kotak tugas; connections = wayar; trigger = kotak pertama.
- **Credentials:** tekankan keselamatan — kunci API **jangan** ditaip dalam prompt/dikongsi; guna sistem Credential (disulitkan, guna semala). Relevan tinggi untuk JPJ.
- **Ingatkan berulang:** workflow pertama ini **belum RAG** — jawab dari pengetahuan umum sahaja, **belum** untuk jawapan JPJ sebenar. Tujuannya **mekanik n8n** sahaja. Hari 2 selitkan langkah RAG antara Webhook & AI.

**Analogi yang berkesan:**
- n8n = lukis carta alir yang benar-benar berjalan; data (JSON) mengalir ikut anak panah dari kotak ke kotak.

**Hook JPJ:** "Petang ini anda bina *API endpoint* AI pertama anda — rangka **sama** yang esok jadi Pembantu Pengetahuan JPJ sebenar." Beri rasa pencapaian: dari sifar ke workflow berjalan dalam satu hari.

**Soalan lazim peserta:**
- *"Perlu tahu coding untuk n8n?"* → Tidak untuk asas — semuanya visual. Sikit ungkapan (`{{ $json... }}`) diterang bila perlu.
- *"Manual Trigger vs Webhook — beza?"* → Manual = tekan butang untuk uji (mudah semasa bina); Webhook = URL yang boleh dipanggil dari luar (endpoint sebenar). Kita mula Manual, naik taraf ke Webhook.
- *"Ralat merah masa execute — kenapa?"* → Biasanya credentials salah, kunci API tiada kredit, atau tiada internet. Baca **baris pertama** mesej ralat.

**Lab (Latihan 5):** Beri masa cukup untuk **semua** peserta dapat node OpenAI memaparkan output (5.1–5.6) sebelum bahagian Webhook (5.7). Jika masa suntuk, 5.7 boleh jadi rumusan demo di depan / kerja rumah ringan. **Pastikan credentials tersimpan** sebelum bersurai — diperlukan Hari 2.

---

## Penutup Hari 1 (sebelum 5.00)

- Sahkan **setiap peserta** ada workflow yang berjaya execute (sekurang-kurangnya node OpenAI beri output).
- Sahkan **credentials OpenAI/Ollama tersimpan** dalam n8n — jimat masa Hari 2.
- Ulang tiga konsep bawa-pulang: (1) RAG ikat jawapan pada dokumen → anti-halusinasi, (2) embeddings + cosine = carian **makna**, (3) n8n bina semua secara visual.
- Teaser Hari 2: pasang Qdrant sebenar, proses dokumen JPJ, bina ingestion + retrieval → Pembantu Pengetahuan JPJ berfungsi.
