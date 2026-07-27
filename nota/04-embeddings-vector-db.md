# Nota Konsep: Embeddings & Vector DB

> Nota latar belakang untuk SESI 4 (Hari 1) & SESI 7 (Hari 2). Ini "enjin carian" di sebalik RAG. Sedikit matematik di sini — tetapi ringan & bergambar. Fahami intuisinya, bukan hafal formula.

---

## Apa itu embedding?

**Embedding** ialah cara menukar **teks → senarai nombor (vektor)** yang mewakili **makna** teks itu.

```
"pembaharuan lesen memandu"  →  [0.12, -0.98, 0.34, 0.05, ... , 0.77]
                                 └──────── vektor (embedding) ────────┘
```

Ideanya: teks yang **maknanya serupa** menghasilkan vektor yang **berdekatan** dalam ruang nombor; teks yang tak berkaitan menghasilkan vektor yang berjauhan. Model AI khas (*embedding model*) menghasilkan vektor ini — contohnya `text-embedding-3-small` (OpenAI) atau model embedding tempatan via Ollama.

> Fikirkan embedding sebagai **"koordinat makna"**. Sama seperti bandar ada koordinat (latitud, longitud), setiap ayat diberi koordinat dalam "peta makna" berdimensi tinggi.

---

## Kenapa ini bijak: carian semantik

Carian biasa (*keyword*) hanya jumpa **padanan perkataan tepat**. Carian **semantik** (guna embedding) jumpa berdasarkan **makna** — walaupun perkataan berbeza.

| Soalan pengguna | Dokumen JPJ | Carian *keyword* | Carian semantik |
|-----------------|-------------|------------------|-----------------|
| "cara renew lesen" | "prosedur **pembaharuan** **lesen memandu**" | ❌ tak padan ("renew" ≠ "pembaharuan") | ✅ padan (makna sama) |
| "kereta saya kena saman" | "notis **kompaun** kesalahan trafik" | ❌ | ✅ |

> Inilah kenapa RAG guna embedding: pengguna JPJ bertanya dengan bahasa harian mereka, tetapi dokumen ditulis dengan istilah rasmi. Carian semantik merapatkan jurang itu.

---

## Dimensi embedding

**Dimensi** = berapa banyak nombor dalam satu vektor. Lebih banyak dimensi = lebih halus makna ditangkap (tetapi lebih besar & perlahan).

| Model embedding | Dimensi |
|-----------------|---------|
| OpenAI `text-embedding-3-small` | **1536** |
| OpenAI `text-embedding-3-large` | 3072 |
| OpenAI `text-embedding-ada-002` (lama) | 1536 |
| Model tempatan (cth. `nomic-embed-text` via Ollama) | 768 |

> **Peraturan emas:** dimensi soalan **mesti sama** dengan dimensi dokumen tersimpan. Jika anda indeks dokumen dengan model 1536-dimensi, soalan **juga** mesti guna model 1536-dimensi yang **sama**. Menukar model embedding = perlu **indeks semula** semua dokumen. Tetapkan dimensi ini ketika cipta *collection* Qdrant.

---

## Cosine similarity — mengukur "keserupaan makna"

Bagaimana komputer tahu dua vektor "berdekatan"? Cara paling lazim ialah **cosine similarity** — ia mengukur **sudut** antara dua vektor, bukan jarak. Nilainya antara **-1 dan 1**:

- **1.0** = arah sama = makna sangat serupa ✅
- **0.0** = serenjang = tiada kaitan
- **-1.0** = bertentangan arah

Formula:

```
                A · B                (a₁·b₁ + a₂·b₂ + ... + aₙ·bₙ)
cos(θ) = ───────────────── = ─────────────────────────────────────────
          ‖A‖ × ‖B‖          √(a₁²+...+aₙ²) × √(b₁²+...+bₙ²)
```

### Contoh kecil (2 dimensi, supaya senang)

Katakan (dipermudah kepada 2 nombor):

- **A** = soalan `"renew lesen"` → `[2, 1]`
- **B** = dokumen `"pembaharuan lesen"` → `[3, 2]`  ← sepatutnya serupa
- **C** = dokumen `"pendaftaran kenderaan"` → `[-1, 2]`  ← kurang berkaitan

