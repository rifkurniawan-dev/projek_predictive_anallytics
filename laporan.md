# Laporan Proyek Machine Learning - Arif Kurniawan

## Domain Proyek


Dalam menghadapi tantangan global seperti perubahan iklim, degradasi tanah, dan pertumbuhan populasi, sektor pertanian dituntut untuk menjadi lebih efisien dan berkelanjutan. Menurut laporan FAO (2021), ketahanan pangan dunia sangat bergantung pada kemampuan petani dalam mengelola sumber daya alam secara optimal dan adaptif. Teknologi seperti Artificial Intelligence (AI) menjadi solusi potensial dalam mendukung pertanian cerdas (smart farming) karena mampu memberikan wawasan berbasis data untuk pengambilan keputusan yang lebih tepat.

Permasalahan utama yang dihadapi petani, khususnya di daerah terpencil, adalah minimnya akses terhadap informasi terkait kondisi tanah dan iklim yang mempengaruhi hasil panen. Dengan adanya sistem berbasis AI yang menganalisis parameter seperti tingkat pH tanah, kelembapan, suhu, serta curah hujan, petani dapat diberikan rekomendasi pemupukan, waktu tanam, dan jenis tanaman yang sesuai.

## Business Understanding
Sektor pertanian perlu menjadi lebih efisien dan berkelanjutan untuk menghadapi tantangan seperti perubahan iklim dan degradasi tanah. Teknologi AI berperan penting dalam mendukung pertanian cerdas dengan memberikan analisis berbasis data. Melalui sistem AI yang memanfaatkan parameter seperti pH tanah, kelembapan, suhu, dan curah hujan, petani dapat memperoleh rekomendasi yang akurat terkait pemupukan, waktu tanam, dan jenis tanaman, sehingga hasil panen dapat ditingkatkan secara optimal.

### Problem Statements

1. Petani kesulitan mendapatkan rekomendasi berdasarkan data mengenai kesesuaian tanah dan iklim terhadap jenis tanaman.

2. Keputusan pertanian seringkali berdasarkan intuisi tanpa mempertimbangkan parameter penting seperti pH tanah, suhu, dan kelembaban.

3. Tidak ada sistem terintegrasi yang menyediakan wawasan prediktif bagi petani tentang kondisi pertanian mereka.
### Goals

* Membangun sistem rekomendasi tanaman berbasis AI dengan memanfaatkan parameter tanah dan iklim.

* Memberikan wawasan real-time dan prediktif yang membantu petani meningkatkan produktivitas secara berkelanjutan.

* Meningkatkan efisiensi dalam penggunaan pupuk, air, dan lahan pertanian.

### Solution statements
* Menganalisis data dengan melakukan analisis univariat dan analisis multivariat. Memahami data juga dapat dilakukan dengan visualisasi. Memahami data dapat membantu untuk mengetahui kolerasi matriks antar fitur dan mendeteksi outlier.
* Melakukan proses pembersihan data dan normalisai data agar mendapat prediksi yang baik.
* Membuat beberapa variasi model untuk mendapatkan model yang paling baik dari beberapa model yang telah dibuat untuk prediksi kualitas apel. Diantaranya adalah menggunakan:
 1. K-Nearest Neighbor (KNN) adalah algoritma sederhana yang mengklasifikasikan data atau kasus baru berdasarkan ukuran kesamaan. Hal ini sebagian besar digunakan untuk mengklasifikasikan titik data berdasarkan tetangga terdekatnya sebagai acuan.
 2. Random Forest adalah algoritma machine learning yang kuat yang dapat digunakan untuk berbagai tugas termasuk regresi dan klasifikasi. Ini adalah metode ensemble, yang berarti bahwa model random forest terdiri dari banyak decision tree kecil, yang disebut estimator, yang masing-masing menghasilkan prediksi mereka sendiri. Random forest menggabungkan prediksi estimator untuk menghasilkan prediksi yang lebih akurat .
 3. Naive Bayes adalah model machine learning probabilistik yang digunakan untuk tugas klasifikasi. Inti dari classifier ini didasarkan pada teorema Bayes.
