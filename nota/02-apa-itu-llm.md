# Nota Konsep: Apa itu LLM?

> Nota latar belakang untuk SESI 2 (Hari 1). LLM ialah "otak" pembantu kita. Fahami cara ia berfikir — dan **kenapa ia kadang tersilap** — sebelum kita ikatnya pada dokumen JPJ dengan RAG.

---

## Apa itu LLM?

**LLM** = *Large Language Model* (Model Bahasa Besar). Ia program AI yang dilatih pada **jumlah teks yang sangat besar** (buku, laman web, kod) sehingga ia belajar corak bahasa manusia. Hasilnya: ia boleh **menjawab soalan, meringkas, menterjemah, dan menulis** dalam bahasa semula jadi.

Contoh LLM: **GPT** (OpenAI), **Claude** (Anthropic), **Gemini** (Google), dan model **sumber terbuka** seperti **Llama** & **Mistral** yang boleh dijalankan tempatan melalui **Ollama**.

> **Penting untuk difahami:** LLM **tidak "tahu" fakta** seperti pangkalan data. Ia meramal teks yang **paling berkemungkinan** berdasarkan latihannya. Inilah punca ia sangat berguna — tetapi juga punca ia boleh **berhalusinasi** (rujuk di bawah). RAG (nota seterusnya) menyelesaikan masalah ini.

---

## Bagaimana ia berfungsi: ramalan token seterusnya

Secara intuitif, LLM ialah **mesin melengkapkan ayat yang sangat pandai**. Ia melihat teks setakat ini, dan meramal **perkataan (token) seterusnya** yang paling berkemungkinan — berulang kali.

```
Input:  "Ibu negara Malaysia ialah ___"
LLM:    Kuala (95%) · Putra (3%) · Johor (1%) · ...
        → pilih "Kuala"

Input:  "Ibu negara Malaysia ialah Kuala ___"
LLM:    Lumpur (99%) · ...
        → pilih "Lumpur"
```

Ia meramal **satu token pada satu masa**, menyambung token yang dipilih ke input, dan meramal token seterusnya — sehingga jawapan lengkap. Tiada "carian fakta" berlaku; ia hanya corak statistik yang **sangat** baik.

---

## Token — unit asas LLM

LLM tidak melihat "perkataan"; ia melihat **token** — cebisan teks. Satu token lebih kurang **¾ perkataan** dalam bahasa Inggeris (BM lebih kurang serupa). Kos & had LLM dikira dalam **token**, bukan perkataan.

| Teks | Anggaran token |
|------|----------------|
| `JPJ` | 1–2 token |
| `Lesen Memandu` | 3–4 token |
| `Bagaimana cara membaharui lesen memandu?` | ~9–11 token |
| `renewal` | 1 token |
| `pembaharuan` | 3–4 token (perkataan panjang dipecah) |
| 1 muka surat teks (~500 patah) | ~650–750 token |

