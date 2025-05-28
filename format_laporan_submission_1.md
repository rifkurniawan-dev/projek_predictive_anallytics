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

Pendekatan:
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

