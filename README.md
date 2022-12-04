# Analisis ChurnRate Pelanggan Telco

Dalam case ini, akan dijelaskan bagaimana saya membangun model yang  bermanfaat untuk menjelaskan tingkat churn berdasarkan dataset Pelanggan Kaggle Telco. Proses khusus meliputi (1) Latar Belakang dan Masalah, (2) Rangkuman Data dan Analisis Eksplorasi, (3) Analisis Data, (4) Rekomendasi Strategi, Keterbatasan, dan Penelitian Masa Depan.
Untuk melihat kode lengkap yang digunakan, temukan GitHub saya.

## Bagian 1 — Latar Belakang dan Masalah
### 1.1 Latar Belakang
Dengan peningkatan jumlah pelanggan yang sangat besar yang menggunakan layanan telepon, divisi pemasaran sebuah perusahaan telekomunikasi ingin menarik lebih banyak pelanggan baru dan menghindari pemutusan kontrak dari pelanggan yang sudah ada (churn rate). Agar perusahaan telekomunikasi memperluas pelanggannya, tingkat pertumbuhannya (jumlah pelanggan baru) harus melebihi tingkat churn (jumlah pelanggan yang ada). 

Beberapa faktor yang menyebabkan pelanggan lama meninggalkan perusahaan telekomunikasi mereka adalah penawaran harga yang lebih baik, layanan internet yang lebih cepat, dan pengalaman online yang lebih aman dari perusahaan lain.
Tingkat churn yang tinggi akan berdampak buruk pada keuntungan perusahaan dan menghambat pertumbuhan. 

Prediksi churn saya akan dapat memberikan kejelasan kepada perusahaan telekomunikasi tentang seberapa baik mempertahankan pelanggan yang sudah ada dan memahami apa alasan mendasar yang menyebabkan pelanggan yang sudah ada memutuskan kontrak mereka (tingkat churn yang tinggi).
Perusahaan telekomunikasi dapat menggunakan analisis saya untuk mengukur apakah menyediakan produk yang bermanfaat dibandingkan dengan produk yang disediakan oleh pesaingnya. Karena biaya untuk memperoleh pelanggan baru jauh lebih tinggi daripada mempertahankan pelanggan yang sudah ada, perusahaan dapat menggunakan analisis tingkat churn untuk memberikan diskon, penawaran khusus, dan produk unggulan untuk mempertahankan pelanggan saat ini.
### 1.2 Sumber Data
Kumpulan data perusahaan telekomunikasi tersedia di Kaggle , yang berasal dari kumpulan kumpulan sampel IBM. Perusahaan menyediakan layanan rumah dan internet untuk 7043 pelanggan di California. Tantangan saya adalah membantu perusahaan memprediksi perilaku untuk mempertahankan pelanggan dan menganalisis semua data pelanggan yang relevan untuk mengembangkan program retensi pelanggan yang terfokus.
Dataset yang disediakan terdiri dari informasi di bawah ini:
1.	Informasi demografis tentang pelanggan termasuk jenis kelamin, usia, status perkawinan
2.	Informasi akun pelanggan termasuk jumlah bulan tinggal bersama perusahaan, tagihan tanpa kertas, metode pembayaran, biaya bulanan, dan total biaya
3.	Perilaku penggunaan pelanggan, seperti streaming TV, streaming film
4.	Layanan yang didaftarkan pelanggan: layanan telepon, kelipatan, layanan internet, keamanan online, pencadangan online, perlindungan perangkat, dan dukungan teknis
5.	Pelanggan churn di mana pelanggan pergi dalam sebulan terakhir
1.3 Tujuan Model
1.	Apa faktor terpenting yang berkontribusi pada tingkat retensi yang tinggi?
2.	Model analitik mana yang dapat secara akurat memprediksi tingkat churn pelanggan?
3.	Membuat API Backend dan Frontend untuk melakukan prediksi pengecekan pelanggan yang akan churn?
Analisis yang dilakukan adalah
 
## Bagian 2 — Rangkuman Data dan Analisis Eksplorasi
Data yang saya gunakan untuk menganalisis adalah data sekunder yang tersedia di Kaggle, platform agregasi data sumber terbuka. Sebagian data terlampir pada gambar 1.
 

### 2.1 Pengenalan Data
Setelah membaca data menggunakan Pandas dengan Python, ditemukan bahwa ada sedikit data yang hilang dari kumpulan data mentah. Data ini dilakukan imputasi dengan  pengisian nilai median dikarenakan datanya skewed.
 

 

