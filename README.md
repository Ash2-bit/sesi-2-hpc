# Akselerasi Pemrosesan Citra Infrastruktur dengan High Performance Computing (HPC)

## Deskripsi

Proyek ini merupakan studi kasus penerapan *High Performance Computing* (HPC) melalui teknik *Multi-Core Parallel Processing* untuk mengurai *bottleneck* (kemacetan) komputasi pada pemrosesan data berskala besar.
Proyek ini diadaptasi dari materi praktisi mengenai arsitektur sistem dan pemrosesan paralel, yang menyoroti pentingnya efisiensi pemanfaatan perangkat keras (*hardware utilization*) sebelum sebuah perusahaan memutuskan untuk membeli *server* yang lebih mahal.

## Tujuan

Tujuan proyek ini adalah:
1. Memecahkan masalah antrean pemrosesan sekuensial yang memakan waktu lama pada data bervolume besar.
2. Mengimplementasikan pustaka `multiprocessing` untuk mendistribusikan beban kerja (*workload*) secara merata ke seluruh inti (*core*) CPU.
3. Membandingkan durasi waktu eksekusi antara pemrosesan konvensional (Serial) dengan pemrosesan paralel (HPC).
4. Menghasilkan metrik *Speedup Factor* untuk mengevaluasi tingkat efisiensi algoritma komputasi.

## Studi Kasus

Sebuah departemen pekerjaan umum (Bina Marga) menerima ratusan citra resolusi tinggi dari *drone* pengawas setiap harinya untuk menganalisis retakan aspal dan kerusakan infrastruktur.
Aplikasi yang ada saat ini memproses gambar tersebut satu per satu (sekuensial). Akibatnya, sistem sering kali membeku (*hang*) dan laporan harian terlambat rilis. 
Melalui pendekatan HPC, komputasi matriks gambar akan dipecah dan dikerjakan serentak oleh beberapa *worker* CPU. Efisiensi waktu ini berfungsi sebagai *decision support enabler*, memastikan para insinyur lapangan mendapatkan hasil deteksi kerusakan lebih awal untuk penanganan darurat.

## Dataset & Beban Kerja (Workload)

Karena citra satelit/drone asli berukuran sangat besar, dataset disimulasikan menggunakan Python dan NumPy.
*   Sistem akan menghasilkan **100 matriks berukuran 2000x2000 piksel** (mensimulasikan 100 gambar beresolusi tinggi).
*   Pada setiap matriks, akan diaplikasikan operasi matematika trigonometri berulang untuk mensimulasikan beban filter piksel digital (*edge detection* atau transformasi Fourier).

## Metode

Tahapan utama yang dilakukan:
1. *Workload Simulation* (Pembuatan fungsi beban komputasi CPU murni tanpa jeda I/O).
2. *Serial Execution* (Eksekusi 100 data menggunakan 1 *core* CPU tunggal).
3. *Parallel Execution* (Eksekusi 100 data yang dipecah ke seluruh *core* CPU yang tersedia menggunakan `multiprocessing.Pool`).
4. *Benchmarking & Evaluation* (Pencatatan waktu dan perhitungan metrik performa).

## Evaluasi

Performa algoritma dievaluasi menggunakan metrik komputasi standar:
*   **Execution Time (detik):** Waktu tempuh total untuk menyelesaikan seluruh antrean data.
*   **Speedup Factor:** Rasio kecepatan antara eksekusi serial dibandingkan paralel (Serial Time / Parallel Time). Menunjukkan berapa kali lipat sistem menjadi lebih cepat.
Dalam kasus industri *big data*, *Speedup Factor* merupakan justifikasi teknis (KPI) untuk menilai apakah arsitektur *backend* sudah berjalan secara optimal.

## Teknologi

*   Python
*   Google Colab / Jupyter Notebook (`.ipynb`)
*   Pustaka `multiprocessing` (Standard Library)
*   NumPy
*   Matplotlib
*   Seaborn

## Cara Menjalankan

1. Proyek dapat dijalankan menggunakan Google Colab (sangat disarankan agar terlihat spesifikasi CPU server Google).
2. Buka file `hpc_parallel_processing.ipynb` menggunakan Google Colab.
3. Jalankan *notebook* dari awal hingga akhir.
4. *Benchmark* waktu dan grafik perbandingan performa (*Serial vs Parallel*) akan dirender secara otomatis pada *cell* terakhir.


## Kesimpulan
Proyek ini membuktikan bahwa penulisan kode yang efektif sama pentingnya dengan spesifikasi komputer. Pemrosesan konvensional sering kali hanya memanfaatkan sebagian kecil dari kemampuan prosesor modern (meninggalkan core lain dalam status idle).
Dengan mengadopsi prinsip HPC, pemrosesan citra infrastruktur dapat dipercepat secara signifikan. Dalam konteks mitigasi bencana atau pelaporan publik, kecepatan pemrosesan data ini adalah kunci untuk menghasilkan keputusan yang responsif dan tepat waktu.
## Struktur Repository

```text
sesi-2-hpc/
├── hpc_parallel_processing.ipynb
├── README.md
└── requirements.txt
