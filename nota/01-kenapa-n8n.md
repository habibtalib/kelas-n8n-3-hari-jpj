# Nota Konsep: Kenapa n8n?

> Nota latar belakang — baca **sebelum** sesi n8n pertama (SESI 5, Hari 1). Fahami **kenapa** kita pilih n8n sebelum belajar **bagaimana** menggunakannya.

---

## Apa itu n8n?

**n8n** (sebut *"n-eight-n"* — ringkasan *"nodemation"*) ialah alat **automasi *workflow* visual** sumber terbuka. Daripada menulis kod baris demi baris, anda **melukis** aliran kerja: seret kotak (*node*), sambungkan dengan garisan, dan n8n menjalankannya mengikut urutan.

Bayangkan carta alir (*flowchart*) yang **benar-benar berfungsi** — bukan sekadar gambar. Setiap kotak melakukan satu tugas (terima permintaan, panggil AI, simpan ke pangkalan data), dan garisan menunjukkan ke mana data mengalir.

```
[Trigger]  →  [Node A]  →  [Node B]  →  [Node C]
 Webhook       AI Model     Format       Response
 masuk         jawab        jawapan      keluar
```

Untuk kursus ini, n8n ialah **kanvas** tempat kita bina Pembantu Pintar JPJ — daripada *workflow* AI ringkas (Hari 1) sehingga ejen berbilang alat (Hari 3) — **tanpa perlu jadi pengaturcara**.

---

## Empat konsep asas n8n

| Konsep | Maksud (mudah) | Contoh dalam kursus |
|--------|----------------|---------------------|
| **Node** | Satu kotak = satu tugas. Ada ratusan jenis (AI, HTTP, pangkalan data, fail). | *AI Agent node*, *Qdrant Vector Store node* |
| **Connection** | Garisan antara node — menentukan **urutan** & **aliran data** dari satu node ke node seterusnya. | Sambung *Webhook* → *AI Agent* → *Respond* |
| **Trigger** | Node **pertama** yang **memulakan** *workflow*. Boleh dicetus manual, ikut jadual, atau oleh *webhook* (permintaan masuk). | *Webhook* (soalan pengguna tiba), *Schedule* (indeks dokumen setiap malam) |
| **Credential** | Simpanan **selamat** untuk kunci rahsia (API key OpenAI, kata laluan Qdrant). Disimpan **berasingan** daripada *workflow*, jadi tak terdedah. | *OpenAI API credential*, *Qdrant credential* |

> **Kenapa *credential* penting untuk JPJ:** kunci API & kata laluan **tidak** ditulis terus dalam *workflow*. Ia disimpan sekali, dirujuk dengan selamat, dan boleh diputar (*rotate*) tanpa mengubah *workflow*. Lihat [`08-governance-keselamatan.md`](./08-governance-keselamatan.md).

---

## Self-hosted vs Cloud

n8n boleh dijalankan dalam dua cara:

| | **Self-hosted** (kita guna) | **n8n Cloud** |
|--|------------------------------|----------------|
| Tempat berjalan | Server/komputer **anda sendiri** (via Docker) | Server n8n (langganan) |
| Data | Kekal dalam infrastruktur **anda** | Melalui server pihak ketiga |
| Kos | Percuma (bayar server sendiri) | Bayaran bulanan |
| Kawalan | Penuh | Terhad |
| Sesuai untuk | **Data sensitif kerajaan** | Pasukan kecil, mula cepat |

> **Untuk JPJ, kita pilih *self-hosted* melalui Docker.** Dokumen JPJ sensitif — dengan *self-hosted*, dokumen & pangkalan data vektor **kekal dalam kawalan JPJ** dan tidak keluar ke *cloud* pihak ketiga. Ini padan dengan keperluan **residensi data**. Persediaan Docker ada di [`05-setup-docker.md`](./05-setup-docker.md).

---

## Bila n8n sesuai vs bila tulis kod

n8n **bukan** pengganti untuk semua pengaturcaraan. Ia bersinar untuk **automasi & mengintegrasi perkhidmatan**.

**✅ Sesuai guna n8n bila:**
- Anda perlu **sambung beberapa perkhidmatan** (AI + pangkalan data + API + e-mel) tanpa banyak kod.
- Logik boleh digambarkan sebagai **aliran langkah demi langkah**.
- Anda mahu **melihat & mengubah** aliran secara visual — mudah diselenggara oleh pegawai bukan pengaturcara.
- Prototaip pantas: dari idea ke *workflow* berjalan dalam beberapa jam.

**⚠️ Kurang sesuai / pertimbang kod bila:**
- Logik pengiraan **sangat kompleks** dengan ribuan baris peraturan.
- Anda membina aplikasi dengan antara muka pengguna tersuai penuh (guna rangka kerja web).
- Prestasi ekstrem (jutaan operasi sesaat) diperlukan.

> Dalam n8n, anda **masih boleh** tulis sedikit kod bila perlu (melalui *Code node*) — tetapi 95% kursus ini **tanpa kod**.

---

## Kenapa n8n padan untuk membina AI tanpa kod di JPJ

1. **AI nodes terbina dalam** — n8n ada *AI Agent*, *LLM Chain*, *Vector Store*, & *Embeddings* node siap sedia. Padan terus dengan OpenAI, Ollama & Qdrant yang kita guna.
2. **Visual = mudah difahami** — pegawai JPJ boleh **lihat** bagaimana pembantu bekerja, tanpa membaca kod. Senang diserahkan antara pasukan.
3. **Self-hosted = data terkawal** — sesuai untuk dokumen JPJ yang sensitif.
4. **Sumber terbuka** — tiada *vendor lock-in*; boleh diaudit; percuma untuk mula.
5. **Boleh berkembang** — bermula dari satu *workflow* ringkas, berkembang ke ejen berbilang alat yang di-*deploy* ke pengeluaran ([`09-deployment.md`](./09-deployment.md)).

---

## Siapa guna n8n? (contoh sebenar)

n8n digunakan oleh organisasi merentas dunia untuk automasi & aliran kerja AI:

- **Delivery Hero** — automasi operasi dalaman berskala besar
- **Cisco** — integrasi proses IT
- **Adobe, Twilio, Vodafone** — antara syarikat yang menggunakan/menyebut n8n dalam aliran kerja mereka
- Ribuan agensi & pasukan IT kerajaan/swasta untuk automasi *back-office* dan kini **pembantu AI berasaskan RAG**

Ini menunjukkan n8n bukan mainan — ia digunakan pada skala pengeluaran sebenar.

---

## Kaitan dengan projek kursus

Sepanjang 3 hari, **setiap** komponen Pembantu Pintar JPJ dibina dalam n8n: *workflow* AI pertama (Hari 1), *pipeline* ingestion & retrieval RAG (Hari 2), dan ejen berbilang alat (Hari 3). n8n ialah "gam" yang menyatukan LLM, embeddings, dan Qdrant menjadi satu pembantu berfungsi.

Seterusnya: [`02-apa-itu-llm.md`](./02-apa-itu-llm.md) — fahami "otak" di sebalik pembantu sebelum kita menyambungnya dalam n8n.

---

## Sumber Rasmi

- **[n8n.io](https://n8n.io)** — laman rasmi n8n.
- **[docs.n8n.io](https://docs.n8n.io)** — dokumentasi rasmi (nodes, credentials, AI nodes, self-hosting).
- **[docs.n8n.io/hosting](https://docs.n8n.io/hosting/)** — panduan *self-hosting* dengan Docker.
