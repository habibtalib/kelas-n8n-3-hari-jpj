# Nota Konsep: Persediaan Docker 🐳

> Nota latar belakang untuk SESI 7 (Hari 2) & prasyarat kursus. Kita jalankan **n8n + Qdrant + PostgreSQL (+ Ollama)** menggunakan Docker. Nota ini bawa anda dari pemasangan sehingga membuka n8n dalam pelayar.

---

## Apa itu Docker (secara mudah)?

**Docker** membolehkan kita jalankan perisian di dalam **kontena (*container*)** — kotak terpencil yang sudah mengandungi **semua** yang perlu (perisian + tetapan + kebergantungan). "Ia berjalan pada mesin saya" menjadi "ia berjalan **di mana-mana**".

- 📦 **Image** = resipi/*blueprint* perisian (cth. imej `qdrant/qdrant`).
- 🏃 **Container** = image yang sedang **berjalan** (satu salinan hidup).

> **Kenapa Docker untuk kursus ini:** daripada memasang n8n, Qdrant, & PostgreSQL satu-satu (susah & mudah rosak), Docker jalankan **kesemuanya** dengan **satu arahan** — sama pada Windows, Mac & Linux. Ini juga cara kita *deploy* ke pengeluaran nanti ([`09-deployment.md`](./09-deployment.md)).

### Docker Compose

**Docker Compose** menjalankan **beberapa kontena bersama** sebagai satu "*stack*", ditakrifkan dalam satu fail `docker-compose.yml`. Fail ini menyenaraikan setiap perkhidmatan (n8n, Qdrant, Postgres), *port*, & tetapan. Satu arahan menghidupkan semuanya.

---

## Pasang Docker Desktop

**Docker Desktop** ialah cara paling mudah (ada antara muka grafik + `docker` & `docker compose` terus sedia).

| OS | Langkah |
|----|---------|
| **Windows 10/11** | Muat turun **Docker Desktop for Windows** dari [docker.com](https://www.docker.com/products/docker-desktop/). Ia guna **WSL2** (Windows Subsystem for Linux) — pemasang akan minta pasang/aktifkan WSL2. Perlu hak *admin*. |
| **macOS** | Muat turun **Docker Desktop for Mac** — pilih cip **Apple Silicon (M1/M2/M3)** atau **Intel** yang betul. Seret ke *Applications*. |
| **Linux** | Pasang **Docker Engine** + **Docker Compose plugin** melalui pengurus pakej pengedaran anda (cth. `apt`), atau Docker Desktop for Linux. |

> Selepas pasang, **buka Docker Desktop** dan tunggu ia menunjukkan status **"running"** (ikon paus hijau) sebelum meneruskan.

---

## Sahkan pemasangan

Buka **Terminal** (Mac/Linux) atau **PowerShell** (Windows) dan jalankan:

```bash
docker --version
# cth. output: Docker version 27.x.x, build ...

docker compose version
# cth. output: Docker Compose version v2.x.x
```

Jika kedua-duanya memaparkan nombor versi → Docker sedia. ✅

> **Nota:** guna `docker compose` (dua perkataan, versi baharu), **bukan** `docker-compose` (lama, dengan sengkang).

---

## Jalankan *stack* kursus

*Stack* kursus ditakrifkan dalam [`../templates/docker-compose.yml`](../templates/docker-compose.yml) (n8n + Qdrant + PostgreSQL, dan Ollama sebagai pilihan). Langkah:

**1. Masuk ke folder yang mengandungi fail itu:**
```bash
cd templates
```

**2. Hidupkan semua perkhidmatan (mod latar belakang `-d` = *detached*):**
```bash
docker compose up -d
```
Kali pertama akan **muat turun imej** (beberapa minit). Selepas siap, semua kontena berjalan di latar belakang.

**3. Semak kontena yang sedang berjalan:**
```bash
docker compose ps
# atau
docker ps
```
Anda patut nampak `n8n`, `qdrant`, & `postgres` berstatus **Up**.

---

## Buka perkhidmatan dalam pelayar

Setiap perkhidmatan didedahkan pada satu **port** di `localhost` (mesin anda):

| Perkhidmatan | URL | Port | Guna |
|--------------|-----|:----:|------|
| **n8n** | [http://localhost:5678](http://localhost:5678) | 5678 | Kanvas *workflow* — tempat kita bina semua |
| **Qdrant dashboard** | [http://localhost:6333/dashboard](http://localhost:6333/dashboard) | 6333 | Lihat *collections* & vektor (REST API juga di 6333) |
| **PostgreSQL** | (dalaman) | 5432 | Pangkalan data — diakses oleh n8n, bukan pelayar |
| **Ollama** *(pilihan)* | http://localhost:11434 | 11434 | LLM/embeddings tempatan (on-prem) |

> Kali pertama buka n8n (`localhost:5678`), ia minta cipta **akaun pemilik** (owner) tempatan. Ini akaun untuk *instance* anda sahaja.

---

## Hentikan *stack*

```bash
docker compose down
```
Ini **menghentikan & membuang kontena**, tetapi **data kekal** (disimpan dalam Docker *volumes* yang ditakrifkan dalam `docker-compose.yml`). Hidupkan semula bila-bila dengan `docker compose up -d`.

> ⚠️ **Jangan** guna `docker compose down -v` melainkan anda **sengaja** mahu **memadam semua data** (termasuk vektor Qdrant & pangkalan data). Flag `-v` membuang *volumes*.

Arahan berguna lain:
```bash
docker compose logs -f n8n     # lihat log n8n secara langsung
docker compose restart n8n     # mulakan semula satu perkhidmatan
docker compose pull            # muat turun versi imej terkini
```

---

## Penyelesaian masalah (*troubleshooting*)

| Masalah | Gejala | Penyelesaian |
|---------|--------|--------------|
| **Port sudah diguna** | `port is already allocated` / `address already in use` (5678/6333) | Program lain guna port itu. Hentikannya, **atau** ubah pemetaan port dalam `docker-compose.yml` (cth. `5679:5678`) dan buka `localhost:5679`. |
| **Memori rendah** | Kontena mati/*restart* sendiri; n8n perlahan | Beri Docker lebih RAM: **Docker Desktop → Settings → Resources → Memory** (naikkan ke ≥ 6–8GB). Minimum kursus: **8GB RAM** mesin. |
| **WSL2 (Windows)** | Docker tak start; "WSL2 required" | Pasang/kemas kini WSL2: jalankan `wsl --update` dalam PowerShell (admin), pastikan *virtualization* dihidupkan dalam BIOS, & aktifkan integrasi WSL dalam Docker Desktop settings. |
| **Docker Desktop tak "running"** | Arahan `docker` beri ralat sambungan | Buka aplikasi Docker Desktop dan tunggu ikon paus jadi **hijau/stabil** sebelum jalankan arahan. |
| **Imej gagal dimuat turun** | *Timeout* / gagal *pull* | Semak sambungan internet; jalankan `docker compose pull` semula. |
| **Lupa data hilang** | Vektor/akaun hilang selepas restart | Anda mungkin guna `down -v`. Pastikan *volumes* wujud; **elak** flag `-v`. |

---

## Ringkasan arahan

| Tujuan | Arahan |
|--------|--------|
| Semak versi | `docker --version` · `docker compose version` |
| Hidupkan stack | `docker compose up -d` |
| Senarai kontena | `docker compose ps` |
| Lihat log | `docker compose logs -f n8n` |
| Hentikan stack | `docker compose down` |

Selepas *stack* berjalan, kita mula bina dalam n8n. Untuk seni bina & keselamatan *deployment* sebenar: [`08-governance-keselamatan.md`](./08-governance-keselamatan.md) & [`09-deployment.md`](./09-deployment.md).

## Sumber Rasmi

- **[docs.docker.com](https://docs.docker.com/)** — dokumentasi rasmi Docker & Compose.
- **[docs.n8n.io/hosting](https://docs.n8n.io/hosting/)** — self-hosting n8n dengan Docker.
- **[qdrant.tech/documentation](https://qdrant.tech/documentation/)** — dokumentasi Qdrant.
