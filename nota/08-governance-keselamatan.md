# Tadbir Urus & Keselamatan AI 🔒

Nota konsep untuk kursus **RAG-N8N-JPJ-101**. Bahagian ini menyokong **SESI 15 (Hari 3)** dan menjadi rangka fikir sepanjang kursus: apabila kita membina pembantu AI yang mengendalikan dokumen JPJ, **keselamatan dan tadbir urus data bukan tambahan — ia keperluan asas.**

> **Kenapa ini penting:** Dokumen JPJ — pekeliling dalaman, SOP, data pemohon — adalah maklumat sensitif kerajaan. Satu pembantu AI yang direka cuai boleh **membocorkan data ke pihak ketiga**, memberi jawapan salah yang orang awam anggap rasmi, atau melanggar undang-undang perlindungan data. Nota ini menerangkan cara elak semua itu.

---

## 1. Residensi Data (Data Residency)

**Residensi data** bermaksud: *di mana* fizikalnya data anda disimpan dan diproses. Untuk agensi kerajaan, jawapannya selalunya perlu **"dalam negara, dalam kawalan kami"**.

| Senario | Risiko | Postur disyorkan JPJ |
|---------|--------|----------------------|
| Hantar dokumen ke API awan luar negara (cth. OpenAI) | Data JPJ keluar dari infrastruktur kerajaan | Guna untuk **data awam/tidak sensitif** sahaja |
| Jalankan model **secara tempatan (Ollama)** di server JPJ | Data **tidak pernah** meninggalkan rangkaian JPJ | ✅ **Disyorkan** untuk dokumen sensitif |
| Simpan vektor & pangkalan data di VPS kerajaan / *on-premise* | Kawalan penuh, boleh diaudit | ✅ Disyorkan |

> **Prinsip utama:** Untuk dokumen sulit/terhad, gunakan konfigurasi **Ollama + on-premise** supaya keseluruhan aliran — dokumen, *embedding*, carian, jawapan — berlaku dalam infrastruktur yang dikawal JPJ. Lihat [`05-setup-docker.md`](./05-setup-docker.md) untuk servis `ollama` dalam stack, dan [`09-deployment.md`](./09-deployment.md) untuk *hosting* on-prem.

**Bila OpenAI (awan) masih boleh diguna?** Untuk latihan, prototaip, dan dokumen yang memang **awam** (cth. FAQ yang sudah tersiar di laman web JPJ). Untuk pengeluaran dengan data sensitif → beralih ke Ollama.

---

## 2. Perlindungan Data Peribadi (PDPA)

Malaysia mempunyai **Akta Perlindungan Data Peribadi 2010 (PDPA)**. Walaupun agensi kerajaan mempunyai kedudukan khusus di bawah akta, prinsipnya kekal panduan baik:

- **Minimum data** — jangan masukkan data peribadi (No. KP penuh, alamat, nombor telefon) ke dalam pangkalan pengetahuan RAG melainkan benar-benar perlu.
- **Nyahpengenalan (de-identification)** — kalau boleh, tanggalkan/kabur PII sebelum dokumen di-*embed* dan disimpan dalam Qdrant.
- **Tujuan jelas** — pembantu ini untuk menjawab *prosedur*, bukan menyimpan rekod peribadi orang awam.
- **Jangan bocor PII ke API pihak ketiga** — jika guna OpenAI, ingat setiap teks yang dihantar keluar dari rangkaian JPJ. Untuk data mengandungi PII → Ollama tempatan.

> ⚠️ **Peringatan:** Jika pembantu perlu mengakses status lesen/pemohon sebenar (cth. dalam ejen Hari 3), jangan tanam data itu dalam *knowledge base*. Sebaliknya **panggil API rasmi secara langsung** ketika perlu (lihat ejen berbilang alat di [`06-ai-agents.md`](./06-ai-agents.md)) supaya data peribadi tidak diduplikasi/dikekalkan.

---

## 3. Kawalan Akses & Credentials dalam n8n

n8n menyimpan kunci API dan kata laluan sebagai **Credentials**. Amalan selamat:

- **Jangan** tulis kunci API terus dalam parameter *node* — gunakan sistem **Credentials** n8n, dan rujuk melalui pembolehubah persekitaran (`.env`).
- Hadkan siapa boleh log masuk ke antara muka n8n (`N8N_BASIC_AUTH_*` atau SSO) — lihat [`05-setup-docker.md`](./05-setup-docker.md).
- Asingkan *credentials* mengikut peranan — pembangun vs operator.
- Putar (rotate) kunci API secara berkala; batalkan yang bocor serta-merta.
- Jangan simpan fail `.env` sebenar dalam kawalan versi (guna `.env.example` sahaja).

---

## 4. Guardrail Halusinasi — Sentiasa Petik Sumber

Seluruh sebab kita guna **RAG** ialah untuk **mengikat jawapan pada dokumen sebenar**. Ini juga kawalan keselamatan:

- *System prompt* mesti mengarahkan model **hanya** menjawab daripada konteks yang diberi, dan berkata *"Maaf, maklumat ini tidak dijumpai dalam dokumen JPJ yang disediakan"* jika tiada padanan. Lihat contoh dalam [`07-prompt-engineering.md`](./07-prompt-engineering.md).
- Setiap jawapan patut **memetik dokumen sumber** (nama fail / tajuk pekeliling) supaya pegawai boleh sahkan.
- Untuk keputusan yang memberi kesan undang-undang/penguatkuasaan (cth. saman, kompaun), kekalkan **human-in-the-loop** — pembantu memberi *maklumat*, pegawai membuat *keputusan*.

> **Kesan salah:** Jawapan AI yang salah tetapi kelihatan yakin boleh disalah anggap sebagai nasihat rasmi JPJ. Petikan sumber + penafian mengurangkan risiko ini.

---

## 5. Audit & Kebolehjejakan (Traceability)

- Log setiap pertanyaan & jawapan (tanpa PII berlebihan) untuk tujuan audit dan penambahbaikan.
- Simpan **versi dokumen** dalam *metadata* Qdrant — supaya anda tahu jawapan datang daripada pekeliling versi mana.
- Pantau penggunaan (lihat Grafana dalam [`09-deployment.md`](./09-deployment.md)).

---

## Senarai Semak Tadbir Urus (Ringkas)

- [ ] Dokumen sensitif → aliran **Ollama on-premise** (bukan API awan)
- [ ] PII dinyahpengenalan / tidak disimpan dalam *knowledge base*
- [ ] Kunci API dalam **Credentials** n8n, bukan *hardcoded*; `.env` tidak di-*commit*
- [ ] *System prompt* berpaksikan konteks + berkata "tidak dijumpai" bila tiada padanan
- [ ] Setiap jawapan **memetik dokumen sumber**
- [ ] *Human-in-the-loop* untuk keputusan penguatkuasaan
- [ ] Log & audit diaktifkan; versi dokumen dalam *metadata*

> **Seterusnya:** [`09-deployment.md`](./09-deployment.md) — bagaimana melaksanakan semua ini pada server pengeluaran (VPS/on-prem) dengan HTTPS, sandaran & pemantauan.
