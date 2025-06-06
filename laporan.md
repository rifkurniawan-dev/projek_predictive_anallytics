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
* Menganalisis data dengan melakukan analisis univariat dan analisis multivariat. Memahami data juga dapat dilakukan dengan visualisasi. Memahami data dapat membantu untuk mengetahui kolerasi matriks antar fitur dan mendeteksi outlier 
* Melakukan proses pembersihan data dan normalisai data agar mendapat prediksi yang baik.
* Membuat beberapa variasi model untuk mendapatkan model yang paling baik dari beberapa model yang telah dibuat untuk prediksi kualitas apel. Diantaranya adalah menggunakan:
 1. K-Nearest Neighbor (KNN) adalah algoritma sederhana yang mengklasifikasikan data atau kasus baru berdasarkan ukuran kesamaan. Hal ini sebagian besar digunakan untuk mengklasifikasikan titik data berdasarkan tetangga terdekatnya sebagai acuan (Subramanian, D. 2019).
 2. Random Forest adalah algoritma machine learning yang kuat yang dapat digunakan untuk berbagai tugas termasuk regresi dan klasifikasi. Ini adalah metode ensemble, yang berarti bahwa model random forest terdiri dari banyak decision tree kecil, yang disebut estimator, yang masing-masing menghasilkan prediksi mereka sendiri. Random forest menggabungkan prediksi estimator untuk menghasilkan prediksi yang lebih akurat  (Wood, n.d.). 

