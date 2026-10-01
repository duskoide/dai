# Tubes 1 IF3070 — Ringkasan Spesifikasi (Versi Detail, Mudah Dipahami)

Ringkasan tidak resmi dari `Spesifikasi Tugas Besar 1 IF3070 Dasar Inteligensi Artifisial 2026_2027.pdf`.
Bila bertentangan dengan PDF/spesifikasi asli, spesifikasi asli yang berlaku.

Catatan kecil: teks spesifikasi kadang menulis "truk" / "peti kemas" (sisa template lama). Yang dimaksud di tugas ini tetap **kapal** dan **geladak**.

## 1. Inti masalah dalam satu kalimat
Susun kendaraan di geladak kapal (grid 2D) supaya **total tarif pengiriman (ShippingFee) yang didapat sebesar mungkin**.

## 2. Cerita & peran
Kamu adalah Forward Deployed Engineer di PT Owen Intim Line, perusahaan kapal kargo pengangkut kendaraan. Perusahaan mendapat kontrak dari beberapa dealer sekaligus; tiap jadwal keberangkatan ada daftar kendaraan dengan dimensi, berat, tarif, dan deadline berbeda. Penempatan selama ini manual sehingga ruang geladak terbuang dan keuntungan tidak optimal. Tugasmu: membangun sistem pendukung keputusan yang menentukan kendaraan mana yang diangkut serta posisi & orientasinya di geladak, bisa mengevaluasi berbagai konfigurasi, dan menghasilkan penataan optimal.

## 3. Data persoalan (variabel)
| Entitas | Atribut | Contoh | Arti |
|---|---|---|---|
| Vehicle | id | "Owen" | penanda kendaraan |
| | dimensions | {w:3, l:4} | ukuran lebar × panjang (satuan sel grid) |
| | orientation | Horizontal | mendatar (Horizontal) atau tegak (Vertical) |
| | shippingFee | 60 | tarif/keuntungan kirim (juta rupiah) |
| | weight | 5 | berat kendaraan (satuan unit) |
| | ETA | 3 | perkiraan waktu kedatangan (sisa hari) |
| Ship | dimensions | {w:10, l:20} | ukuran geladak (satuan sel grid) |
| | maxCapacity | 500 | kapasitas angkut maksimum (satuan unit) |

Catatan: ETA tersedia di data tapi **tidak dipakai** oleh objective function maupun batasan wajib — relevan hanya kalau kamu mengambil bonus objective function alternatif.

## 4. Batasan (wajib selalu terpenuhi)
1. **Tidak boleh bertumpuk** — satu sel grid geladak maksimal ditempati satu kendaraan.
2. **Harus di dalam geladak** — seluruh sel yang ditempati kendaraan yang diangkut harus berada dalam dimensi geladak. Kendaraan yang tidak diangkut berada utuh di luar.
3. **Kapasitas** — total berat kendaraan yang diangkut tidak boleh melebihi MaxCapacity kapal.

## 5. Objective function
**Memaksimalkan total ShippingFee** kendaraan yang diangkut. (Boleh objective lain hanya sebagai bonus, tanpa menambah variabel.)

## 6. Aturan teknis representasi
- Kendaraan dan geladak = grid 2D persegi/persegi panjang; koordinat bilangan bulat; cara representasi koordinat dan struktur data bebas (misal key-value/JSON).
- Ukuran kendaraan berbeda-beda; tidak semua kendaraan harus diangkut.
- State awal (**initial state**) diacak.
- Langkah pergerakan (**neighbor/move**) per iterasi hanya boleh salah satu dari:
  1. Menukar dua kendaraan, baik di dalam maupun di luar geladak.
  2. Memindahkan satu kendaraan ke koordinat yang berbeda.
  3. Mengubah orientasi satu kendaraan.
- State akhir (**final state**) = pemetaan semua kendaraan (di dalam maupun di luar) + pemetaan semua koordinat geladak. State dan visualisasi harus bisa menunjukkan:
  1. Keadaan semua kendaraan yang di dalam maupun di luar,
  2. Keadaan geladak kapal,
  3. Nilai (value) state tersebut.

## 7. Algoritma yang wajib diimplementasi (Python)
1. **Hill-climbing** — cukup salah satu varian: Steepest Ascent, Stochastic, Hill-climbing with Sideways Move, atau Random Restart.
2. **Simulated Annealing**.
3. **Genetic Algorithm**.

Boleh menambah heuristik buatan sendiri atau dari referensi untuk membantu pencarian, asal masih dalam lingkup local search, dan wajib dijelaskan di laporan.

## 8. Skema eksperimen
### 8.1 Hill-climbing & Simulated Annealing — masing-masing 3 kali run
Catatan yang berlaku untuk **semua** algoritma:
- a. State awal dan state akhir,
- b. Nilai objective function akhir,
- c. Plot nilai objective terhadap banyak iterasi,
- d. Durasi proses pencarian.

Tambahan sesuai varian/algoritma:
- **Steepest Ascent / Stochastic HC**: banyak iterasi sampai pencarian berhenti.
- **HC with Sideways Move**: banyak iterasi sampai berhenti + parameter *maximum sideways move* (pencarian dihentikan saat sideways move mencapai maksimum).
- **Random Restart HC**: banyak restart, banyak iterasi per restart, + parameter *maximum restart* (pencarian dihentikan saat restart mencapai maksimum).
- **Simulated Annealing**: plot e^(ΔE/T) terhadap banyak iterasi + frekuensi "stuck" di local optima.

### 8.2 Genetic Algorithm — total 18 run
Dua parameter yang bisa diubah: **jumlah populasi** dan **banyak iterasi**.
- Sweep A: populasi dijadikan kontrol (tetap), pilih 3 variasi banyak iterasi berbeda → masing-masing konfigurasi dijalankan 3 kali (9 run).
- Sweep B: banyak iterasi dijadikan kontrol (tetap), pilih 3 variasi jumlah populasi berbeda → masing-masing konfigurasi dijalankan 3 kali (9 run).

