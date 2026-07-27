# Persediaan n8n — Cloud vs Tempatan (Local) ☁️🐳

Nota konsep untuk kursus **RAG-N8N-JPJ-101**. Sebelum bina *workflow* pertama (**Hari 1 / SESI 5**), anda perlukan **satu *instance* n8n yang boleh diakses**. Ada dua jalan — pilih satu:

- **Path A — n8n Cloud** ☁️ : paling pantas untuk mula (tiada pemasangan). Sesuai untuk **latihan/percubaan**.
- **Path B — n8n Tempatan (Docker)** 🐳 : jalan di komputer/server sendiri. **Pilihan kursus & disyorkan untuk JPJ** kerana data kekal dalam kawalan anda.

> **Ringkas untuk JPJ:** Untuk *belajar* pantas → Cloud. Untuk *data sebenar JPJ* → **Tempatan (self-host)**, kerana dokumen sensitif **tidak boleh** keluar ke *cloud* pihak ketiga (residensi data — lihat [`08-governance-keselamatan.md`](./08-governance-keselamatan.md)).

---

## Cloud vs Tempatan — mana satu?

| | ☁️ **n8n Cloud** | 🐳 **Tempatan (Docker)** |
|---|---|---|
| Pemasangan | Tiada — daftar & guna | Pasang Docker Desktop |
| Masa untuk mula | ~2 minit | ~15–20 minit (kali pertama) |
| Kos | Percubaan percuma, kemudian langganan bulanan | **Percuma** (perisian sumber terbuka) |
| Kawalan data | Data di pelayan n8n (luar JPJ) | **Data kekal dalam infrastruktur JPJ** |
| Kemas kini | Automatik | Anda kawal (`docker compose pull`) |
| Sesuai untuk | Belajar, prototaip, data awam | **Pengeluaran JPJ**, data sensitif |
| Guna Ollama/Qdrant tempatan | Terhad | ✅ Ya (stack penuh) |

> **Nota kursus:** Lab Hari 2 & 3 memerlukan **Qdrant** dan (pilihan) **Ollama** berjalan di sebelah n8n. Ini paling lancar dengan **Path B (Docker)** kerana `docker-compose.yml` kursus sudah menyatukan kesemuanya. Jika anda guna Cloud untuk Hari 1, anda tetap perlu beralih ke Docker untuk stack RAG penuh.

---

## Path A — n8n Cloud (paling pantas) ☁️

