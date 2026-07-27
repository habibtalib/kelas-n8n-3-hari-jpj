# Deployment 🚀

Nota konsep untuk kursus **RAG-N8N-JPJ-101**, menyokong **SESI 15 (Hari 3)**. Setakat ini kita menjalankan semuanya pada **komputer riba** melalui Docker. Nota ini menerangkan cara memindahkan pembantu yang sama ke **server pengeluaran** — VPS kerajaan atau server *on-premise* JPJ — dengan selamat dan boleh dipercayai.

> **Kenapa berbeza daripada laptop?** Di pengeluaran, pembantu perlu **sentiasa hidup**, dicapai oleh ramai pengguna, dilindungi HTTPS, disandarkan, dan dipantau. Konsep Docker yang sama digunakan — kita cuma tambah beberapa lapisan.

---

## 1. Dari Laptop ke Server — Gambaran Seni Bina

```
                Pengguna (pegawai / kaunter / awam)
                              │  HTTPS
                              ▼
                    Reverse Proxy (Traefik / nginx)
                              │
                              ▼
                            n8n  ──────────────┐
                              │                │
                   ┌──────────┼──────────┐     │
                   ▼          ▼          ▼     ▼
            OpenAI / Ollama  Qdrant   PostgreSQL  (log)
             (LLM+embed)   (vektor)   (data n8n)
```

Untuk data sensitif JPJ, gantikan OpenAI dengan **Ollama tempatan** supaya keseluruhan blok ini berada dalam rangkaian JPJ (lihat [`08-governance-keselamatan.md`](./08-governance-keselamatan.md)).

---

## 2. Docker Compose di Server

Stack yang sama seperti [`05-setup-docker.md`](./05-setup-docker.md) dan [`../templates/docker-compose.yml`](../templates/docker-compose.yml) boleh dijalankan di server:

```bash
# di server (Linux)
git clone <repo-anda> && cd <repo-anda>/templates
cp .env.example .env          # kemudian sunting nilai sebenar
docker compose up -d          # jalankan di latar belakang
docker compose ps             # sahkan semua servis "healthy"
```

**Perbezaan utama berbanding laptop:**

| Aspek | Laptop (latihan) | Server (pengeluaran) |
|-------|------------------|----------------------|
| Akses | `localhost` | Nama domain + HTTPS |
| Kata laluan | ringkas | kuat, unik, dari secret manager |
| Data | volume tempatan | volume + **sandaran berjadual** |
| Kebolehcapaian | 1 pengguna | ramai (di belakang reverse proxy) |
| Model | OpenAI awan | **Ollama on-prem** (data sensitif) |

---

## 3. Reverse Proxy + HTTPS

Jangan dedahkan port `5678` (n8n) terus ke internet. Letakkan **reverse proxy** di hadapan:

- **Traefik** atau **nginx** menerima trafik pada port 443 (HTTPS) dan menghala ke n8n secara dalaman.
- Guna sijil TLS percuma **Let's Encrypt** (Traefik boleh urus automatik).
- Set `N8N_HOST`, `N8N_PROTOCOL=https`, dan `WEBHOOK_URL` supaya *webhook* n8n menjana URL `https://` yang betul.

> Untuk on-prem tanpa internet, gunakan sijil dalaman (PKI kerajaan) dan bukan Let's Encrypt.

---

## 4. Pembolehubah Persekitaran & Rahsia (Secrets)

- Semua kunci/kata laluan dalam **`.env`** (rujuk [`../templates/.env.example`](../templates/.env.example)) — **jangan** *commit* fail `.env` sebenar.
- Untuk pengeluaran, pertimbang *secret manager* (cth. Docker secrets, HashiCorp Vault).
- Guna kata laluan **kuat & unik** untuk PostgreSQL, n8n basic auth, dan mana-mana API.

---

## 5. Sandaran (Backup) — Kritikal

Dua perkara **mesti** disandarkan secara berjadual:

1. **Qdrant** — koleksi vektor `jpj_knowledge` (indeks dokumen anda).
   ```bash
   # snapshot koleksi Qdrant
   curl -X POST http://localhost:6333/collections/jpj_knowledge/snapshots
   ```
2. **PostgreSQL** — pangkalan data n8n (workflow, credentials, sejarah eksekusi).
   ```bash
   docker compose exec postgres pg_dump -U n8n n8n > backup_n8n_$(date +%F).sql
   ```

Simpan sandaran di lokasi **berasingan** daripada server, dan **uji pemulihan** sekurang-kurangnya sekali.

> Jika dokumen sumber masih ada, koleksi Qdrant boleh **dibina semula** dengan menjalankan *ingestion workflow* semula — tetapi sandaran menjimatkan masa.

---

## 6. Pemantauan & Logging

- **Grafana** (dalam stack) untuk papan pemuka metrik: penggunaan CPU/memori, bilangan pertanyaan, masa respons.
- **Log n8n** — semak eksekusi gagal (`docker compose logs -f n8n`).
- Log setiap pertanyaan/jawapan untuk audit & penambahbaikan (tanpa PII berlebihan — lihat [`08-governance-keselamatan.md`](./08-governance-keselamatan.md)).
- Set makluman (alert) untuk servis yang tumbang atau kos API melonjak.

---

## 7. Skala (Scaling) — Asas

Untuk kebanyakan kes guna dalaman JPJ, satu server yang cukup gagah sudah memadai. Bila trafik meningkat:

- Pisahkan Qdrant & PostgreSQL ke server sendiri.
- Jalankan n8n dalam mod **queue** (worker berbilang) untuk *throughput* tinggi.
- Untuk Ollama, gunakan mesin ber-GPU bagi respons lebih pantas.

---

## Senarai Semak Pra-Keluaran (Pre-Launch)

- [ ] Docker Compose berjalan; semua servis `healthy` (`docker compose ps`)
- [ ] Reverse proxy + **HTTPS** aktif; port dalaman tidak terdedah ke internet
- [ ] Kata laluan kuat; `.env` **tidak** di-*commit*; rahsia diurus selamat
- [ ] Data sensitif → **Ollama on-premise** (bukan API awan)
- [ ] Sandaran berjadual untuk **Qdrant** & **PostgreSQL**; pemulihan diuji
- [ ] Pemantauan (Grafana) + log + makluman diaktifkan
- [ ] *System prompt* berpaksikan sumber + petikan dokumen (anti-halusinasi)
- [ ] Semakan tadbir urus [`08-governance-keselamatan.md`](./08-governance-keselamatan.md) lengkap
- [ ] Ujian penerimaan (UAT) dengan soalan JPJ sebenar lulus

> **Ringkasan:** Deployment JPJ yang baik = **Docker + reverse proxy HTTPS + Ollama on-prem + sandaran + pemantauan + guardrail sumber**. Mulakan kecil (satu server), pantau, kembangkan bila perlu.