Sebagian besar fitur seperti jenis kelamin, layanan telepon, hingga metode pembayaran semuanya adalah data kategorikal. Biaya Bulanan dan TotalBiaya adalah data numerik. Statistik ringkasan untuk Tagihan Bulanan adalah sebagai berikut:
 
Rata-rata, orang yang membayar layanan tersebut $64,76 dan biaya bulanan termahal adalah $118,75. Biaya bulanan termurah adalah $18,25.

### 2.2 Korelasi
Setelah mengonversi semua data kategorikal menggunakan Label Encoding dan encoder, saya menjalankan korelasi berpasangan untuk semua fitur:
 
Dari heatmap tersebut terlihat bahwa fitur 'Contract' dan 'Tenure' memiliki korelasi yang tinggi. Hal ini masuk akal karena fitur ini mengukur loyalitas pelanggan.
“StreamingTV”, “StreamingMovie”, “Multiple Lines” dan “Monthly Charges” memiliki korelasi yang tinggi satu sama lain. Menurut saya, hal ini terjadi karena pelanggan yang melakukan streaming film cenderung juga melakukan streaming TV. Tagihan bulanan mereka cenderung naik karena banyaknya data yang mereka gunakan saat menonton film atau acara TV. Untuk pelanggan yang memiliki kelipatan di akun mereka, mereka akan cenderung membayar lebih dari pelanggan yang hanya memiliki satu baris.

### 2.3 Analisis data eksplorasi
Data target merupakan data imbalance dengan jumlah pelanggan kategori churn lebih sedikit dari pada yang tidak churn.
 

Jika dilihat TotalCharges, pelanggan yang churn adalah pelanggan-pelanggan yang TotalCharges lebih kecil.
 

Selain itu diketahui juga bahwa proporsi pelanggan Wanita vs prian yang churn dan tidak churn cenderung sama
 
### 2.4 Encoding
Encoding dilakukan pada fitur yang termasuk dalam kategorikal.  Ubah “Tidak” menjadi 0 dan “Ya” menjadi 1. Selain itu Ada tiga kolom fitur yang harus dikodekan menjadi faktor:
1.	Layanan Internet yang meliputi DSL, fiber optic, dan no.
2.	Kontrak yang mencakup bulan ke bulan, satu tahun, dan dua tahun.
3.	Metode Pembayaran yang meliputi transfer Bank, Kartu Kredit, Cek Elektronik, dan cek Surat.
sebagaimana di bawah ini:
 
### 2.5 Imbalance Handling
Metode SMOTE dan undersampling dipilih untuk melakukan handling terhadap data imbalance sehingga analisis dilakukan terhadap 2 set data tsb (SMOTE dan Undersampling).
 
 
## Bagian 3 — Analisis Data, temuan kunci, dan kesimpulan
Model yang saya pilih untuk data saya adalah (1) Decision Tree sebagai Baseline; (2) Ensemble : Random Forest dan XGBoost. Metrics yang digunakan untuk evaluasi adalah AUC.

### 3.1 Decision Tree (Baseline)
Decision tree pada analisis ini menghasilkan AUC 67% untuk SMOTE dataset dan 64% untuk undersampling data set.
 
 
### 3.2 Ensemle (RandomForest dan XGBoost)
Untuk optimalisasi metode ensemble digunakan dataset undersampling serta GridCV dalam rangka hyperparameter tuning. Pemilihan undersampling data dikarenakan data ini memiliki akurasi lebih rendah pada baseline model. Model ensemble yang dapat menghasilkan metode terbaik selanjutnya akan dipilih sebagai model final untuk deployment API.
Dari hasil eksperimen diperoleh bahwa ensemble method RandomForest mendapat AUC yang paling tinggi yaitu 83.8% dan mengungguli XGboost yang mendapatkan skor 83.3%. Model RandomForest akan dipilih sebagai final model.
 
# Bagian 4 — API Deployment
### 4.1 Backend
API deployment dilakukan dengan mengubah kode notebook (ipynb) menjadi python native (py). Untuk service backend-nya saya menggunakan FLASK API library yang di deploy pada HEROKU Server (masih aktif ketika API ini dibuat untuk demo video per tanggal 28 Nov 2022) di alamat server:
https://api-customer-chrun.herokuapp.com/predict 
 
Adapun mode API nya sukses ketika ditest dengan postman. Untuk dokumentasi API-nya dapat dilihat pada alamat:
https://documenter.getpostman.com/view/14828231/2s83tGnr7X 
 
### 4.2 Frontend
Frontend untuk mengirimkan request POST layanan API dibuat dengan HTML dan juga PHP serta bootsrap CSS dengan tampilah sbb:
