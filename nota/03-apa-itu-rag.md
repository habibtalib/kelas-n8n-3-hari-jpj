# Nota Konsep: Apa itu RAG?

> Nota latar belakang untuk SESI 3 (Hari 1). RAG ialah **jantung** kursus ini. Fahami idea besarnya di sini; kita bina *pipeline* penuhnya pada Hari 2.

---

## Kenapa LLM biasa tidak cukup

LLM (rujuk [`02-apa-itu-llm.md`](./02-apa-itu-llm.md)) hebat berbahasa, tetapi untuk kegunaan JPJ ia ada **tiga masalah maut**:

| Masalah | Maksud | Contoh JPJ |
|---------|--------|-----------|
| **Tiada akses dokumen JPJ** | LLM tak pernah baca SOP, pekeliling & akta dalaman JPJ. | Tanya "prosedur pindah milik terkini?" → ia meneka. |
| **Pengetahuan lapuk (*stale*)** | Data latihan ada tarikh potong; pekeliling baru tak diketahui. | Kadar kompaun berubah 2026 → LLM masih guna angka lama. |
| **Halusinasi** | Ia mereka jawapan yakin tapi salah. | Ia cipta "Pekeliling JPJ 12/2025" yang **tidak wujud**. |

> Tanya LLM biasa soalan JPJ = seperti bertanya kepada orang pandai yang **tidak pernah membaca dokumen anda** dan **enggan mengaku tidak tahu**. Berbahaya untuk perkhidmatan kerajaan.

---

## Idea RAG: *Retrieve* dahulu, *Generate* kemudian

**RAG** = *Retrieval-Augmented Generation* (Penjanaan Diperkukuh-Pengambilan). Ideanya mudah:

> **Sebelum** LLM menjawab, kita **cari** (*retrieve*) potongan dokumen JPJ yang relevan, **suapkan** kepada LLM sebagai konteks, dan arahkan ia menjawab **hanya** berdasarkan konteks itu (**generate**) — dengan menyebut sumbernya.

Ia seperti memberi LLM **"buku terbuka"**: bukannya menjawab dari ingatan, ia menjawab sambil merujuk halaman dokumen sebenar yang kita hulurkan.

```
Soalan  →  [Cari dokumen relevan]  →  [Suap ke LLM + soalan]  →  Jawapan + petikan
           (retrieve)                  (augment + generate)
```

---

## Pipeline RAG penuh

RAG ada **dua fasa**. Fasa pertama (Ingestion) dilakukan **sekali** untuk memasukkan dokumen. Fasa kedua (Retrieval) berlaku **setiap kali** pengguna bertanya.

```
┌──────────────────────── FASA 1: INGESTION (sekali / bila dokumen berubah) ────────────────────────┐
│                                                                                                     │
│   Dokumen JPJ        Pecah kepada        Tukar setiap chunk         Simpan vektor + teks            │
│   (PDF/SOP/akta)  →  potongan (chunks) →  jadi embedding (vektor) →  ke Vector DB (Qdrant)          │
│   [Extract Text]     [Chunk]              [Embed]                    [Store]                         │
│                                                                                                     │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────── FASA 2: RETRIEVAL (setiap soalan pengguna) ───────────────────────────────┐
│                                                                                                     │
│  Soalan       Tukar soalan      Cari chunk       Ambil teks          LLM jana jawapan               │
│  pengguna  →  jadi embedding →  paling serupa →  chunk teratas   →   dari konteks +   →  Jawapan    │
│                                 dalam Qdrant     sebagai konteks     petikan sumber      + sumber   │
│  [Question]   [Embed]           [Vector Search]  [Context]           [LLM]                           │
│                                                                                                     │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘
```

- **Fasa 1 (Ingestion)** = Hari 2, SESI 9 → `templates/workflows/02-ingestion-workflow.json`
- **Fasa 2 (Retrieval)** = Hari 2, SESI 10 → `templates/workflows/03-retrieval-workflow.json`

> *Embedding* & *Vector Search* diterangkan penuh dalam [`04-embeddings-vector-db.md`](./04-embeddings-vector-db.md). Untuk sekarang, fikirkan: **embedding = "cap jari makna"** teks; **vector search = cari teks bermakna serupa**.

---

## Manfaat RAG

| Manfaat | Kenapa penting untuk JPJ |
|---------|--------------------------|
| **Ketepatan** | Jawapan diikat pada dokumen sebenar, bukan tekaan → kurang salah. |
| **Petikan sumber** | Setiap jawapan boleh sebut "menurut SOP X, seksyen Y" → boleh disemak & dipercayai. |
| **Terkini (*freshness*)** | Kemas kini dokumen dalam Vector DB → jawapan terus terkini. **Tiada latihan semula LLM.** |
| **Kawalan data** | Dokumen kekal dalam Qdrant self-hosted JPJ → residensi data terjaga ([`08`](./08-governance-keselamatan.md)). |
| **Kurang halusinasi** | Arahkan LLM jawab hanya dari konteks & kata *"tidak dijumpai"* jika tiada → guardrail. |
| **Jimat kos** | Hantar hanya chunk relevan (bukan seluruh akta) → kurang token, lebih murah. |

---

## Contoh: dengan vs tanpa RAG

**Soalan:** *"Berapa tempoh sah lesen memandu kelas D untuk pembaharuan 5 tahun?"*

| | LLM biasa | LLM + RAG |
|--|-----------|-----------|
| Sumber jawapan | Ingatan latihan (mungkin lapuk/salah) | Dokumen JPJ sebenar dalam Qdrant |
| Risiko halusinasi | Tinggi | Rendah |
| Petikan | Tiada | "Menurut *Prosedur Pembaharuan Lesen*, ms 3" |
| Jika tiada maklumat | Tetap mereka jawapan | Balas *"tidak dijumpai dalam dokumen"* |

---

## RAG vs Fine-tuning — bila guna yang mana?

Dua cara memberi LLM "pengetahuan tambahan": **RAG** (beri dokumen semasa bertanya) atau **fine-tuning** (latih semula model dengan data anda).

| | **RAG** | **Fine-tuning** |
|--|---------|-----------------|
| Cara | Cari & suap dokumen ketika bertanya | Ubah "otak" model dengan latihan tambahan |
| Kemas kini fakta | **Mudah** — tukar dokumen dalam DB | **Susah** — perlu latih semula |
| Petikan sumber | ✅ Ya | ❌ Tidak |
| Kos & kepakaran | Rendah, tanpa GPU besar | Tinggi, perlu GPU & data berlabel |
| Sesuai untuk | **Pengetahuan yang kerap berubah** (dokumen JPJ) | Mengajar **gaya/format** atau tugas khusus |

> **Untuk JPJ, RAG ialah pilihan utama** — dokumen kerap dikemas kini, petikan sumber wajib, dan kita mahu elak melatih semula model. Fine-tuning hanya dipertimbang untuk hal gaya/nada, bukan fakta.

---

Seterusnya: [`04-embeddings-vector-db.md`](./04-embeddings-vector-db.md) — bagaimana "cari dokumen serupa" benar-benar berfungsi. Kemudian kita bina *pipeline* ini dalam n8n pada [Hari 2](../hari-2/README.md).
