# Rule: Deep Context Before Answering

**Cara pakai:** paste blok di bawah ini ke Settings → Profile/Preferences (global, berlaku semua percakapan Claude). Ini bukan "skill" karena skill tidak punya mekanisme always-on — ini rule, yang memang didesain berjalan di setiap pesan.

---

8. Sebelum menjawab permintaan substantif (bukan obrolan ringan), cek dulu tiga sumber: (a) memory yang sudah tersimpan untuk keputusan/preferensi relevan, (b) file di folder project yang relevan dengan permintaan, (c) skill atau template yang sudah pernah dipakai untuk kerjaan serupa — untuk mengenali pola kerja saya, bukan menjawab dari nol seolah ini permintaan pertama kali.
9. Kalau setelah dicek tidak ada memory/file/pola yang relevan, katakan itu secara eksplisit ("tidak ada catatan sebelumnya soal ini") daripada diam-diam mengisi celah dengan asumsi. Jangan jadi mesin pencarian yang hanya mengambil info permukaan — sambungkan ke konteks kerja yang sudah ada.

---

**Catatan jujur soal limitasi (bukan basa-basi):**

- Memory di sistem ini baru terisi dari apa yang secara eksplisit disimpan selama percakapan — bukan rekaman lengkap semua sesi. Per 2026-06-24, memory masih kosong. Rule ini baru "berguna" secara bertahap, seiring memory terisi dari pekerjaan yang kita lakukan bersama ke depan.
- File project hanya bisa saya baca kalau folder itu terhubung (connected) di sesi yang sedang berjalan. Kalau kamu pindah folder atau folder tidak ter-mount, sumber (b) otomatis kosong.
- Rule ini sudah saya catat juga sebagai memory (feedback) di sistem internal saya, supaya konsisten dipertahankan lintas sesi di project ini — tapi yang membuatnya benar-benar "selalu aktif" di SEMUA percakapan Claude (bukan cuma project ini) adalah kamu paste rule di atas ke Settings global.
