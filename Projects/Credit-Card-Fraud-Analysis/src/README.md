# Credit Card Fraud: Advanced Feature Engineering Pipeline

Proyek ini mendemonstrasikan proses _feature engineering_ pada dataset transaksi kartu kredit untuk kebutuhan deteksi anomali. Fokus utama proyek ini adalah mentransformasi data mentah (raw data) menjadi fitur-fitur yang memiliki nilai prediktif menggunakan logika bisnis (rule-based).

Proyek ini disimpan dalam folder `experiments/` sebagai bagian dari perjalanan belajar di repository [Zero-To-ML-Engineer].

## 📊 Dataset Overview

Dataset: `credit_card_fraud_dataset.csv` (+100,000 baris)
Kolom kunci yang diolah:

- `TransactionDate`: Data waktu transaksi.
- `Amount`: Nominal transaksi.
- `TransactionType`: Kategori transaksi (purchase/refund).
- `Location`: Lokasi geografis.

## 🛠️ Engineering Steps

### 1. Temporal Engineering (Datetime Accessors)

Mengoptimalkan kolom `TransactionDate` dengan mengonversinya ke objek datetime untuk mengekstrak:

- **HourOfDay**: Mengambil komponen jam (0-23).
- **Is_Night_Transaction**: Fitur boolean untuk menandai transaksi pada jam rawan (00:00 - 05:00).

### 2. Multi-Condition Risk Profiling

Menggunakan `np.where` bertingkat (nested) untuk melakukan kategorisasi risiko secara _vectorized_:

- **Critical**: Transaksi `refund` dengan nominal > 4000.
- **Medium**: Transaksi dengan nominal antara 2000 - 4000.
- **Low**: Transaksi di luar parameter risiko tinggi.

### 3. Anomaly Flagging via Custom Python Function

Berbeda dengan pendekatan lambda yang sederhana, saya mengimplementasikan **Custom Function** yang dipanggil melalui `.apply(axis=1)` untuk menangani logika baris yang lebih kompleks:

- **Logic**: Menandai transaksi (`Flag_Suspicious`) jika terjadi di 'New York' atau 'Houston' dengan nominal yang melebihi rata-rata (`mean`) seluruh transaksi.
- **Optimization**: Nilai rata-rata dihitung satu kali di luar fungsi untuk menjaga performa komputasi.

### 4. Data Sanitization

- Pembersihan string pada kolom `Location` menjadi format `Location_Code` (Uppercase, 3 karakter pertama).
- Penghapusan kolom `MerchantID` untuk memastikan privasi data sesuai standar manajemen data.

## 📂 Hasil Akhir

Seluruh proses ini menghasilkan dataset baru `processed_data.csv` yang kaya akan fitur (enriched features), siap untuk masuk ke tahap pemodelan Machine Learning.

---

**Author:** _ Muhammad Haerul Anwar _
