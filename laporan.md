# Laporan Proyek Machine Learning - Arif Kurniawan

## Domain Proyek

Dalam menghadapi tantangan global seperti perubahan iklim, degradasi tanah, dan pertumbuhan populasi, sektor pertanian dituntut untuk menjadi lebih efisien dan berkelanjutan. Menurut laporan FAO (2021), ketahanan pangan dunia sangat bergantung pada kemampuan petani dalam mengelola sumber daya alam secara optimal dan adaptif. Teknologi seperti Artificial Intelligence (AI) menjadi solusi potensial dalam mendukung pertanian cerdas (smart agriculture) karena mampu memberikan wawasan berbasis data untuk pengambilan keputusan yang lebih tepat.

Masalah utama yang dihadapi petani, khususnya di daerah terpencil, adalah minimnya akses terhadap informasi terkait kondisi tanah dan iklim yang memengaruhi hasil panen. Dengan adanya sistem berbasis AI yang menganalisis parameter seperti tingkat pH tanah, kelembapan, suhu, serta curah hujan, petani dapat diberikan rekomendasi pemupukan, waktu tanam, dan jenis tanaman yang sesuai.
## Business Understanding
Dalam menghadapi tantangan seperti perubahan iklim, degradasi tanah, dan pertumbuhan populasi, sektor pertanian perlu menjadi lebih Sektor pertanian perlu menjadi lebih efisien dan berkelanjutan untuk menghadapi tantangan seperti perubahan iklim dan degradasi tanah. Teknologi AI berperan penting dalam mendukung pertanian cerdas dengan memberikan analisis berbasis data. Melalui sistem AI yang memantau parameter seperti pH tanah, kelembapan, suhu, dan curah hujan, petani dapat memperoleh rekomendasi yang akurat terkait pemupukan, waktu tanam, dan jenis tanaman, sehingga hasil panen dapat ditingkatkan secara optimal.
### Problem Statements

1. Petani kesulitan mendapatkan rekomendasi berbasis data mengenai kesesuaian tanah dan iklim terhadap jenis tanaman.

2. Keputusan pertanian seringkali berdasarkan intuisi tanpa mempertimbangkan parameter penting seperti pH tanah, suhu, dan kelembapan.

3. Tidak ada sistem terintegrasi yang menyediakan wawasan prediktif bagi petani tentang kondisi pertanian mereka.
### Goals

* Mengembangkan sistem rekomendasi tanaman berbasis AI dengan memanfaatkan parameter tanah dan iklim.

* Memberikan wawasan real-time dan prediktif yang membantu petani meningkatkan produktivitas secara berkelanjutan.

Meningkatkan efisiensi dalam penggunaan pupuk, air, dan lahan pertanian.


### Solution statements
Membangun model klasifikasi AI menggunakan Random Forest untuk merekomendasikan jenis tanaman berdasarkan data parameter tanah dan iklim.
   
