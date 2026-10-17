Customer Shopping Behavior & Revenue Optimization Analysis

1. Executive Summary & Context

Dalam industri ritel yang kompetitif, memahami perilaku belanja pelanggan (customer shopping behavior) dan faktor pendorong pendapatan (revenue drivers) adalah kunci untuk meningkatkan efisiensi pemasaran serta retensi pelanggan.

Proyek ini menganalisis dataset transaksi pelanggan untuk mengevaluasi bagaimana faktor demografi, preferensi saluran belanja, efektivitas promosi, dan program langganan memengaruhi total pendapatan serta nilai pesanan pelanggan.

2. Business Problem Statement

Manajemen ritel menghadapi tantangan dalam mengoptimalkan margin laba dan meningkatkan retensi pelanggan karena belum memiliki visibilitas terpadu mengenai:

Sensitivitas Diskon vs. Profitabilitas: Diskon dan kode promo sering digunakan, namun bisnis belum mengetahui apakah pemberian diskon ini benar-benar mendorong ukuran keranjang belanja (basket size) di atas rata-rata atau justru menggerus margin laba tanpa peningkatan nilai belanja yang sepadan.

Efektivitas Program Langganan (Subscription): Rendahnya adopsi program langganan berbayar serta minimnya bukti empiris apakah pelanggan langganan menghasilkan Customer Lifetime Value (CLV) dan frekuensi pembelian yang jauh lebih tinggi dibanding non-langganan.

Personalisasi Berbasis Demografi: Penawaran promosi saat ini masih bersifat massal (one-size-fits-all), padahal setiap kelompok usia (age group) dan segmen gender memiliki preferensi kategori produk, metode pembayaran, serta jenis pengiriman yang berbeda secara signifikan.

Retensi dan Segmentasi Pelanggan: Bisnis belum memiliki sistem segmentasi yang jelas untuk membedakan antara pembeli baru (New Customers), pembeli berulang (Returning Customers), dan pelanggan loyal (Loyal Customers) dalam strategi re-engagement.

3. Core Business Objectives

Tujuan utama proyek analisis ini adalah:

Menganalisis Pola Belanja Pelanggan: Mengidentifikasi tren belanja berdasarkan kelompok demografi (gender, usia, lokasi).

Mengevaluasi Ketergantungan Promosi: Mengukur rasio transaksi dengan diskon pada setiap kategori produk dan mengidentifikasi segmen pembeli diskon bernilai tinggi (high-value discount shoppers).

Mengoptimalkan Portofolio Produk: Menentukan 3 produk terlaris di setiap kategori dan produk dengan kepuasan pelanggan tertinggi (berdasarkan ulasan pelanggan).

Meningkatkan Nilai Retensi: Mengidentifikasi karakteristik pembeli berulang (repeat buyers) untuk merancang program loyalitas dan kampanye retensi yang terarah.

4. Key Business Questions

Berapa kontribusi total pendapatan (revenue) dari masing-masing segmen gender dan kelompok usia?

Apakah pelanggan yang menggunakan diskon menghasilkan nilai transaksi di atas rata-rata belanja keseluruhan?

Produk apa saja yang memiliki tingkat ulasan (review rating) tertinggi vs. volume penjualan tertinggi di tiap kategori?

Kategori produk mana yang memiliki tingkat ketergantungan paling tinggi terhadap diskon?

Bagaimana perbandingan perilaku belanja antara pengguna langganan (subscribed) dan pengguna biasa?

Berapa proporsi pelanggan baru vs. pelanggan loyal, dan strategi apa yang tepat untuk meningkatkan konversi antar segmen tersebut?

5. Project Deliverables

A. Data Engineering & Transformation Pipeline (Python)

Jupyter Notebook (customer_data_cleaning.ipynb):

Dokumentasi imputasi missing values secara kontekstual (median rating berbasis kategori produk).

Standarisasi nama kolom ke format snake_case.

Feature engineering: pembuatan kolom age_group (pengelompokan kuantil) dan konversi frekuensi pembelian ke metrik numerik hari (purchase_frequency_days).

Validasi redundansi data (penghapusan kolom duplikat promo_code_used vs. discount_applied).

Database Ingestion Script:

Skrip koneksi SQLAlchemy & psycopg2 untuk memuat data bersih secara langsung ke tabel PostgreSQL.

B. Business Logic & Query Analysis (SQL)

Kumpulan Kueri Analitik PostgreSQL (analysis_queries.sql):

Analisis agregasi demografi dan metrik pendapatan.

Subqueries untuk identifikasi pelanggan diskon bernilai tinggi.

Analisis ketergantungan diskon menggunakan agregasi kondisional (CASE WHEN).

Segmentasi pelanggan (New, Returning, Loyal) berbasis Common Table Expressions (CTE).

Penentuan peringkat produk teratas per kategori menggunakan Window Functions (ROW_NUMBER() OVER PARTITION BY).

C. Executive Dashboard (Power BI)

File Dashboard Interaktif (customer_behavior_dashboard.pbix):

Kartu KPI Utama: Total Pendapatan, Total Pelanggan, Rata-rata Nilai Belanja (Average Order Value), dan Rata-rata Rating Ulasan.

Visualisasi Demografi: Distribusi pendapatan berdasarkan kelompok usia dan gender.

Analisis Kategori & Produk: Grafik kontribusi pendapatan dan performa volume penjualan produk terlaris per kategori.

Analisis Status Langganan: Distribusi pelanggan langganan vs non-langganan.

Interaktivitas: Kontrol pemotong data (Slicers) multi-dimensi (Gender, Subscription Status, Kategori Produk, Metode Pengiriman).

D. Insights & Portfolio Assets

Executive Summary Report: Ringkasan wawasan bisnis utama dan rekomendasi strategis yang dapat ditindaklanjuti (actionable recommendations).

Slide Presentasi Manajemen: Dek presentasi ringkas dan visual untuk pemangku kepentingan (stakeholders).

GitHub Repository Documentation (README.md): Dokumentasi proyek lengkap dan terstruktur mencakup arsitektur data, metodologi, cuplikan visual dashboard, dan instruksi instalasi/replikasi proyek.