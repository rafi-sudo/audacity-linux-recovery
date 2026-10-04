Audacity Freeze — Recovery & Cleanup Runbook (Linux)

Dokumentasi penanganan Audacity freeze / Not Responding, terutama ketika freeze terjadi saat startup atau Automatic Crash Recovery.

«⚠️ PENTING: Jangan langsung menghapus ".aup3unsaved" atau ".aup3unsaved-wal". File tersebut dapat berisi data project yang belum tersimpan.»

---

1. Hentikan Audacity Secara Normal

Dari Terminal:

pkill -TERM audacity

Periksa apakah Audacity masih berjalan:

pgrep audacity

Jika tidak ada output, Audacity sudah berhenti.

«Catatan: Jangan langsung menggunakan "kill -9" apabila masih ada project recovery yang perlu diselamatkan.»

---

2. Backup Recovery Terlebih Dahulu

Pada Linux, temporary recovery Audacity biasanya berada di:

/var/tmp/audacity-

Untuk user "nis":

cp -a /var/tmp/audacity-nis /var/tmp/audacity-nis.BACKUP

Pastikan command selesai tanpa error.

Backup ini jangan dihapus sampai project yang dibutuhkan sudah berhasil diselamatkan.

---

3. Jika Freeze Terjadi Karena Automatic Crash Recovery

Jika pola masalahnya:

Audacity startup
      ↓
Welcome
      ↓
Project Recovery
      ↓
Not Responding / crash

Isolasi temporary recovery tanpa menghapusnya:

mv /var/tmp/audacity-nis /var/tmp/audacity-nis.disabled

Kemudian buka Audacity kembali dari desktop/XFCE.

Hasil yang Diharapkan

Jika Audacity sekarang:

- bisa membuka window
- Welcome dialog bisa ditutup
- project kosong bisa dibuat
- audio bisa di-import
- waveform bisa diedit

maka recovery session sebelumnya adalah tersangka utama.

Jangan hapus:

/var/tmp/audacity-nis.BACKUP
/var/tmp/audacity-nis.disabled

---

4. Jangan Langsung Menganggap Project Recovery Rusak

Recovery masih dapat dicoba secara manual.

Audacity menggunakan file seperti:

.aup3unsaved
.aup3unsaved-wal

untuk project yang belum tersimpan.

Setelah Audacity sudah stabil, recovery dapat dilakukan dari salinan backup, bukan dari data asli.

«Dokumentasi resmi Audacity menyarankan menyalin seluruh temporary recovery folder sebelum melakukan recovery.»

---

5. Jika Konfigurasi Audacity Juga Dicurigai

Backup konfigurasi terlebih dahulu:

mv ~/.config/audacity ~/.config/audacity.backup-$(date +%F-%H%M%S)

Kemudian jalankan Audacity kembali.

Reset Preferences merupakan salah satu langkah troubleshooting untuk kasus freeze, crash, atau perilaku Audacity yang tidak normal.

«Catatan: Lokasi konfigurasi dapat berbeda menurut versi/package Audacity. Dokumentasi Audacity saat ini mendokumentasikan "audacity-data" / "audacity.cfg" untuk Linux. Periksa lokasi konfigurasi versi/package yang digunakan sebelum menghapus file secara manual.»

---

Emergency Procedure — Recovery Aman

Jika produser sudah menunggu dan Audacity freeze:

pkill -TERM audacity
pgrep audacity

Jika Audacity sudah berhenti:

cp -a /var/tmp/audacity-nis /var/tmp/audacity-nis.BACKUP

Kemudian isolasi recovery:

mv /var/tmp/audacity-nis /var/tmp/audacity-nis.disabled

Buka Audacity kembali.

Jika normal:

Audacity berhasil dibuka
        ↓
Recovery loop berhenti
        ↓
Project/audio dapat dikerjakan

«Jangan hapus backup recovery sebelum memastikan project lama tidak diperlukan.»

---

6. Kasus yang Terverifikasi pada Mesin "nis"

Environment

Audacity 3.2.4
Debian GNU/Linux
XFCE

Gejala

Welcome dialog tidak merespons
        ↓
Enter
        ↓
Project Recovery muncul
        ↓
Audacity Not Responding / crash

Tindakan

pkill -TERM audacity

Backup recovery:

cp -a /var/tmp/audacity-nis /var/tmp/audacity-nis.BACKUP

Backup konfigurasi Audacity.

Kemudian rename recovery directory:

mv /var/tmp/audacity-nis /var/tmp/audacity-nis.disabled

Start Audacity kembali.

