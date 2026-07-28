# Tutorial Hands-On: Qdrant DB 🟣

Tutorial **langsung** untuk memahami **Qdrant** (pangkalan data vektor) — tanpa n8n atau OpenAI. Anda taip arahan sendiri dan lihat hasil sebenar. Selepas ini, cara Qdrant diguna dalam RAG (Hari 2) akan jauh lebih jelas.

> **Kenapa toy vectors (4-dimensi)?** Di sini kita guna vektor kecil (4 nombor) yang ditulis tangan supaya anda **nampak** cara Qdrant bekerja. Dalam RAG sebenar (Hari 2), vektor datang dari model *embedding* (**1536** nombor) — konsepnya **sama**, cuma lebih besar.

**Prasyarat:** Qdrant berjalan (dari [`05-setup-docker.md`](./05-setup-docker.md)) — dashboard di `http://localhost:6333/dashboard`.

> **Dua cara jalankan arahan:** (A) **Terminal** dengan `curl` (di bawah), atau (B) **Qdrant dashboard → Console** (menu kiri) — tampal bahagian selepas `http://localhost:6333` sahaja. Kedua-dua sama.

---

## Konsep asas (4 istilah)

| Istilah | Maksud | Analogi |
|---------|--------|---------|
| **Collection** | Bekas untuk satu set vektor | Laci kabinet fail |
| **Point** | Satu rekod: `id` + `vector` + `payload` | Satu fail dalam laci |
| **Vector** | Senarai nombor (makna teks) | "Cap jari" makna |
| **Payload** | Metadata + teks disimpan bersama | Label & kandungan fail |

---

## Latihan 1 — Cipta Collection

```bash
curl -X PUT http://localhost:6333/collections/qdrant_tutorial \
  -H 'Content-Type: application/json' \
  -d '{"vectors": {"size": 4, "distance": "Cosine"}}'
```

**Hasil:** `{"result":true,"status":"ok", ...}`

- `size: 4` — setiap vektor mesti ada **4 nombor** (dalam RAG: 1536).
- `distance: Cosine` — cara Qdrant ukur "keserupaan" makna.

✅ **Semakan:** Buka dashboard → **Collections** → `qdrant_tutorial` wujud, status **GREEN**, 0 points.

---

## Latihan 2 — Masukkan Points (data JPJ)

Kita masukkan 3 rekod. Perhatikan vektor: point 1 & 3 **serupa** (topik lesen), point 2 **berbeza** (saman).

```bash
curl -X PUT http://localhost:6333/collections/qdrant_tutorial/points \
  -H 'Content-Type: application/json' \
  -d '{
    "points": [
      {"id": 1, "vector": [0.9, 0.1, 0.0, 0.1], "payload": {"topik": "lesen", "teks": "Pembaharuan lesen memandu"}},
      {"id": 2, "vector": [0.1, 0.9, 0.1, 0.0], "payload": {"topik": "saman", "teks": "Semak saman tertunggak"}},
      {"id": 3, "vector": [0.85, 0.15, 0.05, 0.1], "payload": {"topik": "lesen", "teks": "Kelayakan lesen GDL"}}
    ]
  }'
```

**Hasil:** `{"status":"ok"}`

✅ **Semakan:** Dashboard → `qdrant_tutorial` → **points = 3**. Klik untuk lihat setiap point + payloadnya.

---

## Latihan 3 — Carian Vektor (Semantic Search)

Cari 2 point paling **serupa** dengan vektor pertanyaan `[0.9, 0.1, 0.0, 0.1]` (arah "lesen"):

```bash
curl -X POST http://localhost:6333/collections/qdrant_tutorial/points/search \
  -H 'Content-Type: application/json' \
  -d '{"vector": [0.9, 0.1, 0.0, 0.1], "limit": 2, "with_payload": true}'
```

**Hasil sebenar:**
```
  1.0    Pembaharuan lesen memandu     (point 1)
  0.996  Kelayakan lesen GDL           (point 3)
```

Perhatikan: Qdrant kembalikan **kedua-dua point lesen** dengan skor tinggi (hampir 1.0), kerana vektor mereka **hampir** dengan pertanyaan. Point saman **tidak** muncul (skor rendah). **Inilah carian semantik** — padan ikut *makna* (arah vektor), bukan perkataan.

✅ **Semakan:** 2 hasil teratas ialah point **lesen** dengan skor ~1.0.

---

## Latihan 4 — Carian + Metadata Filter

Sekarang paksa Qdrant cari **hanya** dalam `topik = "saman"`, walaupun pertanyaan menghala ke "lesen":

```bash
curl -X POST http://localhost:6333/collections/qdrant_tutorial/points/search \
  -H 'Content-Type: application/json' \
  -d '{
    "vector": [0.9, 0.1, 0.0, 0.1],
    "limit": 2,
    "with_payload": true,
    "filter": {"must": [{"key": "topik", "match": {"value": "saman"}}]}
  }'
```

**Hasil sebenar:**
```
  0.217  Semak saman tertunggak        (point 2)
```

Perhatikan: walaupun point saman ada skor **rendah** (0.217 — vektornya jauh), ia **satu-satunya** hasil kerana **filter** membuang semua point bukan-saman **dahulu**. Inilah **metadata filtering** — sangat berguna untuk JPJ (cth. cari hanya dalam dokumen bahagian Pelesenan). Ini yang kita guna di [Hari 3 Latihan 11](../hari-3/snippets/lab.md).

> Format filter Qdrant: `must` (WAJIB semua padan / AND), `should` (mana-mana / OR), `must_not` (kecuali).

✅ **Semakan:** Hanya point **saman** dikembalikan, walaupun skornya rendah — filter mengatasi keserupaan vektor.

---

## Latihan 5 — Ambil & Padam Point

**Ambil satu point ikut id:**
```bash
curl http://localhost:6333/collections/qdrant_tutorial/points/1
```

**Padam point (cth. dokumen lapuk):**
```bash
curl -X POST http://localhost:6333/collections/qdrant_tutorial/points/delete \
  -H 'Content-Type: application/json' \
  -d '{"points": [2]}'
```

**Padam seluruh collection** (bersih selepas tutorial):
```bash
curl -X DELETE http://localhost:6333/collections/qdrant_tutorial
```

✅ **Semakan:** Selepas padam point 2, carian tidak lagi memulangkannya. Selepas padam collection, ia hilang dari dashboard.

---

## Kaitan dengan RAG (Hari 2)

Apa yang anda buat **manual** di sini, RAG buat **automatik**:

| Tutorial ini (manual) | RAG Hari 2 (automatik dalam n8n) |
|------------------------|-----------------------------------|
| Anda tulis vektor 4-dim sendiri | **Embeddings OpenAI** jana vektor 1536-dim dari teks |
| Anda `PUT points` dengan curl | **Qdrant Vector Store (Insert)** simpan chunk |
| Anda `POST search` dengan curl | **Qdrant Vector Store (Retrieve)** cari top-K |
| Anda baca payload | **LLM** guna teks payload sebagai konteks jawapan |

> **Kesimpulan:** Qdrant ialah "otak ingatan" RAG — ia simpan makna sebagai vektor & cari yang paling serupa. Faham 5 latihan ini, dan Hari 2 jadi mudah.

---

> **Seterusnya:** [`04-embeddings-vector-db.md`](./04-embeddings-vector-db.md) (kenapa embeddings & cosine) · [Hari 2](../hari-2/README.md) (guna Qdrant dalam RAG penuh) · [qdrant.tech/documentation](https://qdrant.tech/documentation/) (dokumentasi rasmi).
