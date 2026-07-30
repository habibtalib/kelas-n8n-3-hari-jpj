# Mock API JPJ (Data Sintetik) 🔌

> ⚠️ **Data SINTETIK untuk latihan RAG-N8N-JPJ-101** — bukan rekod JPJ sebenar. Statik (GET sahaja), dihidang melalui GitHub raw. Sesuai untuk uji node **HTTP Request** & tool ejen (Hari 1 & Hari 3).

## Endpoint (GET)

**Semak Status Lesen** — ikut No. KP:
```
https://raw.githubusercontent.com/habibtalib/kelas-n8n-3-hari-jpj/main/mock-api/lesen/{ic}.json
```
Contoh: [`https://raw.githubusercontent.com/habibtalib/kelas-n8n-3-hari-jpj/main/mock-api/lesen/900101-14-5566.json`](https://raw.githubusercontent.com/habibtalib/kelas-n8n-3-hari-jpj/main/mock-api/lesen/900101-14-5566.json)

**Senarai semua lesen:** [`https://raw.githubusercontent.com/habibtalib/kelas-n8n-3-hari-jpj/main/mock-api/lesen.json`](https://raw.githubusercontent.com/habibtalib/kelas-n8n-3-hari-jpj/main/mock-api/lesen.json)

**Maklumat Pemohon:** `https://raw.githubusercontent.com/habibtalib/kelas-n8n-3-hari-jpj/main/mock-api/pemohon/{ic}.json`

**Saman/Kompaun:** `https://raw.githubusercontent.com/habibtalib/kelas-n8n-3-hari-jpj/main/mock-api/saman/{ic}.json`

## No. KP contoh yang boleh diguna
| No. KP | Nama | Lesen | Status | Saman |
|--------|------|-------|--------|-------|
| `900101-14-5566` | Ahmad bin Ali | GDL | Aktif | 0 |
| `880202-10-1234` | Siti binti Kassim | CDL | Tamat Tempoh | 2 |
| `950303-08-7788` | Lim Wei Jie | P | Aktif | 0 |
| `920505-05-1122` | Nurul Aina binti Zainal | D | Aktif | 0 |
| `850707-06-3344` | Rajesh a/l Kumar | GDL | Digantung | 1 |
| `000909-14-9900` | Muhammad Danial bin Hakim | LDL | Aktif | 0 |
| `880202-10-1234` | Siti binti Kassim | CDL | Tamat Tempoh | 2 |
| `950303-08-7788` | Lim Wei Jie | P | Aktif | 0 |

## Guna dalam n8n (node HTTP Request)
1. Tambah node **HTTP Request**.
2. **Method:** `GET`
3. **URL:** `https://raw.githubusercontent.com/habibtalib/kelas-n8n-3-hari-jpj/main/mock-api/lesen/900101-14-5566.json`
   *(atau dinamik: `https://raw.githubusercontent.com/habibtalib/kelas-n8n-3-hari-jpj/main/mock-api/lesen/{{ $json.ic }}.json`)*
4. **Execute** → dapat JSON status lesen.

## Guna sebagai tool ejen (Hari 3 — Semak Status Lesen)
Pada **HTTP Request Tool** ejen:
- **URL:** `=https://raw.githubusercontent.com/habibtalib/kelas-n8n-3-hari-jpj/main/mock-api/lesen/{{ $fromAI('ic','No. KP pemohon','string') }}.json`
- Ejen isi No. KP dari soalan pengguna → panggil endpoint → jawab status lesen.

> Contoh respons `lesen/900101-14-5566.json`:
> ```json
> { "ic": "900101-14-5566", "nama": "Ahmad bin Ali", "licence_type": "GDL", "status": "Aktif", "expiry": "2027-03-31", "outstanding_summons": 0 }
> ```