**A vs B:**
```
A · B   = (2×3) + (1×2) = 6 + 2 = 8
‖A‖     = √(2² + 1²) = √5  ≈ 2.236
‖B‖     = √(3² + 2²) = √13 ≈ 3.606
cos(θ)  = 8 / (2.236 × 3.606) = 8 / 8.062 ≈ 0.99   ← sangat serupa ✅
```

**A vs C:**
```
A · C   = (2×-1) + (1×2) = -2 + 2 = 0
cos(θ)  = 0 / (...) = 0.0                            ← tiada kaitan
```

> Jadi carian akan pilih **B** (skor 0.99) berbanding **C** (skor 0.0). Dalam realiti ini berlaku pada 1536 dimensi, tetapi ideanya **sama**: skor tertinggi = chunk paling relevan untuk dijadikan konteks LLM.

---

## Vector indexing & ANN — kenapa ia laju

Jika ada **berjuta** chunk dokumen, membandingkan soalan dengan **setiap** vektor satu demi satu (*brute force*) terlalu perlahan. Jadi Vector DB guna **ANN** (*Approximate Nearest Neighbor*) — algoritma indeks pintar (cth. **HNSW**) yang cari jiran terdekat **hampir tepat** dengan **sangat pantas**, tanpa periksa semua.

> Pertukaran (*trade-off*): sedikit ketepatan ditukar dengan kelajuan besar. Untuk RAG, ini hampir tak ketara — chunk teratas tetap sama.

---

## Apa yang Vector DB lakukan

**Vector database** ialah pangkalan data yang **direka khas** untuk menyimpan vektor & mencari yang paling serupa dengan pantas. Ia juga simpan:

- **Vektor** (embedding chunk)
- **Teks asal** chunk (untuk disuap ke LLM)
- **Metadata** — cth. `{jenis: "SOP", bahagian: "Pelesenan", sumber: "sop-lesen.pdf", muka_surat: 3}` untuk **penapisan** (*metadata filtering* — Hari 3).

Aliran: `simpan vektor+teks+metadata` (ingestion) → `cari top-k vektor serupa` (retrieval).

---

## Qdrant vs pilihan lain

Kursus ini guna **Qdrant** (sumber terbuka, *self-hosted* via Docker). Perbandingan ringkas:

| Vector DB | Sumber terbuka | Self-host | Nota |
|-----------|:--:|:--:|------|
| **Qdrant** ⭐ | ✅ | ✅ | **Pilihan kursus.** Laju (ditulis dalam Rust), dashboard mudah, sesuai on-prem JPJ. |
| Pinecone | ❌ | ❌ | Perkhidmatan *cloud* terurus sahaja — data keluar dari JPJ. |
| Weaviate | ✅ | ✅ | Berciri kaya, boleh self-host. |
| Chroma | ✅ | ✅ | Ringan, mesra untuk prototaip/tempatan. |
| pgvector | ✅ | ✅ | Sambungan **PostgreSQL** — sesuai jika sudah guna Postgres. |

> **Kenapa Qdrant untuk JPJ:** sumber terbuka + **self-hosted** bermakna vektor & dokumen **kekal dalam infrastruktur JPJ** (residensi data), pantas, dan ada dashboard visual di `http://localhost:6333/dashboard` untuk lihat *collections* & vektor. Pinecone dikecualikan kerana ia *cloud* sahaja. Persediaan Qdrant: [`05-setup-docker.md`](./05-setup-docker.md).

---

## Ringkasan cepat

| Konsep | Satu ayat |
|--------|-----------|
| Embedding | Teks → vektor nombor yang mewakili makna. |
| Dimensi | Bilangan nombor (cth. 1536); soalan & dokumen mesti sama. |
| Carian semantik | Cari ikut makna, bukan perkataan tepat. |
| Cosine similarity | Skor -1..1; makin tinggi makin serupa. |
| ANN / HNSW | Indeks pintar untuk carian pantas berjuta vektor. |
| Vector DB (Qdrant) | Simpan vektor+teks+metadata, cari top-k serupa. |

Seterusnya: [`05-setup-docker.md`](./05-setup-docker.md) — pasang Qdrant & n8n supaya kita boleh bina *pipeline* RAG sebenar.
