# Dokumen Contoh JPJ (Sample Docs)

> ⚠️ **PENAFIAN PENTING:** Semua dokumen dalam folder ini adalah **contoh sintetik untuk latihan sahaja** — **bukan** pekeliling, SOP, atau dokumen rasmi JPJ. Semua angka, tempoh, kadar bayaran dan prosedur adalah **anggaran latihan** yang direka untuk kursus **RAG-N8N-JPJ-101**. **Jangan** gunakannya sebagai rujukan sebenar. Untuk penggunaan pengeluaran, **gantikan sepenuhnya** dengan dokumen rasmi JPJ yang sah dan terkini.

Folder ini menyediakan "korpus pengetahuan" contoh untuk lab **Hari 2** (pengindeksan/*ingestion* & *retrieval*) dan **Hari 3** (ejen). Pembantu RAG anda akan **membaca, memecah (chunk), meng-*embed*, dan mencari** dokumen-dokumen ini.

## Senarai Dokumen

| Fail | Kandungan | Digunakan dalam |
|------|-----------|-----------------|
| [`01-lesen-memandu.md`](./01-lesen-memandu.md) | Prosedur & kelayakan lesen memandu (LDL, P, CDL, GDL), pembaharuan, dokumen, bayaran contoh | Hari 2 & 3 |
| [`02-pendaftaran-kenderaan.md`](./02-pendaftaran-kenderaan.md) | Pendaftaran kenderaan baharu, pindah milik (tukar hak milik), dokumen & proses | Hari 2 & 3 |
| [`03-saman-kompaun.md`](./03-saman-kompaun.md) | Jenis saman/kompaun, semak tertunggak, kadar diskaun, bayaran, rayuan | Hari 2 & 3 |
| [`04-sop-kaunter.md`](./04-sop-kaunter.md) | SOP dalaman ringkas pegawai kaunter, aliran khidmat, piagam pelanggan | Hari 2 & 3 |
| [`05-faq.md`](./05-faq.md) | 16 soalan lazim orang awam + jawapan (lesen, pendaftaran, saman, kaunter) | Hari 2 & 3 |

## Cara Guna

1. Pada **Hari 2**, muat naik / arahkan *ingestion workflow* ke folder ini untuk mengindeks kesemua dokumen ke dalam koleksi Qdrant `jpj_knowledge` (lihat [`../workflows/02-ingestion-workflow.json`](../workflows/02-ingestion-workflow.json)).
2. Uji *retrieval* dengan soalan seperti:
   - *"Apakah dokumen diperlukan untuk memperbaharui CDL?"*
   - *"Bagaimana proses pindah milik kenderaan?"*
   - *"Bagaimana cara menyemak saman tertunggak?"*
3. Sahkan jawapan **memetik dokumen sumber** (contoh: `01-lesen-memandu.md`) — inilah bukti jawapan berpaksikan sumber (*grounded*), bukan halusinasi.

## Menggantikan dengan Dokumen Sebenar

Untuk kegunaan sebenar JPJ:
- Ganti fail-fail ini dengan pekeliling/SOP/FAQ rasmi (PDF atau Markdown).
- Kekalkan **penafian & kawalan tadbir urus** — lihat [`../../nota/08-governance-keselamatan.md`](../../nota/08-governance-keselamatan.md).
- Untuk dokumen sensitif, jalankan aliran *embedding* secara **on-premise (Ollama)** supaya data tidak keluar dari rangkaian JPJ.
