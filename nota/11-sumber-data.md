# Sumber Data — Latihan vs Pengeluaran 🗄️

Nota konsep untuk kursus **RAG-N8N-JPJ-101**: dari mana pembantu JPJ mendapat datanya, dan **cara n8n membaca data** (API, pangkalan data, fail). Nota ini menjawab soalan lazim: *"Perlukah kita tarik data sebenar dari portal JPJ untuk lab?"*

> **Jawapan ringkas:** **Untuk lab — TIDAK.** Guna **data sintetik** (contoh) yang disediakan. **Untuk pengeluaran** — ya, sambung ke sistem sebenar, tetapi **hanya melalui saluran rasmi yang diluluskan**, bukan mengikis (*scrape*) portal.

---

## 1. Latihan guna data SINTETIK (bukan data sebenar)

Semua lab kursus ini menggunakan **data contoh sintetik**:
- **Dokumen** — [`../templates/sample-docs/`](../templates/sample-docs/) (SOP, pekeliling, FAQ contoh — berpenafian jelas).
- **Rekod** — pangkalan data demo `jpjdemo` (jadual `lesen`, `pemohon`, `saman`) yang dicipta dalam Postgres kursus.

**Kenapa bukan data sebenar dalam latihan?** Rekod JPJ sebenar (lesen, kenderaan, saman) ialah **data peribadi (PDPA)**. Memuatnya ke komputer latihan — apatah lagi menghantarnya ke API awan — ialah **risiko tadbir urus & privasi**. Lihat [`08-governance-keselamatan.md`](./08-governance-keselamatan.md). Data sintetik membolehkan anda belajar **corak** yang sama, dengan **selamat**.

---

## 2. Tiga cara n8n membaca data

n8n boleh menarik data dari tiga jenis sumber — kesemuanya *node* yang sudah ada dalam kursus:

| # | Sumber | Node n8n | Bila guna |
|---|--------|----------|-----------|
| A | **API (REST)** | **HTTP Request** | Sistem ada *endpoint* API (GET/POST) |
| B | **Pangkalan data** | **Postgres** / MySQL / MS SQL | Baca terus jadual (`SELECT …`) |
| C | **Fail** | Read Files / Google Drive / dsb. | Dokumen (PDF/DOCX) → ingest ke **Qdrant** (Hari 2) |

Kursus ini mengajar **ketiga-tiganya**:
- **Fail → RAG** (Hari 2: ingestion ke Qdrant).
- **API** (Hari 1 Asas 2: HTTP Request; Hari 3 tool *Check Licence Status*).
- **Database** (demo di bawah).

---

## 3. Demo: baca DB & dedah sebagai API (dalam n8n anda)

Dua *workflow* demo sudah diimport ke n8n anda (data dari `jpjdemo`):

| Workflow | Corak | Cuba |
|----------|-------|------|
| **H3 · 02 Demo — Baca Data DB (Postgres)** | Manual → **Postgres (SELECT)** → output | Execute → lihat 3 rekod lesen |
| **H3 · 03 Mock API — Status Lesen (Webhook→DB)** | Webhook → Postgres (WHERE ic) → **Respond** | `GET /jpj-lesen?ic=900101-14-5566` |

**Credential Postgres** (cipta sekali, guna kedua-dua workflow):

| Medan | Nilai |
|-------|-------|
| Host | `postgres` *(nama servis Docker — bukan `localhost`)* |
| Port | `5432` |
| Database | `jpjdemo` |
| User | `n8n` |
| Password | *(dari `templates/.env`)* |

> Selepas set credential, **H3 · 03** memberi anda satu **URL webhook** — inilah *endpoint* "mock API" yang boleh disambung ke tool **Check Licence Status** dalam [Hari 3 Latihan 3](../hari-3/snippets/lab.md). API di sini hanyalah "pembalut" (*wrapper*) di hadapan pangkalan data — corak sebenar yang JPJ guna.

> ⚠️ *Workflow* mock menggunakan interpolasi rentetan dalam SQL untuk kesederhanaan. Dalam **pengeluaran**, guna **query berparameter** (`$1`) untuk elak *SQL injection*.

**Bina semula data demo** (jika perlu):
```bash
docker compose exec postgres psql -U n8n -d jpjdemo   # kemudian jalankan SQL jadual lesen/pemohon/saman
```

---

## 4. Untuk pengeluaran — sambung ke sistem JPJ sebenar

Selepas latihan, untuk pembantu JPJ **sebenar**:

- ✅ **Guna API rasmi** sistem JPJ (HTTP Request node), **atau** eksport/DB yang **dibenarkan** — dengan **kebenaran bertulis** daripada pemilik sistem.
- ❌ **Jangan mengikis (*scrape*) portal** JPJ untuk "jadikan API". Ia rapuh, biasanya melanggar terma, dan bagi portal kerajaan — **tidak dibenarkan** tanpa kelulusan.
- 🔒 **Data sensitif → on-premise** (Ollama + Qdrant + n8n *self-hosted*) supaya PII **tidak keluar** dari infrastruktur JPJ — [`09-deployment.md`](./09-deployment.md).
- 📋 **PDPA & kebenaran** — dapatkan kelulusan, hadkan medan yang diambil, nyahpengenalan PII jika boleh — [`08-governance-keselamatan.md`](./08-governance-keselamatan.md).

> **Prinsip:** corak teknikal (API/DB/fail → n8n → RAG/ejen) **sama** untuk latihan & pengeluaran. Yang berbeza hanya **sumber data** (sintetik → rasmi) dan **tempat ia berjalan** (mesin latihan → server on-prem diluluskan).

---

> **Seterusnya:** [`08-governance-keselamatan.md`](./08-governance-keselamatan.md) (tadbir urus & PDPA) · [`09-deployment.md`](./09-deployment.md) (on-prem/residensi data).
