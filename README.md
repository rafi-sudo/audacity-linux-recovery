# audacity-linux-recovery
Linux troubleshooting and recovery procedures for Audacity freeze, crash, and project recovery issues.
Audacity Linux Recovery

Dokumentasi troubleshooting Audacity di Linux, khususnya kasus freeze / Not Responding saat startup atau Automatic Crash Recovery.

Environment

- OS: Debian GNU/Linux
- Desktop: XFCE
- Audacity: 3.2.4
- Architecture: x86_64

---

🚨 Emergency: Audacity Freeze

1. Stop Audacity

pkill -TERM audacity

Pastikan sudah berhenti:

pgrep audacity

Jika tidak ada output, Audacity sudah berhenti.

«Jangan langsung gunakan "kill -9" sebelum recovery project diamankan.»

---

2. Backup Recovery Project

Cek apakah recovery directory ada:

ls -lah /var/tmp/audacity-$(whoami)

Backup:

cp -a /var/tmp/audacity-$(whoami) \
      /var/tmp/audacity-$(whoami).BACKUP

Jangan hapus ".aup3unsaved" atau ".aup3unsaved-wal".

File tersebut dapat berisi data project yang belum tersimpan.

---

3. Isolate Recovery Session

Jika Audacity freeze pada pola:

Audacity startup
      ↓
Welcome
      ↓
Project Recovery
      ↓
Not Responding / crash

isolasi recovery directory:

mv /var/tmp/audacity-$(whoami) \
   /var/tmp/audacity-$(whoami).disabled

Kemudian buka Audacity kembali dari desktop XFCE.

---

✅ Verify

Jika Audacity sudah terbuka:

1. Tutup Welcome dialog.
2. Buat/open project kosong.
3. Import file audio.
4. Pastikan waveform muncul.
5. Coba edit/geser waveform.
6. Pastikan tidak freeze atau crash.

Jika semua normal, recovery session sebelumnya kemungkinan menjadi pemicu masalah pada mesin tersebut.

---

🔧 Reset Audacity Configuration

Jika Audacity tetap bermasalah setelah recovery diisolasi, backup konfigurasi:

mv ~/.config/audacity \
   ~/.config/audacity.backup-$(date +%F-%H%M%S)

Kemudian jalankan Audacity kembali.

Jangan menghapus backup konfigurasi.

---

📁 Important Recovery Locations

Recovery sementara pada sistem ini:

/var/tmp/audacity-<username>/

Contoh:

/var/tmp/audacity-nis/

Backup:

/var/tmp/audacity-nis.BACKUP/

Recovery yang diisolasi:

/var/tmp/audacity-nis.disabled/

---

⚠️ Data Safety

Jangan menjalankan:

rm -rf /var/tmp/audacity-*

sebelum memastikan project sudah aman.

Jangan menghapus:

*.aup3unsaved
*.aup3unsaved-wal

karena file tersebut mungkin diperlukan untuk recovery project.

---

🧪 Case: nis

Symptom

Audacity 3.2.4
Debian + XFCE
       ↓
Welcome dialog tidak merespons
       ↓
Enter
       ↓
Project Recovery
       ↓
Not Responding / crash

Fix yang berhasil

pkill -TERM audacity

cp -a /var/tmp/audacity-nis \
      /var/tmp/audacity-nis.BACKUP

mv /var/tmp/audacity-nis \
   /var/tmp/audacity-nis.disabled

Setelah itu Audacity dapat:

- membuka Welcome dialog secara normal
- menutup Welcome dialog
- membuat project
- meng-import MP3
- menampilkan waveform
- melakukan editing
- tidak mengalami freeze/crash pada pengujian

Kesimpulan

Pada kasus "nis", mengisolasi temporary recovery session setelah membuat backup berhasil menghilangkan freeze/crash loop.

Ini adalah workaround yang terverifikasi pada mesin tersebut, bukan klaim bahwa semua instalasi Audacity 3.2.4 akan mengalami masalah yang sama.

---

References

- Audacity Manual — Recovery
- Audacity Manual — Preferences
- Audacity GitHub Issues — Crash/Recovery reports

---

Quick Fix

Jika kejadian yang sama terulang:

pkill -TERM audacity

cp -a /var/tmp/audacity-$(whoami) \
      /var/tmp/audacity-$(whoami).BACKUP

mv /var/tmp/audacity-$(whoami) \
   /var/tmp/audacity-$(whoami).disabled

Kemudian buka Audacity dari desktop.

Backup recovery terlebih dahulu. Jangan hapus data recovery.
