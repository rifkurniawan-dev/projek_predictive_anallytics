# Laporan Proyek Machine Learning - Arif Kurniawan

## Domain Proyek

Dalam menghadapi tantangan global seperti perubahan iklim, degradasi tanah, dan pertumbuhan populasi, sektor pertanian dituntut untuk menjadi lebih efisien dan berkelanjutan. Menurut laporan FAO (2021), ketahanan pangan dunia sangat bergantung pada kemampuan petani dalam mengelola sumber daya alam secara optimal dan adaptif. Teknologi seperti Artificial Intelligence (AI) menjadi solusi potensial dalam mendukung pertanian cerdas (smart agriculture) karena mampu memberikan wawasan berbasis data untuk pengambilan keputusan yang lebih tepat.

Masalah utama yang dihadapi petani, khususnya di daerah terpencil, adalah minimnya akses terhadap informasi terkait kondisi tanah dan iklim yang memengaruhi hasil panen. Dengan adanya sistem berbasis AI yang menganalisis parameter seperti tingkat pH tanah, kelembapan, suhu, serta curah hujan, petani dapat diberikan rekomendasi pemupukan, waktu tanam, dan jenis tanaman yang sesuai.
## Business Understanding


### Problem Statements

1. Petani kesulitan mendapatkan rekomendasi berbasis data mengenai kesesuaian tanah dan iklim terhadap jenis tanaman.

2. Keputusan pertanian seringkali berdasarkan intuisi tanpa mempertimbangkan parameter penting seperti pH tanah, suhu, dan kelembapan.

3. Tidak ada sistem terintegrasi yang menyediakan wawasan prediktif bagi petani tentang kondisi pertanian mereka.
### Goals

1. Mengembangkan sistem rekomendasi tanaman berbasis AI dengan memanfaatkan parameter tanah dan iklim.

2. Memberikan wawasan real-time dan prediktif yang membantu petani meningkatkan produktivitas secara berkelanjutan.

3. Meningkatkan efisiensi dalam penggunaan pupuk, air, dan lahan pertanian.

### Solution statements
1. Membangun model klasifikasi AI untuk merekomendasikan jenis tanaman berdasarkan data parameter tanah dan iklim.
2. Menerapkan dua model pembanding seperti Random Forest dan XGBoost, serta membandingkan performanya menggunakan metrik akurasi, precision, recall, dan F1-score.
3. Menyediakan dashboard interaktif yang menampilkan prediksi dan insight berbasis data menggunakan Streamlit.



## Data Understanding
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

**Rubrik/Kriteria Tambahan (Opsional)**:
- Melakukan beberapa tahapan yang diperlukan untuk memahami data, contohnya teknik visualisasi data atau exploratory data analysis.

## Data Preparation
### 1. Handling Missing Values
- Tujuan: Menghindari nilai kosong yang dapat mengganggu pelatihan model.
Pendekatan:
    - Dataset tidak memiliki nilai yang hilang berdasarkan pengecekan berikut:
       ```ruby
      data.isnull().sum()   ```

### 2. Handling Outlier
- Tujuan: Mengatasi nilai yang terlalu ekstrem yang dapat memengaruhi performa model.
- Pendekatan:
  - Z-Score: Menghapus data dengan Z-Score di luar ambang batas (misalnya, |Z| > 3).
  - IQR (Interquartile Range): Menghapus nilai di luar rentang [Q1 = -1.5, Q3 = 1.5].
- Implementasi Code :

   ```ruby
   # Menggunakan IQR untuk menangani outlier
  Q1 = df['close_XRP'].quantile(0.25)  
  Q3 = df['close_XRP'].quantile(0.75)
  IQR = Q3 - Q1

  # Hapus outlier dari XRP
  df = df[~((df['close_XRP'] < (Q1 - 1.5 * IQR)) | (df['close_XRP'] > (Q3 + 1.5 * IQR)))]
   ```

### 3. Feature Engineering
- Tujuan: Membuat fitur baru yang relevan dan dapat membantu model memahami pola dalam data.
- Pendekatan:
  - Technical Indicators: Membuat indikator teknikal seperti moving average (SMA), exponential moving average (EMA), atau relative strength index (RSI).
  - Lag Features: Menambahkan kolom harga sebelumnya sebagai prediktor.
