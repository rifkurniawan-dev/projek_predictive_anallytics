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
  - Pendekatan:
    - Dataset tidak memiliki nilai yang hilang berdasarkan pengecekan berikut:

    
       ```ruby
      data.isnull().sum()```
       
Hasil: Semua kolom memiliki nilai 0 untuk data kosong.

### 2. Handling Outlier

- Tujuan: Mengatasi nilai ekstrim yang dapat mempengaruhi kinerja model.
Pendekatan:
    - Metode IQR digunakan untuk mendeteksi dan cap nilai outlier.
      ```ruby
      def cap_outliers(df,column):
      Q1 = df[column].quantile(0.25)
      Q3 = df[column].quantile(0.75)
      IQR = Q3 - Q1
      lower_bound = Q1 - 1.5 * IQR
      upper_bound = Q3 + 1.5 * IQR
       df[column] = df[column].clip(lower=lower_bound, upper=upper_bound)
      
      for col in ['Temperature', 'Humidity','Rainfall','PH','Nitrogen', 'Phosphorous', 'Potassium','Carbon']:
      cap_outliers(data, col)```

### 3. Feature Engineering
- Tujuan: Menyederhanakan fitur kategorik dan menyiapkannya untuk pemodelan.

- Pendekatan:
    - One-hot encoding diterapkan pada fitur kategorik Soil:

      ```ruby
      data = pd.get_dummies(data, columns=['Soil'], dtype=np.float64)```

Nama kolom diubah agar lebih mudah dibaca:


      ```ruby
      soil_mapping = {
          'Soil_Acidic Soil': 'Acidic_Soil',
          'Soil_Alkaline Soil': 'Alkaline_Soil',
          'Soil_Loamy Soil': 'Loamy_Soil',
          'Soil_Neutral Soil': 'Neutral_Soil',
          'Soil_Peaty Soil': 'Peaty_Soil'
        }
        data.rename(columns=soil_mapping,inplace=True)```

### 4. Data Transformation
- Tujuan: Menstandarkan skala data agar model dapat belajar secara efisien dan optimal.

One-Hot Encoding pada fitur kategorikal Soil:


      ```ruby
      data = pd.get_dummies(data, columns=['Soil'], dtype=np.float64)
      Hasil: Kolom Soil diubah menjadi 5 kolom biner (Soil_Acidic Soil, Soil_Alkaline Soil, dll).```

Label Encoding pada target Crop:

      ```ruby
      label_encoder = LabelEncoder()
      data['crop'] = label_encoder.fit_transform(data['Crop'])
      Hasil: Kolom Crop diubah menjadi nilai numerik (crop).```

Scaling Fitur Numerik menggunakan StandardScaler:

      ```ruby
      scaler = StandardScaler()
      scaled_features = scaler.fit_transform(data[['Temperature','Humidity',...,'Peaty_Soil']])
      X_scaled = pd.DataFrame(scaled_features, columns=...)```

### 5. Splitting Data
- Tujuan: Membagi data menjadi data pelatihan dan pengujian untuk evaluasi model yang adil.
- Pendekatan:

   - Data time series harus dibagi dengan mempertahankan urutan waktu.
   - Data dibagi menjadi training set (80%) dan test set (20%):

      ```ruby
      X_train, X_test, y_train, y_test = train_test_split(X_scaled, y, test_size=0.2, random_state=42)```

- Bukti dimensi data:

Data awal: (3100, 15)

Setelah scaling: X_scaled.shape = (3100, 13)

Target: y.shape = (3100,)

Hasil splitting:

Training: 2480 sampel (80% dari 3100)

Test: 620 sampel (20% dari 3100)


### 6. Validasi Data
- Tujuan: Memastikan data bebas dari masalah setelah preprocessing.
- Pendekatan:
  - Cek kembali data setelah preprocessing untuk memastikan tidak ada nilai kosong, outlier, atau fitur yang hilang.
- Model Random Forest dilatih dan diuji:
    ```ruby
    model = RandomForestClassifier(random_state=42, n_estimators=500)
    model.fit(X_train, y_train)  # Pelatihan
    y_pred = model.predict(X_test)  # Prediksi```
Akurasi diukur pada test set:
      ```ruby
      accuracy = accuracy_score(y_test, y_pred)
      print(f"Accuracy: {accuracy:.2f}")  # Output: Accuracy: 0.96```

Confusion Matrix & Classification Report digunakan untuk evaluasi:
      ```ruby
      cm = confusion_matrix(y_test, y_pred)
      sns.heatmap(cm, annot=True, fmt='d')
      print(classification_report(y_test, y_pred))```

## Modeling
Pada tahap ini, model yang digunakan adalah Random Forest, sebuah algoritma ensemble learning berbasis pohon keputusan. Random Forest dipilih karena kemampuannya menangani data tabular dengan fitur heterogen (numerik dan kategorik), resisten terhadap overfitting, dan kemampuan menangkap hubungan non-linier antar parameter lingkungan. Model ini ideal untuk masalah klasifikasi multi-kelas dengan 31 jenis tanaman.

**Arsitektur Model Random Forest:**
Ensemble of Decision Trees:
- Dibangun 500 pohon keputusan (n_estimators=500)

- Setiap pohon dilatih pada subset data berbeda (bootstrapping)

- Pembagian fitur acak (max_features="sqrt") untuk diversifikasi

Proses Klasifikasi:

- Setiap pohon memproses fitur input (suhu, kelembaban, dll)

- Dilakukan voting mayoritas dari semua pohon untuk prediksi akhir

- Mengukur importance tiap fitur dalam keputusan klasifikasi

Optimasi Hyperparameter:

- random_state=42 untuk hasil yang reproducible

- Kriteria pemisahan: Gini impurity

- Kedalaman pohon dioptimalkan secara otomatis
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
    model.fit(X_train, y_train)```

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

