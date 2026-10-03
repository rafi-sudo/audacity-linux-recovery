Audacity Freeze — Recovery Runbook (Linux)

Dokumentasi penanganan Audacity yang freeze / Not Responding, terutama ketika freeze terjadi pada saat startup atau Automatic Crash Recovery.

«PENTING: Jangan langsung menghapus ".aup3unsaved" atau ".aup3unsaved-wal". File tersebut dapat berisi data project yang belum tersimpan.»

1. Hentikan Audacity secara normal

Dari terminal:

pkill -TERM audacity

Periksa:

pgrep audacity

Jika tidak ada output, Audacity sudah berhenti.

Jangan langsung menggunakan "kill -9" apabila masih ada project recovery yang perlu diselamatkan.

---

2. Backup recovery terlebih dahulu

Pada Linux, temporary recovery Audacity biasanya berada di:

/var/tmp/audacity-<username>

Untuk user "nis":

cp -a /var/tmp/audacity-nis /var/tmp/audacity-nis.BACKUP

Pastikan command selesai tanpa error.

Backup ini jangan dihapus sampai project yang dibutuhkan sudah berhasil diselamatkan.

---

3. Jika Audacity freeze karena Automatic Crash Recovery

Jika pola masalahnya:

Audacity startup
    ↓
Welcome
    ↓
Project Recovery
    ↓
Not Responding / crash

isolasi temporary recovery tanpa menghapusnya:

mv /var/tmp/audacity-nis /var/tmp/audacity-nis.disabled

Kemudian buka Audacity kembali dari desktop/XFCE.

Hasil yang diharapkan

Jika Audacity sekarang:

- bisa membuka window,
- Welcome dialog bisa ditutup,
- project kosong bisa dibuat,
- audio bisa di-import,
- waveform bisa diedit,

maka recovery session sebelumnya adalah tersangka utama.

Jangan hapus:

/var/tmp/audacity-nis.BACKUP
/var/tmp/audacity-nis.disabled

---

4. Jangan langsung menganggap project recovery rusak

Recovery masih bisa dicoba secara manual.

Audacity menggunakan file:

.aup3unsaved
.aup3unsaved-wal

untuk project yang belum tersimpan.

Setelah Audacity sudah stabil, recovery dapat dilakukan dari salinan backup, bukan dari data asli.

Dokumentasi resmi Audacity menyarankan menyalin seluruh temporary recovery folder sebelum melakukan recovery.

---

5. Jika konfigurasi Audacity juga dicurigai

Reset konfigurasi Audacity.

Backup konfigurasi terlebih dahulu:

mv ~/.config/audacity ~/.config/audacity.backup-$(date +%F-%H%M%S)

Kemudian jalankan Audacity kembali.

Reset Preferences memang merupakan langkah troubleshooting resmi Audacity untuk kasus freeze, crash, atau perilaku yang tidak normal.

«Catatan: lokasi konfigurasi dapat berbeda menurut versi/package Audacity. Dokumentasi Audacity saat ini mendokumentasikan "audacity-data"/"audacity.cfg" untuk Linux, jadi cek lokasi konfigurasi versi yang digunakan sebelum menghapus file secara manual.»

---

Emergency Procedure — Versi Singkat

Jika produser sudah menunggu dan Audacity freeze:

pkill -TERM audacity
pgrep audacity

Jika sudah berhenti:

cp -a /var/tmp/audacity-nis /var/tmp/audacity-nis.BACKUP

Kemudian:

mv /var/tmp/audacity-nis /var/tmp/audacity-nis.disabled

Buka Audacity kembali.

Jika normal → lanjutkan pekerjaan menggunakan project/audio yang sudah tersimpan.

Jangan hapus backup recovery.

---

Apa yang dilakukan pada kasus "nis"

Kasus yang berhasil diperbaiki:

Audacity 3.2.4
Debian GNU/Linux
XFCE

Gejala:

Welcome dialog tidak merespons
        ↓
Enter
        ↓
Project Recovery muncul
        ↓
Audacity Not Responding / crash

Tindakan:

1. pkill -TERM audacity
2. Backup /var/tmp/audacity-nis
3. Backup konfigurasi Audacity
4. Rename recovery directory menjadi .disabled
5. Start Audacity kembali

Hasil:

Welcome → OK berhasil
        ↓
Audacity terbuka
        ↓
MP3 berhasil di-import
        ↓
Waveform tampil
        ↓
Editing berjalan normal
        ↓
Tidak freeze / crash

Kesimpulan kasus

Workaround yang berhasil adalah:

mv /var/tmp/audacity-nis /var/tmp/audacity-nis.disabled

setelah recovery directory dibackup terlebih dahulu.

Ini bukan berarti sudah terbukti sebagai bug universal Audacity 3.2.4. Ini adalah workaround yang terverifikasi pada mesin tersebut.

---

Aturan keselamatan data

Jangan lakukan:

rm -rf /var/tmp/audacity-nis

sebelum recovery project dipastikan tidak diperlukan.

Jangan pula menghapus:

*.aup3unsaved
*.aup3unsaved-wal

secara membabi buta.

Automatic Crash Recovery memang dibuat untuk mempertahankan pekerjaan setelah crash, dan Audacity memperingatkan bahwa data yang dibuang dari recovery dapat menjadi tidak dapat dipulihkan.

---

Setelah pekerjaan aman

Setelah project sudah berhasil disimpan sebagai ".aup3", backup recovery yang tidak diperlukan lagi dapat dibersihkan.

Sebelum membersihkan, pastikan:

[ ] Project sudah tersimpan
[ ] Audio hasil edit sudah benar
[ ] Export final sudah tersedia
[ ] Tidak ada pekerjaan yang hanya berada di .aup3unsaved

Baru kemudian lakukan cleanup.
