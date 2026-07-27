# Buku Rujukan Utama 📖

Kursus **RAG-N8N-JPJ-101** menggunakan buku berikut sebagai **rujukan latar belakang teori** (konsep AI/LLM/RAG/agent). Nota kursus dalam repo ini adalah versi **ringkas, Bahasa Melayu, berorientasikan hands-on n8n**; buku ini pula memberi **kedalaman teori & konteks pengeluaran** untuk peserta yang mahu mendalami.

> **Vasyl Zvarydchuk, Ph.D. — _Building Agent-Powered Applications: Your guide to generative AI, RAG, fine-tuning, and orchestration for production use_.** Packt Publishing, © 2026.

> ⚠️ **Hak cipta:** Buku ini berhak cipta (Packt). Nota kursus **tidak menyalin** teks buku — kita hanya **memetik & memetakan bab** sebagai bacaan lanjut. Peserta perlu memiliki salinan sendiri untuk membacanya.

---

## Kenapa buku ini sesuai

Struktur buku hampir **sepadan satu-satu** dengan aturcara 3 hari kursus: Bahagian 1 (asas AI/NLP/LLM) → **Hari 1**, Bahagian 2 (aplikasi LLM & **RAG**) → **Hari 1–2**, Bahagian 3 (**AI Agents** & penilaian) → **Hari 3**. Ia juga menekankan **penggunaan pengeluaran** — selari dengan fokus *deployment* & tadbir urus kursus untuk JPJ.

## Pemetaan Bab Buku → Sesi Kursus

| Hari / Sesi | Topik kursus | Bab buku (bacaan lanjut) |
|-------------|--------------|---------------------------|
| **Hari 1** · SESI 1 | AI, ML vs GenAI | **Bab 1** — AI & NLP Fundamentals (ML basics) |
| **Hari 1** · SESI 2 | LLM, token, context window, halusinasi | **Bab 2** — Understanding LLMs (transformers, context window, sampling, hallucinations, LLM families: GPT/Gemini/LLaMA/Claude) |
| **Hari 1** · SESI 2 | Prompt engineering | **Bab 3** — Prompt Engineering (system/user prompt, n-shot, chain-of-thought) |
| **Hari 1** · SESI 3 | Pengenalan RAG | **Bab 6** — Retrieval-Augmented Generation (komponen RAG) |
| **Hari 1** · SESI 4 | Embeddings, cosine similarity, vector DB | **Bab 1** — Embeddings & similarity metrics (cosine/Euclidean); **Bab 6** — semantic retrieval |
| **Hari 2** · SESI 6 | RAG architecture deep dive | **Bab 6** — RAG components (static retrieval, generator, semantic retrieval) |
| **Hari 2** · SESI 7–8 | Vector DB, chunking, metadata | **Bab 6** — document preprocessing, select document length, embedding models, ANN, vector databases |
| **Hari 2** · SESI 9–10 | Ingestion & retrieval workflow | **Bab 6** — RAG implementations (custom pipeline, semantic retriever, hybrid, RAG as a service) |
| **Hari 3** · SESI 11 | AI Agents, agent vs workflow | **Bab 8** — Architecture of AI Agents (agent basics, LLM agent architecture) |
| **Hari 3** · SESI 12 | Tool calling, ejen berbilang alat, memori | **Bab 8** — memory, tools & orchestration, multi-agent flows; **Bab 9** — function calling (OpenAI/LangChain), Model Context Protocol, planning, human-in-the-loop |
| **Hari 3** · SESI 13 | RAG lanjutan (hybrid, BM25, re-rank, ANN) | **Bab 6** — lexical retrieval (TF-IDF, BM25, inverted indexes), hybrid approach, ANN methods |
| **Hari 3** · SESI 14 | Penilaian & pengoptimuman RAG | **Bab 10** — Evaluating LLM Applications & Agents (RAG eval: retriever & generator, LLM-as-judge, eval datasets) |
| **Hari 3** · SESI 15 | Deployment, keselamatan, tadbir urus | **Bab 9** — deployment & operations; **Bab 10** — Responsible AI (safety, bias, jailbreak eval) |
| *(Bonus)* | Prompting vs RAG vs fine-tuning | **Bab 7** — LLM Fine-Tuning (LoRA/PEFT, RLHF/DPO, bila fine-tune vs RAG) |

## Pemetaan Bab → Nota Konsep

| Nota | Bab buku berkaitan |
|------|--------------------|
| [`02-apa-itu-llm.md`](./02-apa-itu-llm.md) | Bab 1 (ML), Bab 2 (LLM) |
| [`03-apa-itu-rag.md`](./03-apa-itu-rag.md) | Bab 6 (RAG); Bab 7 (RAG vs fine-tuning) |
| [`04-embeddings-vector-db.md`](./04-embeddings-vector-db.md) | Bab 1 (embeddings, similarity), Bab 6 (semantic retrieval, ANN, vector DB) |
| [`06-ai-agents.md`](./06-ai-agents.md) | Bab 8 (arkitektur agent), Bab 9 (bina agent, tool calling, MCP) |
| [`07-prompt-engineering.md`](./07-prompt-engineering.md) | Bab 3 (prompt engineering) |
| [`08-governance-keselamatan.md`](./08-governance-keselamatan.md) | Bab 10 (Responsible AI, safety/bias/jailbreak) |
| [`09-deployment.md`](./09-deployment.md) | Bab 9 (deployment & operations) |

## Cara guna buku ini dalam kursus

- **Peserta:** guna nota BM + lab dalam repo sebagai *panduan hands-on*; rujuk bab buku yang dipetakan di atas untuk **kedalaman teori** (cth. kenapa BM25, bagaimana ANN, atau strategi memori agent).
- **Penceramah/jurulatih:** gunakan buku sebagai sumber *talking points* & jawapan soalan lanjutan. Selaraskan **istilah** dengan buku (English) supaya konsisten.
- **Nota "📖 Bacaan lanjut"** dalam nota konsep & README harian menunjuk terus ke bab yang berkaitan.

> **Ingat:** Buku ini bersifat **umum (produksi AI)**, bukan khusus n8n atau JPJ. Repo kursus inilah yang **menterjemahkan** konsep buku kepada **n8n visual + konteks JPJ + Bahasa Melayu**.