## Data Understanding
Dataset yang digunakan berasal dari Kaggle (https://www.kaggle.com/datasets/nishchalchandel/crop-recommendation), terdiri dari 3100 baris dan 10 kolom. Dataset ini mencakup berbagai parameter penting untuk menentukan jenis tanaman yang optimal berdasarkan kondisi lingkungan.
Selain itu, berdasarkan hasil pemeriksaan di notebook, diketahui bahwa dataset tidak memiliki missing value maupun data duplikat, sehingga tahap pembersihan data tidak diperlukan.
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

  
Dalam proyek ini, tidak ditemukan adanya data duplikat maupun missing value berdasarkan hasil pemeriksaan awal menggunakan ```df.duplicated().sum()``` yang menunjukkan bahwa tidak terdapat baris duplikat. Pemeriksaan terhadap missing value juga dilakukan menggunakan ```df.isnull().sum()```dan hasilnya menunjukkan bahwa seluruh fitur memiliki nilai lengkap, sehingga tidak dilakukan proses penghapusan atau imputasi data. jumlah Dataset yang digunakan terdiri dari 3100 baris dan 10 kolom. Jumlah data ini tetap terjaga sebelum dan sesudah penanganan outlier karena metode yang digunakan tidak menghapus baris data. penanganan outlier dilakukan menggunakan metode **Interquartile Range (IQR)**, yang dihitung dengan mengurangkan kuartil ketiga (Q3) dengan kuartil pertama (Q1), sesuai dengan rumus:
## IQR = Q3 - Q1,
di mana Q1 merupakan kuartil pertama dan Q3 merupakan kuartil ketiga. Dengan menggunakan nilai IQR ini, batas bawah dan batas atas untuk deteksi outlier dapat ditentukan. Setelah melakukan proses penanganan outlier, jumlah data tetap sebanyak 3100 baris, karena nilai-nilai yang dianggap outlier tidak dihapus melainkan disesuaikan agar berada dalam batas yang telah ditentukan. Selanjutnya, dataset dibagi menjadi **data latih dan data uji** menggunakan fungsi ```train_test_split``` dari library ```sklearn.model_selection```, dengan ```rasio pembagian 80% data latih dan 20% data uji```, serta ```random_state``` disetel ke 60 untuk menjaga reprodusibilitas. Proyek ini juga menerapkan normalisasi data menggunakan ```MinMaxScaler``` dari ```sklearn.preprocessing```, yang bertujuan untuk menyamakan skala semua fitur. Seluruh tahapan ini dilakukan untuk memastikan bahwa model yang dibangun memiliki performa optimal.
Selain itu, dilakukan proses Label Encoding pada kolom ```Soil``` dan ```Crop``` untuk mengubah nilai kategorikal menjadi numerik. Setelah proses encoding selesai, kolom asli ```Soil``` dan ```Crop``` dihapus dari dataset untuk menghindari redundansi informasi


## Modeling
**1. K-Nearest Neighbors (KNN)**
KNN adalah salah satu algoritma machine learning yang sederhana namun efektif, digunakan baik untuk klasifikasi maupun regresi. Cara kerjanya adalah dengan mencari sejumlah k data terdekat dari data yang ingin diprediksi, lalu menentukan kelas berdasarkan mayoritas label tetangga terdekat tersebut. Dalam proyek ini digunakan parameter:

* ```n_neighbors=5``` : menetapkan jumlah tetangga terdekat yang digunakan untuk prediksi.

* ```weights='distance'``` : memberi bobot lebih tinggi kepada tetangga yang lebih dekat ke data baru.

Kelebihan:

* Dapat diterapkan pada berbagai jenis masalah klasifikasi dan regresi.

* Implementasinya cukup mudah dan intuitif.

Kekurangan:

* Cukup sensitif terhadap nilai ekstrem (outlier).

* Membutuhkan sumber daya komputasi yang besar saat menangani data skala besar.

* Menentukan nilai k terbaik memerlukan proses tuning yang tidak sederhana.

**2. Random Forest**
Random Forest merupakan algoritma ensemble yang terdiri dari sekumpulan pohon keputusan yang dibentuk secara acak. Prediksi akhir diperoleh melalui mekanisme voting dari setiap pohon yang dibuat. Dalam proyek ini, parameter yang digunakan adalah:

* ```max_depth=42``` : menentukan batas kedalaman maksimum pohon keputusan.

Kelebihan:

* Memiliki akurasi yang tinggi dalam berbagai kasus.

* Tangguh saat menghadapi dataset berdimensi tinggi (banyak fitur).

* Lebih tahan terhadap outlier dibanding algoritma lain.

Kekurangan:

* Risiko overfitting cukup tinggi pada data yang kecil.

* Proses pelatihannya bisa memakan waktu cukup lama.

* Hasil prediksi sulit dijelaskan secara interpretatif.


## Evaluation

Pada tahap evaluasi model, metrik yang digunakan adalah akurasi, yaitu ukuran yang menunjukkan seberapa banyak prediksi model yang benar dibandingkan dengan seluruh jumlah prediksi yang dilakukan. Akurasi dihitung dengan rumus sebagai berikut:

Akurasi = (TP + TN) / (TP + TN + FP + FN) × 100%

Penjelasan komponen:

* TP (True Positive): Jumlah data berlabel positif yang berhasil diprediksi dengan benar sebagai positif.

* TN (True Negative): Jumlah data berlabel negatif yang berhasil diprediksi dengan benar sebagai negatif.

* FP (False Positive): Jumlah data negatif yang secara keliru diprediksi sebagai positif (disebut juga kesalahan tipe I).

* FN (False Negative): Jumlah data positif yang secara keliru diprediksi sebagai negatif (disebut juga kesalahan tipe II).

Secara sederhana, rumus ini membandingkan jumlah prediksi yang tepat (TP dan TN) dengan total seluruh data yang diuji. Hasil akhirnya dikalikan 100% untuk mendapatkan nilai dalam bentuk persentase, sehingga lebih mudah dipahami sebagai tingkat ketepatan model.
Berikut hasil akurasi 2  buah model yang dilatih:

----------------------------------------------------------------------
         Model        Accuracy    Precision    Recall      F1 Score
----------------------------------------------------------------------
0.             KNN         0.75        0.76        0.75          0.74
-----------------------------------------------------------------------
1.     Random Forest        0.96        0.97        0.96           0.96
------------------------------------------------------------------------




Tabel 3. Hasil Akurasi

![image](https://github.com/user-attachments/assets/a952ccf3-67b6-454b-a434-cd6e3711d59f)

Gambar 3. Model Akurasi Visualisasi

Dilihat dari Gambar 3.  visualisasi akurasi model menunjukkan perbandingan performa antara dua algoritma yang digunakan, yaitu K-Nearest Neighbors (KNN) dan Random Forest. Terlihat bahwa Random Forest memiliki akurasi lebih tinggi, sekitar 96%, sedangkan KNN berada di kisaran 75%.

Meskipun Random Forest menunjukkan akurasi yang lebih unggul, KNN dipilih sebagai model akhir dalam proyek ini karena beberapa pertimbangan praktis:

* KNN merupakan algoritma yang sederhana dan mudah dipahami, sehingga memudahkan implementasi dan pengembangan.

* KNN memerlukan sedikit tuning parameter dibandingkan Random Forest yang biasanya membutuhkan proses hyperparameter tuning yang kompleks.

* Untuk skala data kecil hingga menengah seperti pada proyek ini, KNN cukup efisien dan memberikan hasil yang memadai.

Dengan menggunakan model KNN, sistem rekomendasi tanaman dapat membantu petani dalam membuat keputusan berbasis data yang lebih objektif dan akurat, mengurangi ketergantungan pada intuisi atau pengalaman subjektif. Meskipun akurasi KNN sedikit lebih rendah, model ini tetap dapat memberikan rekomendasi yang relevan dan berguna dalam meningkatkan hasil pertanian secara konsisten. Oleh karena itu, KNN dianggap lebih sesuai untuk kebutuhan implementasi cepat dan pengembangan ringan dalam proyek ini.


## Referensi 
- Food and Agriculture Organization. (2023). The State of Food and Agriculture 2023: Leveraging automation in agriculture for   transforming agrifood systems. https://www.fao.org

- Zhang, M., Li, M., Wang, X., & Wang, Q. (2020). Application of deep learning algorithms in agricultural image recognition: A review. Computers and Electronics in Agriculture, 175, 105467. https://doi.org/10.1016/j.compag.2020.105467

- World Bank. (2022). Transforming Agriculture for Shared Prosperity and Improved Livelihoods. https://www.worldbank.org

- McKinsey & Company. (2020). Agriculture’s connected future: How technology can yield new growth. https://www.mckinsey.com/industries/agriculture
- Subramanian, D. (2019). Pengantar Sederhana tentang Algoritma K-Nearest Neighbors. Menuju Ilmu Data. https://towardsdatascience.com/a-simple-introduction-to-k-nearest-neighbors-algorithm-b3519ed98e
- Wood, T. - Apa itu Random Forest?. DeepAI. https://deepai.org/machine-learning-glossary-and-terms/random-forest

