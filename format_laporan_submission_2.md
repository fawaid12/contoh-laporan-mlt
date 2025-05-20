# Laporan Proyek Machine Learning - Muhammad Fawaid As'ad

## Project Overview
Anime merupakan salah satu bentuk hiburan visual yang sangat populer secara global. Dengan ribuan judul dan genre berbeda yang tersedia, pengguna sering kali kebingungan menentukan anime berikutnya yang layak ditonton. Sistem rekomendasi hadir sebagai solusi untuk membantu pengguna menemukan anime sesuai preferensi mereka.
Proyek ini bertujuan untuk membangun sistem rekomendasi anime yang personal dan relevan dengan memanfaatkan dua pendekatan:
- **Content-Based Filtering (CBF)**: Merekomendasikan anime yang mirip dengan apa yang disukai pengguna berdasarkan konten (genre & sinopsis).
- **Collaborative Filtering dengan NeuMF**: Mempelajari pola interaksi pengguna dan item melalui model deep learning.

Proyek ini penting untuk dikembangkan karena dapat membantu mempermudah eksplorasi konten, meningkatkan kepuasan pengguna, dan mengurangi waktu pencarian tontonan yang sesuai.

## Business Understanding

### A. Problem Statements

- Bagaimana memberikan rekomendasi anime yang sesuai dengan preferensi pengguna secara otomatis?

- Pendekatan mana yang lebih akurat dan relevan dalam memberikan rekomendasi: content-based atau collaborative filtering?

### B. Goals

- Menghasilkan top-10 rekomendasi anime untuk setiap pengguna.

- Mengimplementasikan dan mengevaluasi dua pendekatan sistem rekomendasi.

- Membandingkan keunggulan pendekatan content-based dan collaborative filtering.

### C. Solution statements

- Content-Based Filtering (CBF):
Menggunakan fitur teks sinopsis dan genre anime. TF-IDF digunakan untuk membentuk representasi fitur dan cosine similarity digunakan untuk menghitung kemiripan antar anime.

- Collaborative Filtering dengan NeuMF:
Menggunakan interaksi rating user-anime. NeuMF menggabungkan Generalized Matrix Factorization (GMF) dan Multi-Layer Perceptron (MLP) dalam satu arsitektur neural network untuk mempelajari pola interaksi yang kompleks.
    
## Data Understanding

### A. Dataset

Dataset diambil dari [Kaggle - Anime Recommendations Database](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database/data) dan terdiri dari:
1. anime.csv :
    - anime_id: ID unik anime
    - name: Nama anime
    - genre: Daftar genre
    - type: Tipe anime (TV, Movie, OVA)
    - rating: Rating rata-rata
    - synopsis: Deskripsi/sinopsis anime
2. rating.csv
   - user_id: ID pengguna
   - anime_id: ID anime
   - rating: Skor yang diberikan pengguna (1-10, -1 untuk belum menonton)

### B. Exploratory Data Analysis (EDA)
- Distribusi jumlah rating per user
- Top anime dengan rating terbanyak
- Genre yang paling umum
- Korelasi rating vs genre

## Data Preparation
Sebelum membangun model sistem rekomendasi, langkah krusial yang harus dilakukan adalah data preparation. Data mentah sering kali mengandung nilai kosong, duplikat, inkonsistensi format, serta belum dalam bentuk yang sesuai untuk diolah oleh model. Tanpa tahap ini, performa model bisa menurun drastis atau bahkan gagal dijalankan. Data cleaning yang dilakukan secara umum penghapusan data missing value dan data duplicate. Tanpa tahap ini, performa model bisa menurun drastis atau bahkan gagal dijalankan. Oleh karena itu, data preparation dilakukan untuk memastikan:
- Model menerima input dalam format numerik yang sesuai
- Tidak ada noise atau duplikasi yang memengaruhi hasil
- Proses pelatihan menjadi efisien dan akurat
- Hasil rekomendasi dapat diandalkan dan relevan.