## Data Understanding
Berdasarkan dataset yang ada memiliki 3100 baris dan 10 kolom dan 
Dataset yang digunakan berasal dari kaggle (https://www.kaggle.com/datasets/nishchalchandel/crop-recommendation) yang terdiri dari berbagai parameter penting untuk menentukan jenis tanaman yang optimal berdasarkan kondisi lingkungan. 

### Variabel-variabel pada Kaggle dataset adalah sebagai berikut:
- Temperature: Suhu udara (°C) yang memengaruhi pertumbuhan tanaman.

- Humidity: Kelembapan udara (%) yang berpengaruh pada proses transpirasi.

- Rainfall: Curah hujan (mm), penting untuk kebutuhan air tanaman.

- PH: Tingkat keasaman tanah, memengaruhi penyerapan unsur hara.

- Nitrogen: Kandungan nitrogen, mendukung pertumbuhan daun.

- Phosphorous: Kandungan fosfor, penting untuk akar dan pembungaan.

- Potassium: Kandungan kalium, membantu ketahanan tanaman.

- Carbon: Kandungan karbon organik, meningkatkan kesuburan tanah.

- Soil: Jenis tanah (misal: lempung, pasir), memengaruhi struktur dan drainase.

- Crop: Jenis tanaman yang menjadi target prediksi.

EDA menunjukkan tidak ada nilai kosong atau data duplikat. Outlier ditangani menggunakan metode IQR, dan visualisasi distribusi fitur menunjukkan sebaran data yang relatif seimbang.

## Data Preparation
**1. Pemeriksaan dan Penanganan Nilai Kosong:**
Dataset diperiksa menggunakan fungsi ```ruby isnull().sum()``` dan tidak ditemukan nilai kosong di seluruh kolom. Tidak diperlukan imputasi data.

**2. Penanganan Outlier:**
Outlier terdeteksi menggunakan metode IQR (Interquartile Range). Nilai-nilai yang berada di luar rentang ```ruby Q1 - 1.5IQR dan Q3 + 1.5IQR``` dianggap sebagai outlier. Untuk menjaga integritas data, dilakukan teknik clipping agar nilai ekstrim tetap dalam batas wajar tanpa menghapus data.

**3. Encoding Data Kategorikal:**

Fitur ```ruby Soil``` dikonversi menjadi variabel dummy menggunakan teknik One-Hot Encoding karena bersifat kategorikal nominal.

Target ```ruby Crop``` dikodekan menggunakan Label Encoding agar bisa diproses oleh algoritma klasifikasi.

**4. Normalisasi Data:**
Seluruh fitur numerik (termasuk hasil encoding) dinormalisasi menggunakan StandardScaler dari Scikit-Learn untuk menyamakan skala dan mempercepat konvergensi model pembelajaran mesin.

**5. Pembagian Dataset:** 
Data dibagi menjadi dua subset:

* Training set (80%): Digunakan untuk melatih model.

* Testing set (20%): Digunakan untuk mengevaluasi performa model.
Pemisahan dilakukan dengan ```ruby train_test_split``` dengan ```ruuby random_state=42``` untuk replikasi hasil.

**6. Pemeriksaan Korelasi dan Multikolinearitas (Opsional):** 
Matriks korelasi divisualisasikan untuk memahami hubungan antar fitur. Tidak ditemukan korelasi multikolinear yang mengganggu model.

**7. Finalisasi Dataset:**  
Dataset yang telah melalui semua tahap di atas disimpan sebagai ```ruby X_train, X_test, y_train, dan y_test``` , dan siap digunakan untuk proses pelatihan model machine learning.

## Modeling
**Model yang Digunakan:** Random Forest Classifier

**Alasan Pemilihan:**
Random Forest adalah algoritma ensemble learning berbasis pohon keputusan yang menggabungkan banyak pohon (trees) untuk menghasilkan prediksi yang lebih stabil dan akurat. Algoritma ini sangat sesuai untuk data tabular dengan fitur numerik maupun kategorikal, serta memiliki keunggulan dalam:

* Menangani klasifikasi multi-kelas secara efisien (dalam kasus ini, 31 jenis tanaman).

* Tahan terhadap overfitting karena teknik bootstrapping dan pembagian fitur acak.

* Menangkap hubungan non-linear antar fitur lingkungan seperti pH, curah hujan, dan unsur hara.

**Arsitektur Model:**

* Jumlah Pohon (n_estimators):** 500 pohon keputusan dibangun untuk memperkuat kestabilan prediksi.

* **Bootstrapping:** Setiap pohon dilatih pada subset data acak dengan pengembalian.

* **Pembagian Fitur Acak (max_features='sqrt'):** Untuk memastikan keragaman antar pohon.

**1.** **Kriteria Pemisahan:** Gini impurity.

**2** **Pengaturan Reproducibility:** random_state=42.

**Proses Klasifikasi:**

* Setiap pohon memproses input fitur lingkungan (pH, N, P, suhu, kelembapan, dll).

* Hasil klasifikasi dari semua pohon dikombinasikan melalui voting mayoritas.

* Prediksi akhir diberikan berdasarkan hasil voting terbanyak.

Selain prediksi, Random Forest juga memberikan skor pentingnya tiap fitur terhadap hasil klasifikasi.
**Implementasi Kode:**
  
         ```ruby
         from sklearn.ensemble import RandomForestClassifier

         # Inisialisasi model
         model = RandomForestClassifier(
             n_estimators=500,
             random_state=42,
             criterion='gini'
         )
         
         # Pelatihan model
         model.fit(X_train, y_train)
         
         # Prediksi
         y_pred = model.predict(X_test)```

## Evaluation

**Metrik Evaluasi untuk Klasifikasi Multi-Kelas:**
1. Accuracy (Akurasi):

- Mengukur proporsi prediksi benar dari semua sampel

- Nilai: 0.96

- Interpretasi: Model mencapai akurasi 96%, menunjukkan 96 dari 100 rekomendasi tanaman tepat sesuai dengan kondisi lingkungan

2. Precision (Presisi):

- mengukur ketepatan prediksi untuk setiap kelas tanaman

- Rata-rata Makro: 0.97

- Interpretasi: Dari semua tanaman yang direkomendasikan model, 97% benar-benar sesuai dengan kondisi lahan

3. (Sensitifitas):

- Mengukur kemampuan model menemukan semua kemungkinan tanaman yang cocok

- Rata-rata Makro: 0.97

- Interpretasi: Model dapat mengidentifikasi 97% dari semua opsi tanaman yang sebenarnya cocok untuk kondisi tertentu

4. F1-Score:

- Rata-rata harmonik dari precision dan recall

- Rata-rata Makro: 0.96

- Interpretasi: Keseimbangan antara ketepatan dan kelengkapan rekomendasi sangat baik

##Analisis Performa per Kelas Tanaman
**Tanaman dengan Performa Terbaik:**
Rice (Padi):

Precision: 1.00, Recall: 1.00, F1: 1.00

Interpretasi: Rekomendasi padi selalu tepat sesuai kondisi lahan basah

Wheat (Gandum):

Precision: 0.97, Recall: 0.96, F1: 0.96

Interpretasi: Model sangat akurat merekomendasikan gandum untuk lahan kering

Tanaman dengan Tantangan:
Muskmelon (Melon):

Precision: 0.91, Recall: 0.83, F1: 0.87

Analisis: Kesalahan terjadi karena kemiripan dengan watermelon (semangka)

Solusi: Tambahkan fitur "Kebutuhan Sinar Matahari"

Watermelon (Semangka):

Precision: 0.91, Recall: 0.77, F1: 0.83

Analisis: Terkadang tertukar dengan muskmelon

Solusi: Tingkatkan data parameter kematangan buah

## Referensi 
- Food and Agriculture Organization. (2023). The State of Food and Agriculture 2023: Leveraging automation in agriculture for   transforming agrifood systems. https://www.fao.org

- Zhang, M., Li, M., Wang, X., & Wang, Q. (2020). Application of deep learning algorithms in agricultural image recognition: A review. Computers and Electronics in Agriculture, 175, 105467. https://doi.org/10.1016/j.compag.2020.105467

- World Bank. (2022). Transforming Agriculture for Shared Prosperity and Improved Livelihoods. https://www.worldbank.org

- McKinsey & Company. (2020). Agriculture’s connected future: How technology can yield new growth. https://www.mckinsey.com/industries/agriculture

