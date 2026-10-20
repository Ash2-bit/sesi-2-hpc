# Sesi 2: High Performance Computing (HPC)
**Akselerasi Pemrosesan Paralel Citra Digital Skala Besar untuk Analisis Infrastruktur**

## 📖 Deskripsi Studi Kasus
Dalam skenario riil pemantauan fasilitas publik, inspeksi jalan raya, maupun preservasi bangunan bersejarah, sistem sering kali menerima ratusan hingga ribuan foto udara/citra resolusi tinggi sekaligus. Jika citra-citra ini diproses secara sekuensial (satu per satu pada satu thread CPU), sistem akan mengalami *bottleneck* komputasi berat yang memakan waktu berjam-jam.

Studi kasus ini mensimulasikan penerapan prinsip **High Performance Computing (HPC)** melalui teknik *Multi-Core Parallel Computing*. Kita akan mengolah sekumpulan matriks citra digital untuk ekstraksi pola/fitur kerusakan. Skrip ini secara otomatis mengukur perbandingan waktu eksekusi (*speedup benchmark*) antara eksekusi serial konvensional dan eksekusi paralel yang memanfaatkan seluruh *logical cores* pada unit pemroses.

## 🎯 Tujuan Eksekusi
1. **Pemrosesan Beban Tinggi (Heavy Workload Simulation):** Mensimulasikan ekstraksi fitur dan transformasi matriks citra digital pada setiap file input.
2. **Implementasi Paralelisasi:** Membagi antrean citra secara merata ke dalam *worker pools* CPU menggunakan modul `multiprocessing`.
3. **Analisis Performa Empiris:** Mengukur waktu tempuh (*execution time*) dan menghitung faktor percepatan (*speedup factor*) secara objektif.

## 💻 Implementasi Kode (Python - Multiprocessing & Time Benchmark)
Berikut adalah skrip Python yang dapat dijalankan melalui file `parallel_image_processing.py`.

```python
import multiprocessing
import time
import numpy as np

# 1. Simulasi Fungsi Ekstraksi Fitur Citra (Komputasi Intensif Matriks)
def proses_ekstraksi_citra(id_citra):
    """
    Simulasi pemrosesan citra resolusi tinggi (misal: filtering/edge detection)
    menggunakan matriks acak 2000x2000 piksel.
    """
    # Simulasi memuat matriks piksel citra
    matrix_citra = np.random.randint(0, 256, size=(2000, 2000), dtype=np.uint8)
    
    # Simulasi transformasi matematika berat (Filter Gaussian/Deteksi Tepi)
    hasil_filter = np.sin(matrix_citra) ** 2 + np.cos(matrix_citra) ** 2
    rata_intensitas = np.mean(hasil_filter)
    
    # Menyimulasikan waktu I/O dan kalkulasi
    time.sleep(0.1)
    
    return f"Citra #{id_citra:03d} [Selesai] -> Rata-rata Intensitas: {rata_intensitas:.4f}"

if __name__ == '__main__':
    # Menyiapkan batch 32 citra infrastruktur yang akan dianalisis
    daftar_citra = list(range(1, 33))
    jumlah_core = multiprocessing.cpu_count()
    
    print(f"=== BENCHMARK HIGH PERFORMANCE COMPUTING (HPC) ===")
    print(f"Jumlah Core CPU Terdeteksi: {jumlah_core} Cores\n")
    
    # -------------------------------------------------------------
    # PENGUJIAN 1: Eksekusi Sekuensial (Serial / Non-HPC)
    # -------------------------------------------------------------
    print("1. Memulai Eksekusi Sekuensial (Serial)...")
    mulai_serial = time.time()
    hasil_serial = []
    for img_id in daftar_citra:
        hasil_serial.append(proses_ekstraksi_citra(img_id))
    durasi_serial = time.time() - mulai_serial
    print(f"   [SELESAI] Waktu Eksekusi Serial: {durasi_serial:.2f} detik\n")
    
    # -------------------------------------------------------------
    # PENGUJIAN 2: Eksekusi Paralel (HPC Multi-Processing)
    # -------------------------------------------------------------
    print(f"2. Memulai Eksekusi Paralel (Multi-Core dengan {jumlah_core} Workers)...")
    mulai_paralel = time.time()
    with multiprocessing.Pool(processes=jumlah_core) as pool:
        hasil_paralel = pool.map(proses_ekstraksi_citra, daftar_citra)
    durasi_paralel = time.time() - mulai_paralel
    print(f"   [SELESAI] Waktu Eksekusi Paralel: {durasi_paralel:.2f} detik\n")
    
    # -------------------------------------------------------------
    # HASIL EVALUASI BENCHMARK
    # -------------------------------------------------------------
    speedup = durasi_serial / durasi_paralel if durasi_paralel > 0 else 0
    efisiensi = (speedup / jumlah_core) * 100
    
    print("=== KESIMPULAN PERFORMA KOMPUTASI ===")
    print(f"• Total Citra Diproses : {len(daftar_citra)} File")
    print(f"• Efisiensi Waktu     : Penghematan {(durasi_serial - durasi_paralel):.2f} detik")
    print(f"• Speedup Factor     : {speedup:.2f}x lebih cepat dibanding eksekusi serial")
    print(f"• Efisiensi CPU      : {efisiensi:.1f}%")