### Content Based Filtering
Untuk pendekatan Content-Based Filtering, proses persiapan data berfokus pada atribut konten dari anime, yaitu genre. Berikut adalah tahapan yang dilakukan:
- Penanganan Missing Values : Kolom genre diperiksa, dan setiap nilai kosong (NaN) diisi dengan 'unknown' agar dapat diproses secara konsisten.
- Penyaringan Data Unik dan Valid: Hanya data anime yang memiliki genre dan nama yang valid serta tidak duplikat yang dipertahankan. Hal ini memastikan bahwa model tidak membandingkan atau merekomendasikan entri yang tidak informatif.
- Ekstraksi Fitur Konten : Genre dari anime diproses menggunakan teknik TF-IDF Vectorization untuk mengubah data teks menjadi representasi vektor numerik. Teknik ini mempertimbangkan frekuensi relatif kata dalam genre untuk menilai kepentingannya.
- Perhitungan Similaritas : Matriks kemiripan antar anime dihitung menggunakan cosine similarity terhadap vektor hasil TF-IDF. Ini menghasilkan nilai kemiripan antar anime berdasarkan genre-nya.
- Mapping Judul ke Indeks: Dibuat pemetaan antara nama anime dan indeksnya dalam dataset anime_cs. Ini berguna untuk pencarian dan rekomendasi berdasarkan nama anime yang diinput pengguna.

### Collaborative Filtering 
Pada pendekatan Collaborative Filtering, data rating pengguna dan informasi anime dipersiapkan agar bisa digunakan oleh model berbasis Neural Collaborative Filtering (NeuMF). Adapun langkah-langkah persiapan data yang dilakukan adalah sebagai berikut:
- Rhesuffle data rating : data diacak agar tidak terjadi bias sehingga mengurangi overfitting data
- Sampling Data : Pengambilan sampel data 3 juta data rating untuk mengurangi waktu komputasi
- Penggabungan Data Rating dan Anime: Dataset ratings_sample yang berisi penilaian dari pengguna digabungkan dengan dataset anime menggunakan kolom anime_id sebagai kunci. Hal ini memungkinkan setiap rating memiliki konteks deskriptif terhadap anime yang dinilai.
- Pembersihan Data : Hanya baris dengan nilai rating lebih dari 0 yang digunakan. Nilai 0 sering kali menunjukkan bahwa pengguna belum memberikan penilaian yang berarti. Baris dengan nilai kosong (NaN) juga dihapus untuk menjaga integritas data.
- Encoding Kolom Identitas : Kolom user_id dan anime_id diencoding menggunakan LabelEncoder untuk mengubahnya menjadi nilai numerik. Hal ini diperlukan karena model rekomendasi berbasis neural network membutuhkan input berupa integer terindeks.
- Konversi Tipe Data Rating : Nilai rating diubah menjadi tipe float32 untuk efisiensi komputasi dan kompatibilitas dengan model deep learning.
- Pembagian Data Train dan Test : Dataset kemudian dibagi menjadi data latih (80%) dan data uji (20%) menggunakan fungsi train_test_split. Pembagian ini dilakukan secara stratifikasi terhadap label rating untuk menjaga distribusi rating yang seimbang di kedua subset.
- Ekstraksi Fitur Input Model : Setelah pemisahan data, kolom user dan anime dipecah menjadi input tersendiri. Ini dilakukan karena model NeuMF menerima dua input terpisah, yakni ID pengguna dan ID item (anime).

