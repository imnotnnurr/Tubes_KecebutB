# 🧠 EEG Eye-State Intelligent Monitoring — Progres Minggu 12 (Kelompok 14)

**Mata Kuliah:** TE2514031 | Tugas Besar Kecerdasan Buatan | Gasal 2026/2027  
**Institusi:** Program Studi Teknik Elektro, Institut Teknologi Kalimantan (ITK)  
**Tahap:** Minggu 12 — Data, Preprocessing, Baseline & Perencanaan  

## 1. Tujuan & Fokus Kerja Minggu 12
Repository ini memuat artefak progres **Minggu 12** untuk sistem klasifikasi keadaan mata (*Eye-Open* vs *Eye-Closed*) berbasis sinyal Electroencephalography (EEG) 14 kanal dari dataset publik [UCI Machine Learning Repository — EEG Eye State (ID=264)](http://archive.ics.uci.edu/ml/datasets/EEG+eye+state).

Fokus pengerjaan pada Minggu 12 mencakup:
1. **Validasi Target, Tipe Problem, dan Kualitas Dataset**: Memverifikasi dimensi data (`14.980` sampel × `15` kolom), tipe problem klasifikasi sekuensial biner (`0` = *eye-open* 55,1%, `1` = *eye-closed* 44,9%), serta memastikan `0` *missing values*.
2. **Exploratory Data Analysis (EDA) & Deteksi Outlier**: Menganalisis distribusi statistik 14 kanal EEG Emotiv EPOC, mengidentifikasi *spike artifact* ekstrem (hingga `715.897 µV` pada kanal `AF4` dan `642.564 µV` pada `FC5`), serta memetakan matriks korelasi antar-kanal.
3. **Split Kronologis Anti-Leakage & Penanganan Imbalance**:
   - Pembagian data secara berurutan waktu (**70% Train / 15% Validation / 15% Test**) tanpa pengacakan (*no random shuffle*) untuk mencegah kebocoran informasi temporal antar-jendela waktu yang berdekatan.
   - Pemangkasan *outlier* menggunakan batas **IQR 3×** (`[Q1 - 3·IQR, Q3 + 3·IQR]`) dan standardisasi **Z-score** yang dihitung **murni dari data Train**, lalu diterapkan ke Validation dan Test.
   - Pembentukan *sliding window* (`seq_len = 16`, ~125 ms pada frekuensi sampling 128 Hz) secara independen di dalam masing-masing split (`10.471` jendela train, `2.232` jendela val, `2.232` jendela test).
   - Analisis ketimpangan kelas (*temporal distribution shift*) pada segmen akhir rekaman (`test` memiliki rasio *eye-closed* 8,1% dibanding `train` 53,1%) menggunakan metrik evaluasi **F1-Score**, **Precision**, **Recall**, dan **Accuracy**, disertai perhitungan `class_weight`.
4. **Baseline Neural Network (Model A — LSTM)** & **Kerangka Aplikasi Streamlit Awal (`app.py`)**.

## 2. Struktur Repository Minggu 12

```text
├── W12_Proposal_EDA.pdf                    # Dokumen Proposal, EDA, Arsitektur, Risiko & Pembagian Peran (3 halaman)
├── W12_EEG_EDA_Preprocessing_Baseline.ipynb # Notebook Minggu 12 (Validasi, EDA, Split Anti-Leakage, Baseline LSTM)
├── app.py                                  # Kerangka awal aplikasi dashboard Streamlit (Minggu 12)
├── requirements.txt                        # Daftar pustaka Python untuk reproduksi & deployment Streamlit
└── README.md                               # Dokumentasi repository & panduan eksekusi
```

## 3. Cara Menjalankan di Lokal

### A. Persiapan Lingkungan
```bash
python -m venv .venv
source .venv/bin/activate  # Linux/macOS (atau .venv\Scripts\activate pada Windows)
pip install -r requirements.txt
```

### B. Menjalankan Notebook EDA & Baseline NN
Buka dan jalankan `W12_EEG_EDA_Preprocessing_Baseline.ipynb` melalui Jupyter Lab / Google Colab. Saat dijalankan, notebook akan mengunduh dataset UCI EEG Eye State (`id=264`), menampilkan seluruh grafik EDA, melatih **Model A (Baseline LSTM)**, dan mengekspor artefak model ke folder `artifacts_w12/`.

### C. Menjalankan Kerangka Aplikasi Streamlit (`app.py`)
```bash
streamlit run app.py
```
Aplikasi akan terbuka di `http://localhost:8501` dengan 5 tampilan: Ringkasan & Arsitektur Sistem, Validasi Data & EDA Interaktif, Split Anti-Leakage & Preprocessing, Evaluasi Baseline NN (Model A), serta Roadmap Skeleton Minggu 13–14.

## 4. Panduan Deployment URL Skeleton ke Streamlit Community Cloud
1. Unggah (*push*) seluruh isi repository ini ke akun GitHub kelompok (mis. `https://github.com/<username>/eeg-eyestate-kelompok14`).
2. Kunjungi [Streamlit Community Cloud](https://streamlit.io/cloud) dan masuk menggunakan akun GitHub.
3. Klik **Create app** → pilih repository, branch `main`, dan isi **Main file path** dengan `app.py`.
4. Klik **Deploy!** untuk memperoleh tautan publik *skeleton* aplikasi Streamlit (contoh format URL: `https://eeg-eyestate-kelompok14.streamlit.app`).

## 5. Pembagian Peran Anggota Kelompok 14

| Nama Anggota | Peran Utama | Tanggung Jawab Spesifik (Minggu 12 – 14) |
| :--- | :--- | :--- |
| **Anggita** | Lead Data, Baseline NN & Koordinator Integrasi | Validasi dataset UCI EEG, penyusunan pipeline *preprocessing* anti-leakage (IQR + Z-score + windowing), implementasi Baseline LSTM (Model A), dan integrasi laporan Minggu 12 |
| **[Nama Anggota 2 — NIM]** | Eksperimen Optimasi Metaheuristik (GA & PSO) | Analisis korelasi kanal pada EDA Minggu 12, penyiapan modul seleksi kanal Genetic Algorithm (Model B) & optimasi hyperparameter PSO (Model C & D) untuk Minggu 13 |
| **[Nama Anggota 3 — NIM]** | Sistem Inferensi Fuzzy & Engineer Streamlit | Penyusunan kerangka aplikasi `app.py`, konfigurasi deployment Streamlit Cloud, dan perancangan aturan Fuzzy Mamdani (Model E) untuk Minggu 14 |