Per eksperimen catat:
- a. State awal dan state akhir,
- b. Nilai objective function akhir,
- c. Plot nilai objective **maksimum dan rata-rata populasi** terhadap banyak iterasi,
- d. Jumlah populasi,
- e. Banyak iterasi,
- f. Durasi proses pencarian.

## 9. Visualisasi (wajib, cara bebas)
Program harus memvisualisasikan: initial state, final state, dan hasil eksperimen (informasi yang ditampilkan menyesuaikan catatan eksperimen di bagian 8, misal plot objective vs iterasi). Bahasa/library bebas (misal web-based); cara visualisasi tidak memengaruhi penilaian — pilih yang paling memudahkan penjelasan saat demo.

## 10. Analisis di laporan
Pertanyaan acuan (boleh diganti/ditambah):
1. Seberapa dekat tiap algoritma mendekati global optima, dan mengapa hasilnya demikian?
2. Bagaimana perbandingan hasil pencarian antar algoritma?
3. Bagaimana perbandingan durasi pencarian antar algoritma?
4. Seberapa konsisten hasil akhir antar eksperimen (antar run)?
5. Bagaimana pengaruh banyak iterasi dan jumlah populasi terhadap hasil GA?
6. dst.

## 11. Repository & pengumpulan
- Repo GitHub pribadi: `IF3070_GXX_NamaKelompok` (XX = nomor kelompok di sheet kelompok). Boleh private selama pengerjaan, **wajib public sebelum pengumpulan**.
- Commit & branching mengikuti best practice; disarankan **semantic commit** supaya kontribusi tiap anggota terlihat.
- Struktur wajib:
  - `src/` — source code,
  - `docs/` — laporan PDF (lihat bagian 12),
  - `output/` — hasil run algoritma,
  - `README.md` — deskripsi singkat repo, cara setup & run, pembagian tugas tiap anggota.
- Pengumpulan: buat **release dengan tag `vX.Y`** sebelum deadline (X = nomor milestone, Y = revisi mulai dari 0, misal `v1.0` untuk kumpulan pertama). Terlambat = pengurangan nilai. Unggah juga di Edunex.

## 12. Isi laporan (PDF di `docs/`)
1. Cover
2. Deskripsi persoalan
3. Pembahasan: penjelasan implementasi algoritma local search (deskripsi fungsi/kelas + source code) serta hasil & analisis eksperimen (dengan visualisasi dari program)
4. Kesimpulan dan saran
5. Pembagian tugas tiap anggota kelompok
6. Referensi
7. Lampiran Form Penggunaan Generative AI (halaman 19 Panduan AI ITB), diisi sejujur-jujurnya
8. Lampiran lainnya (bila ada)

## 13. Timeline
| Waktu | Kegiatan |
|---|---|
| Kamis, 24 September 2026 | Rilis Tugas Besar 1 |
| Minggu, 27 September 2026, 23.59 WIB | Batas pengisian daftar kelompok (3 orang, boleh lintas kelas) |
| Kamis, 22 Oktober 2026, 22.22 WIB | Pengumpulan final di Edunex (+ release tag di GitHub) |
| Menyusul | Batas pengisian jadwal demo |
| Menyusul | Pelaksanaan demo |

## 14. Aturan kerjasama & akademik
- Utamakan implementasi yang mengacu pada **salindia kuliah**; kalau mengadaptasi metode dari internet, wajib cantumkan sumber sebagai komentar di kode + referensi di laporan.
- Pertanyaan ke asisten hanya lewat **komentar pada Unggahan Tugas Besar di Teams**; pertanyaan personal tidak dijawab.
- Dilarang plagiarisme serta meminta/memberikan kode dan/atau laporan ke kelompok lain → nilai E untuk seluruh anggota yang terlibat. Penggunaan Generative AI wajib dilaporkan jujur lewat form di laporan.

## 15. Bonus (kerjakan hanya setelah semua spesifikasi wajib selesai)
1. Objective function selain yang didefinisikan, tanpa menambah variabel (+3 poin).
2. Implementasi **seluruh** varian algoritma Hill-climbing (+7 poin).
3. Multi-kapal: program menerima lebih dari satu kapal dan memaksimalkan value total angkutan armada (+10 poin).

## 16. Referensi resmi spesifikasi
Seri video M. L. Khodra, "Beyond Classical Search" (2021): Classical vs Local Search; State (Value, Neighbor); Hill-climbing Search; Simulated Annealing; Genetic Algorithm.

## 17. Pembagian kerja bertiga (saran)
- **Orang 1 — Mesin inti:** representasi grid, pengecekan batasan, fungsi objective, inisialisasi acak, 3 jenis move, pencatatan eksperimen (nilai, waktu, file output).
- **Orang 2 — Algoritma:** hill-climbing, simulated annealing, genetic algorithm + semua variasi eksperimennya, memakai mesin inti buatan orang 1.
- **Orang 3 — Visualisasi & laporan:** tampilan state awal/akhir, plot-plot, laporan PDF + form AI, README, kerapian repo, release & tag, pastikan repo jadi public tepat waktu.

Semua anggota tetap menjalankan eksperimen bagiannya masing-masing dan menulis analisisnya sendiri; pembagian tugas wajib ditulis di laporan dan README.

Catatan tambahan: jujur di form penggunaan AI; plagiarisme atau tukar kode/laporan antar kelompok = nilai E satu kelompok. Bonus hanya setelah wajib selesai: objective alternatif (+3), semua varian hill-climbing (+7), multi-kapal (+10).