## Modeling
### A. Content Based Filtering
Content- based filtering merupakan sistem rekomendasi yang didasari dengan korelasi antara antar item. Metode tersebut menggunakan informasi item yang direpresentasikan sebagai atribut untuk dihitung nilai kemiripan antar item [[1]](https://doi.org/10.1016/j.eswa.2017.08.008). Model ini memiliki kelebihan dan kekurangan yang akan disebutkan sebagai berikut:

1. Kelebihan
- mempunyai kemampuan untuk merekomendasikan item yang sifatnya baru bagi pengguna [[2]](https://seminar.ilkom.unsri.ac.id/index.php/ars/article/view/878/777)
- kemampuan merekomendasikan item yang tidak terlihat sehingga mampu menyelesaikan keterbatasan cold start problem terkait dengan item [[3]](https://doi.org/10.1145/3400286.3418253).

2. Kekurangan
- terbatasnya rekomendasi hanya pada item-item yang mirip sehingga tidak ada kesempatan untuk mendapatkan item yang tidak terduga [[4]](https://jurnal.uns.ac.id/itsmart/article/viewFile/35008/27748)

#### Tahapan Model
- Mengambil indeks anime.
- Hitung cosine similarity antar-anime.
- Hindari merekomendasikan dirinya sendiri.
- Urutkan berdasarkan nilai similarity tertinggi.
### B. Collaborative Filtering dengan NeuMF
Neural Matrix Factorization (NeuMF) merupakan model sistem rekomendasi yang dibangun dengan Multilayer Perceptron (MLP), Generalized Matrix Factorization (GMF), dan penggabungan keduanya[[5]](https://dspace.uii.ac.id/bitstream/handle/123456789/42596/18522346.pdf?sequence=1). Model ini memiliki kelebihan dan kekurangan sebagai berikut.
1. Kelebihan
- Memanfaatkan pola interaksi antar user-item
- Dapat menangkap preferensi kompleks dengan MLP
- Lebih personal karena berdasarkan riwayat user
2. Kekurangan
- Butuh banyak data interaksi (tidak cocok untuk cold-start)
- Model besar dan butuh komputasi tinggi
- Rentan overfitting jika data sedikit atau tidak seimbang
#### Tahapan
- Input: ID pengguna dan ID anime.
- GMF Part:
  - Embedding user dan anime (mf_dim=8)
  - Digabung pakai Multiply()
  - L2 regularization (1e-6) untuk mencegah overfitting
- MLP Part:
  - Embedding dengan dimensi sesuai hidden layer (32, lalu 16)
  - Dense layer dengan ReLU
  - Dropout 0.3 untuk regularisasi
- Output Layer:
  - Gabungan dari GMF dan MLP
  - Dense(1) untuk memprediksi rating
- Kompilasi dan Training
  - Loss: Mean Squared Error (MSE)
  - Metric: Mean Absolute Error (MAE)
  - Optimizer: Adam
  - EarlyStopping digunakan untuk menghentikan pelatihan jika model tidak membaik selama 3 epoch.

Tahapan ini membahas mengenai model sisten rekomendasi yang Anda buat untuk menyelesaikan permasalahan. Sajikan top-N recommendation sebagai output.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menyajikan dua solusi rekomendasi dengan algoritma yang berbeda.
- Menjelaskan kelebihan dan kekurangan dari solusi/pendekatan yang dipilih.

## Evaluation

### Content Based Filtering


### Collaborativ Filtering
Model dievaluasi berdasarkan:
- Loss Function (MSE): Untuk menghitung error kuadrat prediksi rating.
- MAE (Mean Absolute Error): Rata-rata selisih mutlak antara rating prediksi dan aktual.
Pada bagian ini Anda perlu menyebutkan metrik evaluasi yang digunakan. Kemudian, jelaskan hasil proyek berdasarkan metrik evaluasi tersebut.

Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.


## Referensi
[[1]](https://doi.org/10.1016/j.eswa.2017.08.008) Son, J., & Kim, S. B. 2017. Content-based filtering for recommendation systems using multiattribute networks. Expert Systems with Applications, 89, 404–412. https://doi.org/10.1016/j.eswa.2017.08.008

[[2]](https://seminar.ilkom.unsri.ac.id/index.php/ars/article/view/878/777) Rendi, M., Jauhari, J., & Rifai, A. 2016. Pengembangan Sistem Citizen Journalism Berbasis Website dengan Metode Content Based Filtering. Annual Research Seminar: Computer Science and Information and Communications Technology, 2(1). https://seminar.ilkom.unsri.ac.id/index.php/ars/article/view/878/777

[[3]](https://doi.org/10.1145/3400286.3418253) Nguyen, L. V., Nguyen, T.-H., & Jung, J. J. 2020. Content-Based Collaborative Filtering using Word Embedding. In Proceedings of the International Conference on Research in Adaptive and Convergent Systems (pp. 96–100). New York, NY, USA: ACM. https://doi.org/10.1145/3400286.3418253.

[[4]](https://jurnal.uns.ac.id/itsmart/article/viewFile/35008/27748) R. H. Mondi, A. Wijayanto, and Winarno, “Recommendation System with Content-Based Filtering Method for Culinary Tourism in Mangan Application,” ITSMART: Jurnal Ilmiah Teknologi dan Informasi, vol. 8, no. 2, pp. 90–96, Dec. 2019. [Online]. Available: https://jurnal.uns.ac.id/itsmart/article/viewFile/35008/27748

[[5]](https://dspace.uii.ac.id/bitstream/handle/123456789/42596/18522346.pdf?sequence=1) S. C. Aishwvarya, Optimasi Model Sistem Rekomendasi Film dengan Neural Network (Studi Kasus: Platform Letterboxd), Tugas Akhir, Program Studi Teknik Industri, Fakultas Teknologi Industri, Universitas Islam Indonesia, Yogyakarta, 2022. https://dspace.uii.ac.id/bitstream/handle/123456789/42596/18522346.pdf?sequence=1
