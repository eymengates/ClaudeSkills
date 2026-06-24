# Skills: Meta-Prompt Builder — Membuat Prompt untuk Claude (Workflow dengan 5 Pertanyaan Kritis)

Tujuan: Dokumen ini mendeskripsikan sebuah skill penulisan prompt yang, setiap kali menerima pertanyaan atau chat pengguna, akan:
1) Menanyakan 5 pertanyaan kritis untuk memastikan pemahaman, lalu
2) Menyusun prompt final yang terstruktur untuk diproses oleh Claude,
3) Menyajikan prompt final untuk verifikasi/tweaking sebelum pengiriman.

Prinsip inti (inspirasi Karpathy)
- Iterasi cepat: tanya -> kumpulkan jawaban -> bangun prompt -> uji.
- Data-centric: minta metrik/criteria yang dapat diukur.
- Dekomposisi: pecah instruksi menjadi bagian (system, task, examples, constraints, output format).
- Automasi: simpan versi prompt dan jawaban klarifikasi untuk audit/regresi.

Bagian A — Lima Pertanyaan Kritis (diajukan sebelum membangun prompt untuk Claude)
Setiap pertanyaan disertai tujuan singkat (mengapa penting).

1) Apa tujuan akhir yang spesifik dan actionable dari permintaan ini?
   - Tujuan: memastikan hasil yang diinginkan jelas (mis. "ringkasan 3 poin", "skrip Python", "rencana 7-hari").
2) Bagaimana Anda akan menilai keberhasilan atau metrik apa yang harus dipenuhi?
   - Tujuan: menetapkan kriteria kuantitatif/kualitatif untuk validasi (mis. akurasi, panjang, format JSON valid).
3) Siapa audiens dan gaya/tingkat detail yang diinginkan?
   - Tujuan: menyesuaikan register bahasa, contoh, dan asumsi latar belakang (pemula vs ahli).
4) Adakah constraint, larangan, atau format output wajib?
   - Tujuan: memaksa batasan (mis. "maks 200 kata", "kembalikan YAML", "jangan menyertakan kode berbahaya").
5) Contoh/kontra-contoh atau konteks tambahan yang relevan?
   - Tujuan: menyediakan few-shot atau edge cases yang membantu konsistensi (konten referensi, data, atau contoh output yang baik/ buruk).

Bagian B — Proses (step-by-step usage)
1. Terima input awal pengguna (pertanyaan/chat).
2. Ajukan 5 pertanyaan kritis di atas, simpan jawaban pengguna.
3. Validasi jawaban: singkatkan dan tunjukkan ringkasan jawaban (1–2 kalimat per poin). Jika ada inkonsistensi, minta klarifikasi tambahan.
4. Bangun prompt final untuk Claude menggunakan template terstruktur (lihat Bagian C).
5. Tampilkan prompt final kepada pengguna untuk review cepat (opsional edit).
6. Setelah persetujuan, kirim prompt final ke Claude (atau beri perintah "Siap kirim ke Claude" jika pengguna ingin mengirim sendiri).
7. Simpan versi: input awal + jawaban 5 Q + prompt final + timestamp + versi.

Bagian C — Template Prompt Final untuk Claude (struktur yang dirakit dari jawaban)
- System (peran singkat): "You are Claude, an expert [role] focused on [goal]. Be concise and follow constraints."
- Context (singkat): lampirkan ringkasan konteks dari jawaban pengguna.
- Task (instruksi utama): hasil yang diinginkan, terukur, langkah terstruktur.
- Examples (opsional): 1–3 contoh positif/negatif dari jawaban Q5.
- Constraints (hard constraints): format, panjang, larangan.
- Output format (keras): contoh JSON/YAML/markdown dengan schema yang harus dipatuhi.
- Verification step: "After producing output, validate it against these criteria: [list metrik]. If validation fails, explain differences and propose fix."

Contoh Fill-in (otomatis dari jawaban 5 Q)
System: You are Claude, an expert technical writer. Goal: create a 3-point executive summary.
Context: <ringkasan input pengguna>
Task: 1) Extract 3 main points. 2) Provide one TL;DR sentence. 3) List assumptions.
Constraints: Return valid JSON: {summary, tldr, assumptions}. Max 120 words in summary.
Examples: [optional]
Verification: Ensure JSON parses and summary contains the 3 highest-impact claims.

Bagian D — Script/Mapping otomatis (bagaimana jawaban 5 Q di-mapping)
- Q1 (Tujuan) -> Task (apa yang harus dilakukan)
- Q2 (Metrik) -> Verification step & acceptance criteria
- Q3 (Audiens/Gaya) -> System tone / level of detail / examples
- Q4 (Constraints) -> Constraints section & Output format
- Q5 (Contoh/Konteks) -> Examples section + tambahan konteks

Bagian E — Example end-to-end (singkat)
Input user: "Buat ringkasan teknis dari artikel ini dan buat rekomendasi langkah implementasi."
Skill mengajukan 5 Q:
  1) Tujuan? -> "Ringkasan 3 poin + 3 langkah implementasi prioritas"
  2) Metrik? -> "Panjang summary ≤150 kata; steps actionable"
  3) Audiens? -> "Tim engineering senior"
  4) Constraint? -> "Output JSON, keys: summary, steps; jangan sertakan opini tak berdasar"
  5) Contoh? -> "Lampirkan artikel + referensi API"
Skill menyusun prompt final sesuai template, menampilkan ke user, dan setelah approval dikirim ke Claude.

Bagian F — Checklist cepat sebelum mengirim ke Claude
- Sudah ada jawaban 5 Q? (ya/tidak)
- Ringkasan jawaban singkat ditampilkan & disetujui?
- Output format ditentukan dan contoh valid disertakan?
- Metrik verifikasi jelas?
- Tidak ada konflik constraints?

Bagian G — Logging, Versioning, dan Evaluasi
- Simpan paket: {input_awal, jawaban_5Q, prompt_final, response_Claude, metrik_result}.
- Versi prompt: vYYYYMMDD-<shortdesc>.
- Setelah eksekusi: jalankan verification automatic (parse JSON, cek panjang, cek metrik). Laporkan pass/fail.
- Jika gagal: buat daftar perbaikan dan jalankan iterasi.

Bagian H — Tips & Guardrails
- Jika jawaban Q2 tidak bisa diukur, ubah menjadi pertanyaan follow-up konkret.
- Untuk tugas berisiko (medis, hukum, keselamatan), selalu tambahkan step verifikasi manusia sebelum tindakan.
- Jangan kirim data sensitif tanpa redaksi; tambahkan langkah redaksi otomatis bila perlu.

Penutup — Template pesan untuk dijalankan oleh skill
- Saat menerima chat pengguna, skill harus memulai dialog:
  "Sebelum saya susun prompt untuk Claude, saya perlu 5 jawaban singkat untuk memastikan hasil tepat. Tolong jawab: 1) Tujuan akhir? 2) Kriteria keberhasilan? 3) Audiens & gaya? 4) Constraint/format wajib? 5) Contoh atau konteks tambahan?"
- Setelah mendapat jawaban, skill merakit prompt final sesuai Bagian C dan menampilkannya untuk persetujuan.

---
Versi dokumen ini: v20260624-meta-prompt