## Data Understanding
Dataset yang digunakan berasal dari Kaggle (https://www.kaggle.com/datasets/nishchalchandel/crop-recommendation), terdiri dari 3100 baris dan 10 kolom. Dataset ini mencakup berbagai parameter penting untuk menentukan jenis tanaman yang optimal berdasarkan kondisi lingkungan.

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

### Univariate Analysis
Gambar 1a. Analisis Univariat (Data Kategori)
![image](https://github.com/user-attachments/assets/d1c88f72-a2b7-47c8-87ed-d2b2c3b89855)



**Berdasarkan Gambar 1a**, dapat dilihat bahwa distribusi data pada dataset klasifikasi tanaman terdiri dari variabel numerik dan kategorikal yang memiliki karakteristik sebagai berikut:

**1. Variabel Kategorikal**
* Soil_encoded:
* Variabel ini merupakan hasil encoding dari jenis tanah. Terlihat bahwa:

* Kategori ```0``` merupakan yang paling dominan, dengan jumlah data lebih dari **1500**.

* Kategori lainnya (1 hingga 4) memiliki jumlah data yang jauh lebih sedikit, berkisar antara kurang dari **100 hingga 600 data.**

Hal ini menunjukkan bahwa jenis tanah dengan label 0 paling sering dijumpai, sementara jenis tanah lainnya tidak tersebar merata. Ketidakseimbangan ini dapat menjadi pertimbangan saat proses pelatihan model, misalnya dengan melakukan penyesuaian bobot (class weighting) atau sampling ulang.

**2. Variabel Numerik**

**Temperature:**
Data suhu menunjukkan distribusi yang mendekati normal (bell-shaped curve) dengan rentang nilai antara 13 hingga 40°C. Nilai terbanyak berada di kisaran 25–30°C, yang mengindikasikan suhu rata-rata yang optimal bagi pertumbuhan tanaman.

**Humidity:**
Distribusi kelembaban bersifat multimodal, dengan beberapa puncak. Nilai umum berada di kisaran 60–90%, namun terdapat kelompok data dengan kelembaban sangat rendah (<20%). Ini menunjukkan variasi lingkungan tempat data dikumpulkan sangat beragam.

**Rainfall:**
Distribusi data curah hujan menunjukkan positif skew (condong ke kanan). Sebagian besar data memiliki curah hujan antara 40–150 mm, dengan nilai maksimum yang melebihi 250 mm. Data ini memperlihatkan bahwa sebagian besar area memiliki curah hujan sedang hingga tinggi.

**pH:**
Nilai pH tanah berkisar antara 4 hingga 8.5, dengan konsentrasi tertinggi pada pH 6.0–7.0. Ini menandakan bahwa sebagian besar tanah tergolong netral hingga sedikit asam, yang umumnya sesuai untuk sebagian besar jenis tanaman.

**Nitrogen:**
Sebagian besar nilai nitrogen berkisar antara 55–65 ppm, dengan sebaran yang tidak simetris (negatively skewed). Terdapat beberapa data dengan kadar nitrogen tinggi hingga 80 ppm, yang mungkin berasal dari tanah dengan tingkat pemupukan tinggi.

**Phosphorous:**
Nilai fosfor dalam tanah menunjukkan distribusi positif skew. Mayoritas data berada di bawah 60 ppm, namun terdapat outlier dengan nilai hingga 135 ppm, yang dapat mempengaruhi analisis jika tidak ditangani dengan baik.

**Potassium:**
Data potasium memiliki pola distribusi yang mirip dengan fosfor. Nilai umum berada di 45–70 ppm, dan terdapat lonjakan pada nilai maksimal di sekitar 110 ppm, yang mungkin disebabkan oleh praktik pertanian tertentu.

**Carbon:**
Distribusi karbon terlihat merata (uniform) di seluruh rentang nilai 0.5–2.5%. Ini menandakan bahwa kandungan karbon organik dalam tanah bervariasi secara seimbang dan tidak terpusat pada nilai tertentu.

**3. Normalisasi Data**
Data numerik pada dataset ini belum dinormalisasi, terlihat dari perbedaan skala antar fitur. Misalnya:

* Temperature dalam derajat Celsius,

* Nutrien seperti nitrogen dan potassium dalam ppm,

* pH dalam skala 0–14,

* dan karbon dalam persen.

Untuk model machine learning yang sensitif terhadap skala (seperti KNN atau SVM), diperlukan normalisasi seperti:

* **Min-Max Scaling:** Mengubah semua nilai ke skala [0,1].

* **Z-Score Standardization:** Mengubah data menjadi distribusi dengan rata-rata 0 dan standar deviasi 1.

## Analisis Multivariat**
![image](https://github.com/user-attachments/assets/dc961b24-0358-4015-8404-b5312df3cd74)
**Gambar 2a. Analisis Multivariat**

![image](https://github.com/user-attachments/assets/294af004-d487-4aa4-88fe-a604a2cc9beb)

Gambar 2b. Analisis Matriks Korelasi
Berdasarkan Gambar 2a yang menunjukkan Matriks Korelasi antar fitur numerik, dapat diketahui bahwa sebagian besar fitur memiliki korelasi yang rendah hingga sedang satu sama lain. Korelasi tertinggi terlihat antara Nitrogen dan Rainfall (0.87) serta Phosphorous dan Potassium (0.81), yang menunjukkan hubungan linier yang kuat positif — artinya, ketika nilai Nitrogen atau Phosphorous meningkat, nilai Rainfall dan Potassium cenderung ikut meningkat. Selain itu, PH juga menunjukkan korelasi sedang dengan Phosphorous (0.48) dan Potassium (0.42). Di sisi lain, fitur seperti Carbon dan Temperature memiliki korelasi yang sangat rendah terhadap fitur lainnya, menunjukkan bahwa variabel-variabel ini relatif independen. Korelasi rendah ini mengindikasikan bahwa sebagian besar fitur memberikan informasi unik, yang baik untuk proses pembelajaran model karena dapat mengurangi kemungkinan multikolinearitas.

## Data Preparation
Dalam tahap persiapan data, terdapat beberapa aktivitas utama yang meliputi pengumpulan data (Data Gathering), evaluasi data (Data Assessing), serta pembersihan data (Data Cleaning). Pada tahap pengumpulan data, proses impor dilakukan dengan penyesuaian agar data dapat diolah dengan optimal dalam bentuk DataFrame menggunakan Pandas. Sedangkan pada tahap penilaian data, dilakukan beberapa pengecekan penting untuk memastikan kualitas data sebelum digunakan dalam proses analisis lebih lanjut.

* Memeriksa adanya data duplikat (baris yang sama persis).

* Mengecek nilai yang hilang (missing values) dalam dataset.

* Mengidentifikasi outlier atau nilai-nilai yang menyimpang jauh dari pola umum data.

Pada proses Data Cleaning yang dilakukan adalah seperti: 
* Mengonversi tipe data kolom agar sesuai dengan kebutuhan analisis (misalnya dari string ke numerik).

* Melakukan train-test split, yaitu membagi dataset menjadi data latih dan data uji.

* Menerapkan normalisasi, yaitu menyesuaikan skala fitur agar memiliki rentang nilai yang sebanding, guna meningkatkan performa model machine learning.


**1. Encoding Data Kategorikal:**
* Fitur ```Soil``` dikonversi menjadi variabel dummy menggunakan One-Hot Encoding karena termasuk kategori nominal.
Setelah encoding, nama kolom hasil One-Hot Encoding diubah (rename) untuk menyederhanakan dan memperjelas nama fitur, misalnya ```Soil_Acidic Soil``` menjadi ```Acidic_Soil```.
Selain itu, target ```Crop``` juga dikodekan menggunakan Label Encoding agar bisa diproses oleh algoritma klasifikasi.

**2. Penanganan Outlier:**
Outlier terdeteksi menggunakan metode IQR (Interquartile Range). Nilai-nilai yang berada di luar rentang ```Q1 - 1.5IQR dan Q3 + 1.5IQR``` dianggap sebagai outlier. Untuk menjaga integritas data, dilakukan teknik *clipping* agar nilai ekstrim tetap dalam batas wajar tanpa menghapus data.

**3. Normalisasi Data:**
Seluruh fitur numerik (termasuk hasil encoding) dinormalisasi menggunakan ```StandardScaler``` dari Scikit-Learn untuk menyamakan skala dan mempercepat konvergensi model pembelajaran mesin.

**4. Pemeriksaan Missing Value dan Duplikasi:**
Dataset diperiksa dengan ```.isnull().sum()``` dan ```.duplicated()``` untuk memastikan tidak ada data kosong atau duplikat.

**5. Pembagian Dataset:** 
Data dibagi menjadi dua subset:

* Training set (80%): Digunakan untuk melatih model.

* Testing set (20%): Digunakan untuk mengevaluasi performa model.
Pemisahan dilakukan dengan ```train_test_split``` dengan ```random_state=42``` untuk replikasi hasil.

**6. Pemeriksaan Korelasi dan Multikolinearitas (Opsional):** 
Matriks korelasi divisualisasikan untuk memahami hubungan antar fitur. Tidak ditemukan korelasi multikolinear yang mengganggu model.

**7. Finalisasi Dataset:**  
Dataset yang telah melalui semua tahap di atas disimpan sebagai ```X_train```, ```X_test```, ```y_train```, dan ```y_test``` , dan siap digunakan untuk proses pelatihan model machine learning.

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
  
         ```
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

**Metrik Evaluasi:**
Evaluasi model dilakukan menggunakan accuracy, precision, recall, F1-score, dan confusion matrix.

**Hasil Akurasi:**

Model Random Forest mencapai akurasi 96% pada data uji (620 sampel). Ini menunjukkan model mampu mengklasifikasikan jenis tanaman berdasarkan parameter tanah dan iklim dengan sangat baik.

**Confusion Matrix:**

Visualisasi confusion matrix menunjukkan bahwa sebagian besar kelas tanaman terprediksi dengan benar. Hampir semua kelas memiliki diagonal yang dominan, menandakan prediksi akurat.

**Classification Report:**

Berdasarkan hasil classification report:

* Precision rata-rata (macro avg): 0.97

* Recall rata-rata (macro avg): 0.97

* F1-score rata-rata (macro avg): 0.96

**Akurasi total: 0.96**

Sebagian besar kelas (jenis tanaman) memiliki nilai F1-score ≥ 0.94, menandakan performa klasifikasi yang konsisten di hampir semua kategori tanaman.

**Analisis Tambahan:**

Beberapa kelas seperti tanaman ke-21 (index 30) memiliki F1-score sedikit lebih rendah (0.83), menandakan kemungkinan perlu data lebih banyak pada kategori tersebut.

Tidak ada tanda overfitting karena model juga menunjukkan performa baik di data uji.

**Kesimpulan Evaluasi:**
Model Random Forest menunjukkan performa yang sangat baik, unggul dalam klasifikasi multi-kelas dengan data agrikultur. Model ini cocok digunakan untuk sistem rekomendasi tanaman yang andal.

## Referensi 
- Food and Agriculture Organization. (2023). The State of Food and Agriculture 2023: Leveraging automation in agriculture for   transforming agrifood systems. https://www.fao.org

- Zhang, M., Li, M., Wang, X., & Wang, Q. (2020). Application of deep learning algorithms in agricultural image recognition: A review. Computers and Electronics in Agriculture, 175, 105467. https://doi.org/10.1016/j.compag.2020.105467

- World Bank. (2022). Transforming Agriculture for Shared Prosperity and Improved Livelihoods. https://www.worldbank.org

- McKinsey & Company. (2020). Agriculture’s connected future: How technology can yield new growth. https://www.mckinsey.com/industries/agriculture

