# Analisis Keterlambatan & Akurasi Waktu Pengiriman Pizza 🍕 

**Status:** Completed  
**Role:** Data Analyst  
**Tools:**  
- **Python** (Pandas, Seaborn) - *Cleaning & Visualization*
- **SQL** (SQLite, Window Functions) - *Advanced Querying*
- **Microsoft Excel** - *Reporting*

## 📌 Business Understanding (Latar Belakang)
Berdasarkan data operasional, ditemukan masalah kritis dimana **100% pesanan mengalami keterlambatan** (*delay*) jika dibandingkan dengan estimasi aplikasi. Proyek ini bertujuan untuk mengaudit algoritma estimasi waktu dan mencari akar penyebab keterlambatan tersebut.

**Tujuan:**
1. Memvalidasi klaim keterlambatan 100%.
2. Mengidentifikasi apakah faktor *Traffic* (Macet) atau *Kitchen* (Dapur) yang menjadi penyebab utama.
3. Memberikan rekomendasi perbaikan algoritma estimasi.

---

## 🔍 Key Insights (Temuan Utama)
Melalui analisis hybrid menggunakan Python dan SQL, ditemukan beberapa fakta menarik:

### 1. The "Naïve" Algorithm
Algoritma estimasi saat ini memiliki korelasi **0.99** dengan jarak. Ini membuktikan sistem hanya menghitung `Jarak / Kecepatan` tanpa memperhitungkan kondisi lalu lintas atau waktu masak.

### 2. Visualisasi: Estimasi vs Realita
<img width="1164" height="625" alt="image" src="https://github.com/user-attachments/assets/80a787aa-1b6c-44f6-9ac6-bfbc3b40c74a" />

> Terlihat pola Estimasi (Hijau) terlalu linear dan optimis, sedangkan Realita (Merah) jauh lebih lama dan fluktuatif.

### 3. Impact of Traffic
<img width="971" height="642" alt="image" src="https://github.com/user-attachments/assets/1945ba58-6de0-4a7a-8575-71b3a2a20c06" />

> Kemacetan memperparah keterlambatan, namun data menunjukkan bahwa bahkan di kondisi "Low Traffic" pun pesanan tetap terlambat. Ini mengonfirmasi kesalahan ada pada rumus dasar estimasi.

---

## 💻 SQL Implementation (Technical Highlights)
Untuk memvalidasi temuan Python, dilakukan *querying* langsung ke database untuk kasus operasional spesifik:

### A. Operational Bottlenecks (Window Functions)
Menggunakan teknik `RANK() OVER (PARTITION BY ...)` untuk mengidentifikasi **order paling lambat (worst-case)** di setiap cabang restoran secara otomatis.
* *Insight:* Ditemukan cabang tertentu memiliki *outlier* durasi ekstrem (90+ menit) yang tidak terdeteksi jika hanya melihat rata-rata.

### B. Efficiency Ratio
Melakukan kalkulasi *Calculated Fields* di SQL untuk menghitung efisiensi `Menit per KM`.
* *Insight:* Pizza dengan **High Complexity** (banyak topping) memiliki tingkat kegagalan kirim tepat waktu 5x lebih tinggi dibanding pizza reguler.

*(Script SQL lengkap tersedia di file notebook)*

---

## 📂 File Structure
- `Pizza-Delivery-Efficiency-Analysis.ipynb`: **[MAIN FILE]** Notebook berisi Full Analysis.
- `SQL_Implementation.ipynb`** Notebook implementasi teknis SQL.
- `Pizza-Delivery-Efficiency-Analysis.xlsx`: File Excel berisi Dashboard interaktif dan Pivot Table.
- `Enhanced_pizza_sell_data_2024-25.xlsx`: Dataset mentah.

---

## 💡 Rekomendasi Bisnis
1. **Revisi Rumus Estimasi:** Tambahkan *Base Preparation Time* (misal: 10-12 menit) ke dalam rumus, jangan hanya mengandalkan jarak.
2. **Dynamic Traffic Buffer:** Tambahkan koefisien pengali waktu (x1.2 atau x1.5) saat kondisi *Traffic* terdeteksi 'High'.
3. **Special Handling for Complex Orders:** Pisahkan antrean dapur untuk pizza dengan kompleksitas tinggi (berdasarkan temuan SQL Analysis) untuk mencegah *bottleneck*.

### 📬 Connect with me
Jika Anda memiliki pertanyaan tentang analisis ini, mari terhubung di [LinkedIn Saya](https://www.linkedin.com/in/muiz-isman)