- Implementasi dalam project : Moving Average (7 hari)
  ```ruby
  df['SMA_7_XRP'] = df['close_XRP'].rolling(window=7).mean()
  df['SMA_7_USDT'] = df['close_USDT'].rolling(window=7).mean()
  ```

### 4. Data Transformation
- Tujuan: Mengubah skala data agar lebih sesuai untuk algoritma machine learning.
- Pendekatan:
  - Normalisasi Data: Data harga dinormalisasi menggunakan MinMaxScaler agar semua nilai berada dalam rentang [0, 1]. Hal ini membantu model LSTM untuk lebih cepat melakukan konvergensi selama pelatihan.
  - Min-Max Scaling: Mengubah skala data ke rentang [0, 1], digunakan untuk LSTM.
  - Standardization: Mengubah data menjadi distribusi dengan mean 0 dan standar deviasi 1.
- Langkah Implementasi dalam project:
  ```ruby
   from sklearn.preprocessing import MinMaxScaler  
   scaler = MinMaxScaler()
   df['Close_XRP_Normalized'] = scaler.fit_transform(df[['Close_XRP']])
   df['Close_USDT_Normalized'] = scaler.fit_transform(df[['Close_USDT']])
  ```

### 5. Splitting Data
- Tujuan: Membagi data menjadi data pelatihan dan pengujian untuk evaluasi model yang adil.
- Pendekatan:
   - Data dibagi dalam rasio 80:20 untuk pelatihan dan pengujian.
   - Data time series harus dibagi dengan mempertahankan urutan waktu.
   ```ruby
   # Split data menjadi train-test
   train_size = int(len(X_xrp) * 0.8)
   X_xrp_train, X_xrp_test = X_xrp[:train_size], X_xrp[train_size:]
   y_xrp_train, y_xrp_test = y_xrp[:train_size], y_xrp[train_size:] 
   X_usdt_train, X_usdt_test = X_usdt[:train_size], X_usdt[train_size:]
   y_usdt_train, y_usdt_test = y_usdt[:train_size], y_usdt[train_size:]
   ```

### 6. Validasi Data
- Tujuan: Memastikan data bebas dari masalah setelah preprocessing.
- Pendekatan:
  - Cek kembali data setelah preprocessing untuk memastikan tidak ada nilai kosong, outlier, atau fitur yang hilang.
- Langkah Implementasi:
  ```ruby
  # Periksa apakah ada nilai kosong
  print(df.isnull().sum())
  
  # Periksa dimensi data
  print(df.shape)
  ```

## Modeling
Tahapan ini membahas mengenai model machine learning yang digunakan untuk menyelesaikan permasalahan. Anda perlu menjelaskan tahapan dan parameter yang digunakan pada proses pemodelan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan kelebihan dan kekurangan dari setiap algoritma yang digunakan.
- Jika menggunakan satu algoritma pada solution statement, lakukan proses improvement terhadap model dengan hyperparameter tuning. **Jelaskan proses improvement yang dilakukan**.
- Jika menggunakan dua atau lebih algoritma pada solution statement, maka pilih model terbaik sebagai solusi. **Jelaskan mengapa memilih model tersebut sebagai model terbaik**.

## Evaluation
Pada bagian ini anda perlu menyebutkan metrik evaluasi yang digunakan. Lalu anda perlu menjelaskan hasil proyek berdasarkan metrik evaluasi yang digunakan.

Sebagai contoh, Anda memiih kasus klasifikasi dan menggunakan metrik **akurasi, precision, recall, dan F1 score**. Jelaskan mengenai beberapa hal berikut:
- Penjelasan mengenai metrik yang digunakan
- Menjelaskan hasil proyek berdasarkan metrik evaluasi

Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.

**---Ini adalah bagian akhir laporan---**

_Catatan:_
- _Anda dapat menambahkan gambar, kode, atau tabel ke dalam laporan jika diperlukan. Temukan caranya pada contoh dokumen markdown di situs editor [Dillinger](https://dillinger.io/), [Github Guides: Mastering markdown](https://guides.github.com/features/mastering-markdown/), atau sumber lain di internet. Semangat!_
- Jika terdapat penjelasan yang harus menyertakan code snippet, tuliskan dengan sewajarnya. Tidak perlu menuliskan keseluruhan kode project, cukup bagian yang ingin dijelaskan saja.

