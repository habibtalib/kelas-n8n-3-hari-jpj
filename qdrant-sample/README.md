# Sample Qdrant Collection — JPJ (Data Sintetik) 🟣

> Koleksi contoh untuk **Tutorial Qdrant** (nota/12). Vektor **4-dimensi (toy)** + payload FAQ JPJ. ⚠️ Sintetik. Untuk RAG penuh (n8n), guna vektor **1536-dim OpenAI** melalui *ingestion* — koleksi ini untuk **belajar mekanik Qdrant**, bukan carian OpenAI.

## Muat naik (2 arahan)
```bash
# 1) Cipta collection (4-dim, Cosine)
curl -X PUT http://localhost:6333/collections/jpj_sample \
  -H 'Content-Type: application/json' \
  -d '{"vectors":{"size":4,"distance":"Cosine"}}'

# 2) Muat naik points
curl -X PUT http://localhost:6333/collections/jpj_sample/points \
  -H 'Content-Type: application/json' \
  --data @jpj-sample-points.json
```

## Cuba carian (arah "lesen")
```bash
curl -X POST http://localhost:6333/collections/jpj_sample/points/search \
  -H 'Content-Type: application/json' \
  -d '{"vector":[0.9,0.1,0.0,0.1],"limit":3,"with_payload":true}'
```
→ patut pulangkan 3 point topik **lesen** dengan skor tertinggi.

## Untuk RAG sebenar (1536-dim, boleh cari dengan OpenAI)
Jalankan *ingestion workflow* (H2·01 / H2·06 / H2·07) sekali dengan credential OpenAI → collection `jpj_documents` terisi. Itu koleksi yang digunakan oleh workflow retrieval/agent.
