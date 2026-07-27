# Nota Konsep: Prompt Engineering

> Nota latar belakang untuk SESI 2 (Hari 1) & SESI 14 (Hari 3). *Prompt* ialah **arahan** yang kita beri LLM. Prompt yang baik = jawapan tepat & selamat; prompt lemah = halusinasi. Ini kemahiran paling berbaloi dalam kursus.

---

## Kenapa prompt penting

LLM melakukan **apa yang anda minta** — betul-betul. Ia tak dapat baca fikiran anda. Prompt yang **kabur** menghasilkan jawapan **kabur** (atau direka). Prompt yang **jelas & berstruktur** menghasilkan jawapan **tepat & konsisten**.

> Dalam RAG, prompt jugalah **guardrail** utama menentang halusinasi: kita arahkan LLM menjawab **hanya** dari konteks yang diberi, dan mengaku bila tidak tahu.

---

## Anatomi prompt yang baik

Prompt yang kukuh biasanya ada 5 bahagian:

| Bahagian | Fungsi | Contoh |
|----------|--------|--------|
| **Role** (peranan) | Siapa LLM patut jadi | "Anda pembantu pegawai JPJ." |
| **Context** (konteks) | Maklumat latar / dokumen | Potongan SOP yang di-*retrieve* |
| **Instruction** (arahan) | Apa nak buat | "Jawab soalan pengguna." |
| **Constraints** (kekangan) | Peraturan & had | "Guna hanya konteks. Petik sumber. Jangan meneka." |
| **Examples** (contoh) | Tunjuk format dikehendaki | 1–2 contoh soal-jawab |

> Tak semua prompt perlukan kelima-lima, tetapi **Role + Instruction + Constraints** hampir selalu berguna. Untuk RAG, tambah **Context**. Untuk tugas berformat, tambah **Examples**.

---

## System prompt vs User prompt

| | **System prompt** | **User prompt** |
|--|-------------------|-----------------|
| Siapa tulis | Pembina (anda) | Pengguna akhir |
| Bila | Ditetapkan **sekali**, tetap | Berubah **setiap** soalan |
| Kandungan | Peranan, peraturan, guardrail | Soalan sebenar |
| Contoh | "Anda Pembantu Pintar JPJ. Petik sumber..." | "Macam mana nak renew lesen kelas D?" |

> System prompt ialah tempat anda **kunci tingkah laku** (nada, peraturan, anti-halusinasi). Pengguna tak nampak & tak boleh ubahnya.

---

## Few-shot prompting

**Few-shot** = beri LLM **beberapa contoh** input→output supaya ia meniru corak. (Tanpa contoh = *zero-shot*.)

```
Contoh:
Soalan: Apa kelas lesen untuk motosikal 250cc?
Jawapan: Kelas B2. (Sumber: Panduan Kelas Lesen, ms 2)

Soalan: Apa kelas lesen untuk kereta persendirian?
Jawapan: Kelas D. (Sumber: Panduan Kelas Lesen, ms 2)

Sekarang jawab dengan format sama:
Soalan: {soalan pengguna}
```

> Few-shot berguna bila anda mahu **format tetap** (cth. sentiasa "Jawapan + Sumber"). Untuk RAG mudah, kadang satu contoh sudah cukup.

---

## Grounding prompt RAG (paling penting!)

Inti RAG: arahkan LLM menjawab **hanya** dari konteks yang di-*retrieve*, dan balas *"tidak dijumpai dalam dokumen"* jika tiada. Ini **guardrail anti-halusinasi** utama.

**System prompt RAG (Inggeris — amalan biasa):**
```
You are Pembantu Pintar JPJ, an assistant for Malaysian Road Transport
Department (JPJ) officers and the public.

Rules:
1. Answer ONLY using the information in the <context> below.
2. Always cite the source document and page/section.
3. If the answer is NOT in the context, reply exactly:
   "Maaf, maklumat ini tidak dijumpai dalam dokumen." Do NOT guess.
4. Answer in the same language as the question (Bahasa Melayu or English).
5. Be concise and factual. Do not give legal advice.

<context>
{retrieved_chunks}
</context>
```
**User prompt:**
```
Soalan: {soalan pengguna}
```