1. Pergi ke **[n8n.io](https://n8n.io)** → klik **"Get started"** / **"Start free trial"**.
2. Daftar dengan emel (percubaan biasanya **tiada kad kredit** diperlukan).
3. Pilih nama *instance* — anda dapat URL seperti `https://<nama-anda>.app.n8n.cloud`.
4. Sahkan emel, log masuk — anda terus masuk ke **editor n8n**.
5. **Sedia!** Teruskan ke [Hari 1, SESI 5](../hari-1/README.md) untuk bina *workflow* AI pertama.

> Selepas tempoh percubaan tamat, n8n Cloud memerlukan langganan berbayar (bermula sekitar **~€20/bulan**, harga berubah — semak laman rasmi). Untuk kursus, percubaan memadai bagi Hari 1.

---

## Path B — n8n Tempatan via Docker (disyorkan) 🐳

Ini jalan yang digunakan sepanjang kursus. Langkah **penuh** ada di [`05-setup-docker.md`](./05-setup-docker.md). Ringkasnya:

1. **Pasang Docker Desktop** (Windows/macOS/Linux) — lihat [`05-setup-docker.md`](./05-setup-docker.md).
2. Sahkan:
   ```bash
   docker --version
   docker compose version
   ```
3. Dari folder `templates/` kursus:
   ```bash
   cp .env.example .env      # sunting kata laluan jika mahu
   docker compose up -d      # jalankan n8n + PostgreSQL + Qdrant
   docker compose ps         # sahkan servis "healthy"
   ```
4. Buka **n8n** di **[http://localhost:5678](http://localhost:5678)** (log masuk guna `N8N_BASIC_AUTH_USER` / `..._PASSWORD` dalam `.env`).
   Qdrant dashboard: **[http://localhost:6333/dashboard](http://localhost:6333/dashboard)**.
5. Hentikan bila selesai: `docker compose down` (data kekal dalam *volume*).

> Rujukan: [`../templates/docker-compose.yml`](../templates/docker-compose.yml) · [`../templates/.env.example`](../templates/.env.example) · [`05-setup-docker.md`](./05-setup-docker.md).

---

## Platform Percuma (Tanpa Setup Local) 🆓

Jika anda **tidak mahu** memasang apa-apa pada komputer sendiri, ada dua strategi percuma: (1) **self-host di server awan percuma** — masih guna `docker-compose.yml` yang sama, tetapi anda kekal kawal data; atau (2) guna **perkhidmatan terurus percuma** — paling pantas, tetapi data keluar dari rangkaian anda.

> ⚠️ **Kadar percuma sering berubah** — angka di bawah sah pada **Julai 2026**; sentiasa semak had semasa di laman rasmi.

### 🥇 Pilihan terbaik: self-host di Oracle Cloud "Always Free"

Jalankan **stack kursus yang sama** (`docker compose up -d`) pada VM awan percuma, bukan komputer riba anda.

- **Oracle Cloud Always Free** — VM ARM (Ampere A1) **percuma selamanya**. *(Nota: had dikurangkan Jun 2026 daripada 4 OCPU/24 GB kepada **2 OCPU / 12 GB RAM**, + ~200 GB storan.)* Masih cukup untuk **n8n + Qdrant** (Ollama model kecil agak ketat pada 12 GB).
- **Kelebihan untuk JPJ:** percuma **dan** *self-hosted* — data kekal dalam kawalan anda (bukan di awan pihak ketiga). Ini satu-satunya pilihan percuma yang mengekalkan postur residensi data.
- Persediaan = cipta VM → pasang Docker → `docker compose up -d` (sama seperti [`05-setup-docker.md`](./05-setup-docker.md)).

### Platform lain untuk *hosting* n8n percuma

| Platform | Kadar percuma | Sesuai untuk |
|----------|---------------|--------------|
| **Oracle Cloud** | VM Always Free (2 OCPU / 12 GB) | ✅ Stack penuh, kekal |
| **Google Cloud** | Kredit $300 (90 hari) + VM kecil always-free | Skala percubaan |
| **Railway / Render / Koyeb / Fly.io** | Tier percuma/kredit kecil (kerap "tidur"/ephemeral) | Demo pantas, bukan data kekal |
| **Hugging Face Spaces** | Docker space percuma (ephemeral) | Demo buang |
| **n8n Cloud** | **Percubaan 14 hari** (tiada kad kredit), kemudian berbayar (~$20/bln) | Mula paling pantas — *bukan* percuma kekal |

### Komponen terurus percuma (jika tak mahu jalankan sendiri)

| Komponen | Pilihan percuma |
|----------|-----------------|
| **Vector DB** | **Qdrant Cloud** kluster percuma **1 GB** (0.5 vCPU · 4 GB disk, kekal) · Pinecone Starter · Supabase pgvector |
| **LLM (chat)** | **Google Gemini** API (tier percuma) · **Groq** (laju, percuma) · OpenRouter model `:free` · Mistral |
| **Embeddings** | **Gemini embeddings** (percuma) · Cohere (percubaan) · *(atau Ollama = tempatan)* |

> ⚠️ **Amaran JPJ:** setiap pilihan terurus/awan bermakna dokumen **keluar** dari infrastruktur JPJ — hanya sesuai untuk **latihan/demo dengan dokumen contoh sintetik**, **bukan** data JPJ sebenar. Untuk pengeluaran dengan data sensitif, kekal **on-prem (Ollama + Qdrant self-hosted)** — lihat [`08-governance-keselamatan.md`](./08-governance-keselamatan.md). **Oracle Always Free** ialah pilihan yang percuma **dan** mengekalkan kawalan data.

> **Cadangan kursus:** paling pantas = **n8n Cloud (percubaan) + Qdrant Cloud percuma + Gemini API percuma**; terbaik untuk "percuma & kekal kawalan data JPJ" = **Oracle Cloud Always Free** menjalankan stack Docker kursus.

---

## (Rujukan) Path C — VPS / On-Premise untuk pengeluaran 🖥️

Untuk **penggunaan sebenar** (bukan latihan), n8n biasanya dipasang pada **server VPS kerajaan atau on-premise JPJ** — masih guna Docker Compose, tetapi ditambah *reverse proxy* + HTTPS, sandaran, dan pemantauan. Ini dibincang pada **Hari 3 / SESI 15** dan diperincikan dalam [`09-deployment.md`](./09-deployment.md).

---

## Senarai Semak Persediaan

- [ ] Saya boleh log masuk ke satu *instance* n8n (Cloud **atau** tempatan)
- [ ] Editor n8n terbuka dalam pelayar (kanvas kosong kelihatan)
- [ ] *(Path B)* `docker compose ps` menunjukkan `n8n`, `postgres`, `qdrant` berjalan
- [ ] Saya faham: untuk **data sensitif JPJ**, gunakan **tempatan/on-prem**, bukan Cloud
- [ ] Sedia untuk [Lab Hari 1](../hari-1/snippets/lab.md)

> **Seterusnya:** [`01-kenapa-n8n.md`](./01-kenapa-n8n.md) untuk *kenapa* n8n, atau terus ke [Hari 1](../hari-1/README.md) untuk mula bina.