Hasil

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

Kesimpulan Kasus

Workaround yang berhasil:

mv /var/tmp/audacity-nis /var/tmp/audacity-nis.disabled

setelah recovery directory dibackup terlebih dahulu.

«Ini bukan berarti sudah terbukti sebagai bug universal Audacity 3.2.4. Ini adalah workaround yang terverifikasi pada mesin tersebut.»

---

7. Aturan Keselamatan Data

Jangan lakukan

rm -rf /var/tmp/audacity-nis

sebelum recovery project dipastikan tidak diperlukan.

Jangan pula menghapus secara membabi buta:

*.aup3unsaved
*.aup3unsaved-wal

Automatic Crash Recovery dibuat untuk mempertahankan pekerjaan setelah crash.

Data yang dibuang dari recovery dapat menjadi tidak dapat dipulihkan.

---

8. Setelah Pekerjaan Aman

Setelah project berhasil disimpan sebagai:

.aup3

dan backup recovery sudah tidak diperlukan, recovery sementara dapat dibersihkan.

Sebelum cleanup, pastikan:

[ ] Project sudah tersimpan
[ ] Audio hasil edit sudah benar
[ ] Export final sudah tersedia
[ ] Tidak ada pekerjaan yang hanya berada di .aup3unsaved
[ ] Recovery backup tidak lagi diperlukan

Baru kemudian lakukan cleanup.

---

9. FULL CLEANUP — Hapus Recovery & Reset Konfigurasi

«⚠️ DESTRUCTIVE PROCEDURE

Gunakan bagian ini hanya jika recovery lama sudah tidak diperlukan dan memang ingin membersihkan Audacity sampai kondisi fresh.

Prosedur ini tidak membuat backup dan dapat menghapus data recovery yang masih diperlukan.»

9.1 Matikan Audacity dan Hapus Recovery

pkill -9 audacity
rm -rf /var/tmp/audacity-$(whoami)*

Verifikasi

Jalankan:

ls /var/tmp/audacity-$(whoami)

Jika muncul:

No such file or directory

maka directory recovery tersebut sudah tidak ada.

«⚠️ Perhatian: "rm -rf" bersifat destruktif. Pastikan tidak ada project yang masih hanya tersimpan di temporary recovery.»

---

9.2 Reset Total Konfigurasi Audacity

Hapus konfigurasi:

rm -rf ~/.config/audacity

Verifikasi

ls ~/.config/audacity

Jika muncul:

No such file or directory

maka directory konfigurasi tersebut sudah terhapus.

«Lokasi konfigurasi dapat berbeda tergantung versi/package Audacity. Gunakan langkah ini hanya jika "~/.config/audacity" memang merupakan konfigurasi Audacity yang digunakan pada sistem tersebut.»

---

9.3 Jalankan Audacity Kembali

audacity

Hasil yang Diharapkan

Audacity akan berjalan dengan konfigurasi yang telah di-reset.

Tidak ada lagi:

Temporary Recovery
        ↓
Project Recovery
        ↓
Freeze / Not Responding

Audacity seharusnya dapat dimulai seperti konfigurasi baru.

---

10. Quick Reference

Recovery Aman

pkill -TERM audacity
cp -a /var/tmp/audacity-nis /var/tmp/audacity-nis.BACKUP
mv /var/tmp/audacity-nis /var/tmp/audacity-nis.disabled

Reset Konfigurasi dengan Backup

mv ~/.config/audacity ~/.config/audacity.backup-$(date +%F-%H%M%S)

Full Cleanup — Tanpa Backup

pkill -9 audacity
rm -rf /var/tmp/audacity-$(whoami)*
rm -rf ~/.config/audacity
audacity

---

Decision Flow

Audacity freeze
      │
      ▼
Masih ada recovery penting?
      │
 ┌────┴────┐
 │         │
YA        TIDAK
 │         │
 ▼         ▼
BACKUP    FULL CLEANUP
 │         │
 ▼         ▼
Isolasi   Hapus recovery
recovery  Reset config
 │         │
 ▼         ▼
Test      Start Audacity
Audacity
 │
 ▼
Recovery manual jika diperlukan

---

Prinsip Utama

RECOVERY PENTING?
      │
      ├── YA → BACKUP → ISOLASI → RECOVERY MANUAL
      │
      └── TIDAK → CLEANUP TOTAL → RESET CONFIG → START FRESH

Jangan mengorbankan data recovery hanya untuk memperbaiki freeze. Pastikan pekerjaan sudah aman terlebih dahulu sebelum menggunakan prosedur destructive cleanup.
