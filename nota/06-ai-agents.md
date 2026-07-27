# Nota Konsep: AI Agents

> Nota latar belakang untuk SESI 11 (Hari 3). Setakat Hari 2, kita bina **RAG chain** yang tetap. Hari 3 kita naik taraf kepada **ejen** yang boleh **membuat keputusan** sendiri. Fahami perbezaannya di sini.

---

## Ejen vs *workflow* biasa

*Workflow* RAG Hari 2 ialah **aliran tetap (*fixed chain*)**: langkah **sama** setiap kali, dalam urutan **sama**.

```
WORKFLOW TETAP (RAG chain):
Soalan → Embed → Vector Search → Konteks → LLM → Jawapan
(sentiasa laluan yang sama, tak kira soalan apa)
```

**AI Agent** berbeza: ia diberi satu **matlamat** & satu set **alat (*tools*)**, kemudian **LLM sendiri** memutuskan **langkah mana** untuk diambil, alat mana untuk dipanggil, dan bila ia sudah selesai.

```
EJEN (dinamik):
Soalan → LLM fikir → "Saya perlu cari KB dulu" → panggil tool → lihat hasil
       → LLM fikir lagi → "Sekarang semak status lesen" → panggil tool lain
       → LLM fikir → "Cukup maklumat" → jana jawapan akhir
(laluan berbeza bergantung pada soalan)
```

| | **Workflow / RAG chain** | **AI Agent** |
|--|--------------------------|--------------|
| Langkah | Tetap, ditetapkan pembina | Dipilih **dinamik** oleh LLM |
| Alat | Satu laluan | Boleh pilih antara **banyak** alat |
| Boleh diramal | Tinggi ✅ | Kurang (lebih fleksibel) |
| Sesuai untuk | Soalan jenis **sama** (tanya dokumen) | Soalan **pelbagai** perlu tindakan berbeza |
| Contoh JPJ | "Apa prosedur pindah milik?" | "Semak status lesen saya **dan** buka tiket jika tamat tempoh" |

---

## Gelung *Reason–Act* (fikir–bertindak)

Teras ejen ialah gelung berulang: **fikir → bertindak → perhati → fikir lagi** — sehingga matlamat tercapai.

```
              ┌──────────────────────────────────────────┐
              │                                          │
              ▼                                          │
   ┌──────────────────┐                                  │
   │  REASON (fikir)   │  LLM tafsir keadaan:             │
   │  "Apa saya perlu  │  perlu maklumat apa?             │
   │   buat seterusnya?"│  alat mana patut dipanggil?     │
   └────────┬─────────┘                                  │
            │                                             │
            ▼                                             │
   ┌──────────────────┐                                   │
   │   ACT (bertindak) │  Panggil satu tool               │
   │  panggil tool +   │  (cth. cari KB, semak API)       │
   │   beri input      │                                  │
   └────────┬─────────┘                                   │
            │                                             │
            ▼                                             │
   ┌──────────────────┐                                   │
   │ OBSERVE (perhati) │  Terima hasil tool ─────────────►┘
   │  baca output tool │  (ulang gelung jika perlu)
   └────────┬─────────┘
            │  cukup maklumat?
            ▼
   ┌──────────────────┐
   │  FINAL ANSWER     │  Jana jawapan akhir + petikan
   └──────────────────┘
```

> Ini kadang dipanggil corak **ReAct** (*Reason + Act*). LLM tak jalankan alat sendiri — ia **mencadangkan** alat mana & input apa; n8n menjalankannya, kemudian pulangkan hasil ke LLM.

---

## Tool calling — bagaimana LLM "guna alat"

**Tool calling** ialah keupayaan LLM memilih untuk **memanggil fungsi luaran** dan bukannya menjawab terus. Setiap **tool** diberi:

- **Nama** — cth. `cari_pangkalan_pengetahuan`
- **Penerangan** — ayat yang beritahu LLM **bila** guna tool ini (sangat penting!)
- **Input** — apa yang tool perlukan (cth. teks soalan)

LLM membaca penerangan semua alat, lalu **memutuskan** alat mana relevan untuk soalan semasa.

**Contoh 3 alat Ejen Perkhidmatan JPJ (Hari 3, SESI 12):**