> **Kenapa arahan #3 kritikal:** tanpa ia, LLM akan **mereka** jawapan bila konteks tak cukup. Dengan ia, pembantu **mengaku tidak tahu** — jauh lebih selamat & dipercayai untuk kegunaan kerajaan. Rujuk [`08-governance-keselamatan.md`](./08-governance-keselamatan.md).

---

## Prompt untuk ejen & penerangan tool

Untuk **ejen** (Hari 3), dua tempat prompt penting:

**1. System prompt ejen** — tetapkan peranan & bila guna alat:
```
You are the JPJ Service Agent. You have tools to search the knowledge base,
check licence status, and create support tickets. Think step by step: decide
which tool to use, use it, then answer. Always cite KB sources. For any
enforcement or penalty decision, do NOT decide — create a ticket for a human
officer.
```

**2. Penerangan tool** — beritahu LLM **bila** setiap alat digunakan (rujuk [`06-ai-agents.md`](./06-ai-agents.md)):
```
Tool: cari_KB
Description: Search JPJ documents (SOPs, acts, circulars) for procedure and
policy information. Use this for any question about how a process works.

Tool: semak_status_lesen
Description: Check the current status of a driving licence by IC number.
Use ONLY when the user asks about their own licence status.
```

> Penerangan tool yang kabur = ejen pilih alat salah. Tulis ia seperti arahan jelas kepada pekerja baru.

---

## Contoh prompt gaya-JPJ (BM soalan, English system prompt)

**Contoh 1 — Prosedur lesen (RAG chain):**
- *System (EN):* seperti "System prompt RAG" di atas.
- *User (BM):* `"Apakah dokumen yang diperlukan untuk pembaharuan lesen memandu kelas D selama 5 tahun?"`
- *Jangkaan:* jawapan ringkas + petikan dokumen; jika tiada dalam konteks → *"tidak dijumpai dalam dokumen"*.

**Contoh 2 — Saman/kompaun:**
- *User (BM):* `"Macam mana nak semak dan bayar kompaun tertunggak?"`
- *Constraint:* mesti petik SOP/pekeliling sumber; jangan nyatakan kadar melainkan ada dalam konteks.

**Contoh 3 — Grounding negatif (uji guardrail):**
- *User (BM):* `"Berapa denda letak kereta atas bulan?"` (soalan mengarut/tiada dokumen)
- *Jangkaan:* pembantu balas *"Maaf, maklumat ini tidak dijumpai dalam dokumen."* — **bukan** jawapan direka. Inilah ujian yang menunjukkan guardrail berfungsi.

---

## Petua ringkas prompt engineering

| Petua | Kenapa |
|-------|--------|
| Jadilah **spesifik** — nyatakan format & had | Kurang jawapan melantur |
| Letak **peraturan penting di awal & akhir** | LLM beri perhatian lebih pada hujung prompt |
| Guna **temperature rendah (0–0.2)** untuk fakta | Jawapan konsisten (rujuk [`02`](./02-apa-itu-llm.md)) |
| **Uji dengan soalan sukar** (& yang tiada jawapan) | Sahkan guardrail berfungsi |
| **Ulang & perhalus** (iterate) | Prompt jarang sempurna kali pertama — ini "engineering" |

> **Prinsip kursus:** *AI membantu, anda memandu.* Sentiasa semak jawapan yang dijana terhadap dokumen sumber sebelum percaya.

Seterusnya: [`08-governance-keselamatan.md`](./08-governance-keselamatan.md) — menjadikan pembantu selamat & patuh untuk kegunaan JPJ.