> **Petua:** ~**1,000 token ≈ 750 patah perkataan** (Inggeris). Perkataan jarang/panjang & bukan-Inggeris dipecah kepada lebih banyak token. Anda boleh uji di [platform.openai.com/tokenizer](https://platform.openai.com/tokenizer).

**Kenapa ini penting:** setiap panggilan LLM dicaj mengikut *(token input + token output)*. Prompt yang panjang = lebih mahal & lebih perlahan. Dalam RAG, kita hantar **hanya potongan dokumen yang relevan** (bukan keseluruhan akta) untuk jimat token.

---

## Context window — "ingatan jangka pendek"

**Context window** ialah **jumlah maksimum token** (input + output) yang LLM boleh proses dalam **satu** panggilan. Ia seperti meja kerja: hanya muat sekian banyak kertas pada satu masa.

- Model moden: dari ~8,000 token sehingga **ratusan ribu** token (setara ratusan muka surat).
- Jika anda hantar teks melebihi context window → ia **terpotong** atau ditolak.
- LLM **tidak ingat** perbualan lepas melainkan anda hantar semula sejarah itu dalam context window.

> **Kaitan RAG:** context window terhad, jadi kita **tidak boleh** simpan seluruh koleksi dokumen JPJ di dalamnya. Sebaliknya kita **cari** potongan relevan dan hanya suapkan itu — inilah teras RAG ([`03-apa-itu-rag.md`](./03-apa-itu-rag.md)).

---

## GPT vs Claude vs Gemini vs Sumber Terbuka (Ollama)

| Model | Penyedia | Jenis | Nota untuk JPJ |
|-------|----------|-------|----------------|
| **GPT** (cth. GPT-4o) | OpenAI | API cloud | Kuat & popular; data melalui API OpenAI. Digunakan sebagai LLM utama kursus. |
| **Claude** | Anthropic | API cloud | Kuat untuk penaakulan & dokumen panjang. |
| **Gemini** | Google | API cloud | Bersepadu dengan ekosistem Google. |
| **Llama / Mistral** | Meta / Mistral (sumber terbuka) | **Jalankan tempatan** via **Ollama** | **Data tidak keluar** dari server JPJ — pilihan disyorkan untuk dokumen sensitif. |

> **Kenapa Ollama penting untuk JPJ:** dengan **Ollama**, model seperti **Llama** atau **Mistral** berjalan **di dalam server JPJ sendiri** — tiada teks dihantar ke API pihak ketiga. Ini kunci **residensi data** & pematuhan PDPA. Dibincang penuh di [`08-governance-keselamatan.md`](./08-governance-keselamatan.md). Kursus guna OpenAI untuk belajar, dan tunjuk Ollama sebagai posture pengeluaran.

---

## Temperature — kawalan "kreativiti"

**Temperature** ialah tetapan (biasanya **0 hingga 1**, kadang sehingga 2) yang mengawal sejauh mana LLM **rawak** dalam memilih token seterusnya.

| Temperature | Kesan | Sesuai untuk |
|-------------|-------|--------------|
| **0 (rendah)** | Deterministik, fokus, jawapan konsisten & boleh diramal | **RAG / jawapan fakta JPJ** — kita mahu jawapan tepat & seragam |
| **~0.7 (sederhana)** | Seimbang, sedikit variasi | Perbualan biasa |
| **1+ (tinggi)** | Kreatif, pelbagai, kadang melantur | Sumbang saran, penulisan kreatif |

> **Untuk Pembantu Pintar JPJ, guna temperature rendah (0–0.2)** — kita mahu jawapan yang **tepat, konsisten & berpaksikan dokumen**, bukan kreatif.

---

## Kenapa halusinasi berlaku

**Halusinasi** = LLM menjana jawapan yang **kedengaran yakin tetapi salah / direka**. Contoh: mencipta nombor pekeliling JPJ yang tidak wujud.

Puncanya berbalik kepada cara LLM bekerja:
- Ia **meramal teks yang berkemungkinan**, bukan menyemak fakta.
- Jika ia **tidak tahu**, ia tetap meramal sesuatu yang "kelihatan betul" — kerana itulah corak yang dilatih.
- Pengetahuannya **statik** (terhad pada data latihan) dan **tiada akses** kepada dokumen dalaman JPJ.

> **Inilah masalah utama yang RAG selesaikan:** dengan **memberi** LLM potongan dokumen sebenar & mengarahkannya menjawab **hanya** dari situ (dengan petikan), kita kurangkan halusinasi secara mendadak. Lihat [`03-apa-itu-rag.md`](./03-apa-itu-rag.md) & [`07-prompt-engineering.md`](./07-prompt-engineering.md).

---

## Batasan LLM (ringkas)

- ❌ **Tiada pengetahuan terkini** — data latihan ada tarikh potong; ia tak tahu pekeliling JPJ terbaru.
- ❌ **Tiada akses dokumen dalaman** — ia tak pernah baca SOP JPJ anda.
- ❌ **Boleh berhalusinasi** — mereka fakta dengan yakin.
- ❌ **Tiada ingatan** antara panggilan (melainkan diberi konteks semula).
- ❌ **Berat sebelah / had penaakulan** — bergantung pada data latihan.

RAG + prompt yang baik + LLM tempatan menangani kebanyakan batasan ini — itulah keseluruhan kursus ini.

---

Seterusnya: [`03-apa-itu-rag.md`](./03-apa-itu-rag.md) — cara mengikat LLM pada dokumen sebenar JPJ.

## Sumber Rasmi

- **[platform.openai.com/tokenizer](https://platform.openai.com/tokenizer)** — lihat teks dipecah kepada token.
- **[ollama.com](https://ollama.com)** — jalankan Llama/Mistral secara tempatan.
- **[docs.n8n.io](https://docs.n8n.io/advanced-ai/)** — nod AI n8n yang menyambung ke LLM ini.
