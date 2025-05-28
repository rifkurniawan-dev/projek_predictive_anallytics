# Laporan Proyek Machine Learning - Arif Kurniawan

## Domain Proyek

Pertanian merupakan tulang punggung perekonomian di banyak negara berkembang, termasuk Indonesia. Namun, sektor ini menghadapi tantangan besar yang mengancam produktivitas dan keberlanjutan, seperti perubahan iklim, degradasi tanah, ketergantungan pada metode tradisional, serta keterbatasan akses terhadap data dan teknologi oleh petani.

Salah satu tantangan utama adalah rendahnya kemampuan petani dalam mengambil keputusan berdasarkan kondisi tanah dan iklim yang dinamis. Keputusan terkait waktu tanam, jenis tanaman, serta penggunaan pupuk dan irigasi sering kali dilakukan secara tradisional tanpa dukungan data, yang dapat mengarah pada hasil panen yang tidak optimal atau bahkan gagal panen.

Di sisi lain, perkembangan teknologi Artificial Intelligence (AI) membuka peluang besar untuk melakukan transformasi digital di sektor pertanian. Dengan memanfaatkan data tanah dan iklim serta model AI, petani dapat dibekali dengan wawasan preskriptif yang akurat untuk mendukung pertanian cerdas dan berkelanjutan.

## Permasalahan
Permasalahan utama dalam proyek ini adalah minimnya informasi yang dapat diakses oleh petani secara real-time terkait kondisi tanah dan iklim, yang berdampak pada:

1. Keputusan tanam yang tidak akurat

2. Overuse atau underuse pupuk dan air

3. Tidak terdeteksinya risiko gagal panen akibat perubahan cuaca ekstrim

4. Rendahnya produktivitas pertanian dan kerugian ekonomi

Jika tidak ditangani, hal ini akan memperburuk krisis pangan, memperbesar kerentanan petani kecil, serta menghambat upaya pembangunan berkelanjutan di sektor pertanian.

##Ide dan Tujuan Proyek

Ide dari proyek ini adalah membangun sistem prediksi dan rekomendasi pertanian berbasis AI dengan menganalisis data parameter tanah dan iklim, seperti:

1. Suhu harian

2. Curah hujan

3. Kelembaban tanah dan udara

4. Durasi penyinaran matahari

5. Kandungan nitrogen, fosfor, dan kalium pada tanah

6. Tekanan udara dan arah angin

Tujuan proyek:

1. Memberikan rekomendasi waktu tanam optimal berdasarkan prediksi cuaca dan kondisi tanah

2. Memberikan saran dosis pupuk sesuai kandungan hara tanah

3. Memberikan peringatan dini terhadap risiko cuaca ekstrem

4. Mendukung petani dalam meningkatkan produktivitas secara berkelanjutan

Sistem ini ditujukan untuk diakses melalui perangkat mobile agar ramah bagi petani lokal, serta dapat bekerja dengan dataset historis maupun sensor lingkungan secara real-time.

## Urgensi Penyelesaian Masalah
Menurut World Bank (2022), lebih dari 70% petani kecil di negara berkembang tidak memiliki akses terhadap informasi iklim dan tanah secara akurat. Hal ini berkontribusi pada rendahnya produktivitas pertanian dan ketidakstabilan pendapatan petani.

Sementara itu, studi oleh McKinsey & Company (2020) menunjukkan bahwa penerapan teknologi digital, termasuk AI, dalam pertanian dapat mendorong efisiensi dan meningkatkan hasil panen secara global hingga US$500 miliar per tahun pada 2030.

Oleh karena itu, penerapan AI di bidang ini menjadi solusi strategis dan mendesak untuk mengoptimalkan sistem pertanian nasional menuju ketahanan pangan dan kesejahteraan petani.

## Referensi
- Food and Agriculture Organization. (2023). The State of Food and Agriculture 2023: Leveraging automation in agriculture for transforming agrifood systems. https://www.fao.org

- Zhang, M., Li, M., Wang, X., & Wang, Q. (2020). Application of deep learning algorithms in agricultural image recognition: A review. Computers and Electronics in Agriculture, 175, 105467. https://doi.org/10.1016/j.compag.2020.105467

- World Bank. (2022). Transforming Agriculture for Shared Prosperity and Improved Livelihoods. https://www.worldbank.org

- McKinsey & Company. (2020). Agriculture’s connected future: How technology can yield new growth. https://www.mckinsey.com/industries/agriculture

## Business Understanding

Pada bagian ini, kamu perlu menjelaskan proses klarifikasi masalah.

Bagian laporan ini mencakup:

### Problem Statements

Menjelaskan pernyataan masalah latar belakang:
- Pernyataan Masalah 1
- Pernyataan Masalah 2
- Pernyataan Masalah n

### Goals

Menjelaskan tujuan dari pernyataan masalah:
- Jawaban pernyataan masalah 1
- Jawaban pernyataan masalah 2
- Jawaban pernyataan masalah n

Semua poin di atas harus diuraikan dengan jelas. Anda bebas menuliskan berapa pernyataan masalah dan juga goals yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**:
- Menambahkan bagian “Solution Statement” yang menguraikan cara untuk meraih goals. Bagian ini dibuat dengan ketentuan sebagai berikut: 

    ### Solution statements
    - Mengajukan 2 atau lebih solution statement. Misalnya, menggunakan dua atau lebih algoritma untuk mencapai solusi yang diinginkan atau melakukan improvement pada baseline model dengan hyperparameter tuning.
    - Solusi yang diberikan harus dapat terukur dengan metrik evaluasi.

## Data Understanding
Paragraf awal bagian ini menjelaskan informasi mengenai data yang Anda gunakan dalam proyek. Sertakan juga sumber atau tautan untuk mengunduh dataset. Contoh: [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Restaurant+%26+consumer+data).

Selanjutnya uraikanlah seluruh variabel atau fitur pada data. Sebagai contoh:  

### Variabel-variabel pada Restaurant UCI dataset adalah sebagai berikut:
- accepts : merupakan jenis pembayaran yang diterima pada restoran tertentu.
- cuisine : merupakan jenis masakan yang disajikan pada restoran.
- dst

**Rubrik/Kriteria Tambahan (Opsional)**:
- Melakukan beberapa tahapan yang diperlukan untuk memahami data, contohnya teknik visualisasi data atau exploratory data analysis.

## Data Preparation
Pada bagian ini Anda menerapkan dan menyebutkan teknik data preparation yang dilakukan. Teknik yang digunakan pada notebook dan laporan harus berurutan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan proses data preparation yang dilakukan
- Menjelaskan alasan mengapa diperlukan tahapan data preparation tersebut.

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