| Tool | Penerangan (untuk LLM) | Guna bila |
|------|------------------------|-----------|
| `cari_KB` | "Cari dokumen JPJ (SOP, akta, pekeliling) untuk maklumat prosedur." | Soalan tentang prosedur/dasar |
| `semak_status_lesen` | "Semak status semasa lesen memandu ikut no. kad pengenalan (mock API)." | Pengguna tanya status lesen mereka |
| `cipta_tiket` | "Buka tiket khidmat pelanggan untuk isu yang perlu tindakan pegawai." | Masalah tak dapat diselesaikan pembantu |

> **Penerangan tool = "prompt kecil".** Jika penerangan kabur, LLM pilih alat yang salah. Menulis penerangan tool yang jelas ialah sebahagian *prompt engineering* — lihat [`07-prompt-engineering.md`](./07-prompt-engineering.md).

---

## System prompt / peranan

**System prompt** ialah arahan tetap yang menentukan **peranan, nada & peraturan** ejen — dibaca sebelum setiap soalan. Ia "perlembagaan" ejen.

Contoh (Inggeris, amalan biasa):
```
You are "Pembantu Pintar JPJ", an assistant for Malaysian Road Transport
Department officers. Answer ONLY using the provided tools and retrieved
documents. Always cite the source document. If the information is not found,
say "tidak dijumpai dalam dokumen" — never guess. For enforcement decisions,
defer to a human officer.
```

> System prompt inilah tempat kita **pasang guardrail**: mesti petik sumber, jangan meneka, serahkan keputusan penguatkuasaan kepada manusia. Ini kritikal untuk kegunaan kerajaan ([`08-governance-keselamatan.md`](./08-governance-keselamatan.md)).

---

## Memori (memory)

LLM tak ingat perbualan lepas (rujuk [`02-apa-itu-llm.md`](./02-apa-itu-llm.md)). **Memory** menyelesaikan ini dengan **menyimpan & menyuap semula** sejarah perbualan ke ejen, jadi ia boleh faham konteks bersambung:

- Pengguna: *"Apa dokumen untuk pembaharuan lesen?"*
- Pengguna: *"Berapa **kosnya**?"* ← "nya" merujuk lesen tadi — memory yang membolehkan ejen faham.

> Dalam n8n, memory disambung sebagai komponen berasingan pada *AI Agent node*. Sesuai untuk pembantu perbualan; boleh dimatikan untuk pertanyaan sekali-sahaja.

---

## Node *AI Agent* n8n (secara konsep)

Dalam n8n, **AI Agent node** menyatukan semuanya secara visual. Anda **sambungkan**:

```
                    ┌──────────────────────┐
   Chat/Webhook ──► │     AI Agent node     │ ──► Jawapan
                    └──────────┬───────────┘
                     ┌─────────┼──────────┬───────────┐
                     ▼         ▼          ▼           ▼
                 [LLM model] [Memory]  [Tool: KB] [Tool: API]
                  OpenAI/     simpan    Qdrant     mock lesen
                  Ollama      sejarah   search     / tiket
```

Anda tak menulis gelung reason–act sendiri — **n8n & LLM menguruskannya**. Tugas anda: bekalkan model, alat, memory & system prompt yang baik.

---

## Bila guna ejen vs RAG chain tetap

| Guna **RAG chain tetap** bila... | Guna **AI Agent** bila... |
|----------------------------------|---------------------------|
| Soalan sentiasa jenis "tanya dokumen" | Soalan pelbagai, perlu tindakan berbeza |
| Mahu tingkah laku **boleh diramal** & murah | Perlu **pilih** antara banyak alat/API |
| Satu sumber (Qdrant) mencukupi | Perlu gabung KB + API + cipta rekod |
| Kawalan ketat (penguatkuasaan) | Bantuan interaktif berbilang langkah |

> **Nasihat:** mulakan dengan **RAG chain** yang mudah & boleh diramal (Hari 2). Naik taraf ke **ejen** hanya bila anda benar-benar perlukan pemilihan alat dinamik (Hari 3). Lebih banyak kuasa = lebih banyak yang perlu dikawal.

Seterusnya: [`07-prompt-engineering.md`](./07-prompt-engineering.md) — cara menulis prompt & penerangan tool yang membuatkan ejen berkelakuan betul.
