**Mini-Case: Monday Morning Go-Live**  
2314000028 \- Fadilla Suri Rinaldo   
2314000008 \- Ariq Hammam Naufal  
2414000025 \- Celesta Rio Daffa Daniswara  
   
   
**1\.**  	Tiga Risiko Terbesar (Three Biggest Risks)  
A.	Risiko Infrastruktur: Kegagalan Performa dan Server Tumbang (*Peak Load Crash*)

* Detail Masalah: Mengoperasikan server yang belum diuji (*untested*) untuk melayani 5.000 pengguna potensial menciptakan ancaman kegagalan total (*bottleneck* pada CPU/RAM/Database) saat sistem dibuka serentak di hari Senin.  
* Dampak: Pengguna mengalami *timeout*, pendaftaran gagal terproses, serta reputasi institusi menurun akibat layanan yang tidak dapat diakses.  
  B. 	Risiko Integritas Data: Korupsi atau Ketidakcocokan Data Migrasi (*Data Integrity Failure*)  
* Detail Masalah: Pemindahan data dari sistem lama (*legacy*) ke struktur basis data baru tanpa proses verifikasi berjenjang berisiko menyebabkan data hilang, duplikat, atau format data tidak valid.  
* Dampak: Siswa tidak dapat *login*, riwayat data akademik hilang, dan admin terpaksa melakukan perbaikan data secara manual di tengah operasional yang berjalan.  
  C. 	Risiko Operasional: Keterbatasan Waktu Peluncuran & Tingginya *Human Error*  
* Detail Masalah: Jendela *downtime* 30 menit sangat mepet untuk eksekusi skrip migrasi. Ditambah lagi, pelatihan admin selama 2 jam tidak cukup untuk membangun kemahiran dalam menangani kasus khusus (*edge cases*).  
* Dampak: Proses *go-live* berpotensi melampaui alokasi *downtime*, serta admin akan kewalahan (*overwhelmed*) menangani keluhan pengguna saat sistem aktif.  
     
  **2\.**  	Persyaratan Minimum Go-Live (Minimum Go-Live Requirements)  
  Untuk meminimalkan risiko di atas, peluncuran hanya boleh dilakukan jika seluruh kriteria ambang batas minimum berikut terpenuhi:

  A.    Infrastruktur & Beban Sistem:  
  ·        Server telah lulus simulasi *smoke test* dan *stress test* minimum dengan target *concurrent users* puncak.  
  ·        *Auto-scaling* atau alokasi resource cadangan telah disiapkan di lingkungan produksi.  
  B.     Integritas Data:  
  ·        Skrip migrasi data telah berhasil diuji coba (*dry-run*) di lingkungan *staging* dengan tingkat keberhasilan 100% pada data identitas inti.  
  C.     Fungsionalitas Aplikasi:  
  ·        Seluruh alur kritis (Login, Input Registrasi, Simpan Data, dan Cetak Bukti/Konfirmasi) berfungsi tanpa *bug* tingkat *Critical* atau *Blocker*.  
  D.    Kesiapan Prosedur *Rollback*:  
  ·        Tersedia skrip otomatis untuk mengembalikan (*restore*) sistem dan data ke kondisi semula jika terjadi kegagalan dalam jendela waktu 30 menit.  
  E.     Kesiapan Operasional & *Support*:  
  ·        Admin telah memahami prosedur dasar operasional dan eskalasi masalah.  
  ·        Tersedia *SOP Helpdesk* untuk menangani keluhan siswa saat hari peluncuran.

     
    
  **3\.**  	Bukti yang Harus Tersedia Sebelum Launching (Mandatory Pre-Launch Evidence)

  Sebelum tombol peluncuran ditekan, tim pengembang dan operasional wajib menyerahkan dokumen/bukti fisik berikut kepada *Project Manager* atau *Stakeholder*:

| Kategori Bukti | Nama Dokumen / Bukti | Deskripsi & Tolok Ukur |
| :---- | :---- | :---- |
| Kinerja Server | *Load & Stress Test Benchmark Report* | Hasil uji yang menunjukkan *response time* $\< 3$ detik pada tingkat beban puncak. |
| Integritas Data | *Data Reconciliation & Audit Log* | Berita acara yang membuktikan jumlah baris data (*row count*) dan checksum data lama setara dengan data baru. |
| Kualitas Aplikasi | *UAT Sign-Off & Defect Matrix* | Tanda tangan persetujuan dari perwakilan pengguna (*UAT*) yang menyatakan 0 *Blocker Bug*. |
| Operasional | *30-Minute Deployment Timeline & Rollback Plan* | Dokumen menit-demi-menit pelaksanaan peluncuran beserta simulasi skenario *rollback*. |
| SDM | *Admin Training Checklist & Escalation Matrix* | Daftar hadir pelatihan admin beserta struktur tim pendamping (*on-call engineer*) saat peluncuran. |

   

  **4\.**  	Kriteria Pengambilan Keputusan NO-GO

  Keputusan NO-GO (pembatalan/penundaan peluncuran) wajib diambil secara tegas jika salah satu dari kondisi berikut terjadi selama persiapan atau di dalam jendela *downtime* 30 menit:

 

\[Mulai Jendela Downtime 30 Menit\]  
        │  
        ├──► Migrasi Data Gagal / Data Corrupt? ─────────────► \[NO-GO\]  
        │  
        ├──► Ditemukan Celah Keamanan / Bug Blocker? ─────────► \[NO-GO\]  
        │  
        ├──► Waktu Eksekusi Melebihi 20 Menit Tanpa Kejelasan? ► \[NO-GO\]  
        │  
        └──► Semua Pengujian Akhir Lulus (OK) ────────────────► \[GO\]

1. Gagal Migrasi Data Utama: Jika proses migrasi data mengalami eror fatal atau data siswa tidak cocok dan tidak dapat diperbaiki dalam kurun waktu 15 menit pertama.  
2. Ketersediaan Server Tidak Stabil: Jika server mengalami *crash* atau *unhandled exception* saat dilakukan verifikasi akhir (*final sanity check*) di lingkungan produksi.  
3. Terdeteksi Celah Keamanan (*Security Breach*): Ditemukannya celah keamanan kritis (seperti kebocoran data pribadi siswa atau akses tanpa otentikasi) pada verifikasi akhir.  
4. Batas Waktu *Downtime* Terlampaui (*Time-out*): Jika pada menit ke-20 proses deployment belum mencapai tahap penyelesaian 80%, keputusan NO-GO harus diambil agar tersisa 10 menit untuk melakukan *rollback* ke sistem lama.  
5. Ketidaksiapan Tim Penanggung Jawab: Apabila personel kunci (seperti *Lead Database Administrator* atau *DevOps Engineer*) tidak hadir atau tidak dapat merespons saat jendela peluncuran berlangsung.

 

