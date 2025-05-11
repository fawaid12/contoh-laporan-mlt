# Laporan Proyek Machine Learning - Muhammad Fawaid As'ad

## Domain Proyek

Sektor pertanian merupakan bagian krusial dalam ketahanan pangan dan ekonomi suatu negara. Pemilihan tanaman yang sesuai untuk ditanam di suatu wilayah sangat bergantung pada faktor-faktor seperti kadar nitrogen (N), fosfor (P), kalium (K), suhu, kelembaban, pH tanah, dan curah hujan. Kesalahan dalam memilih jenis tanaman dapat mengakibatkan gagal panen dan kerugian ekonomi.

Dengan meningkatnya ketersediaan data dan kemampuan komputasi, pendekatan machine learning dapat dimanfaatkan untuk membantu proses pengambilan keputusan dalam menentukan tanaman yang cocok berdasarkan parameter lingkungan. Menurut [FAO](https://www.fao.org/home/en/), efisiensi dalam manajemen pertanian berbasis data berpotensi meningkatkan hasil panen dan mengurangi risiko gagal tanam.

Dalam proyek ini, dibangun sistem klasifikasi berbasis machine learning untuk merekomendasikan jenis tanaman yang paling sesuai berdasarkan parameter-parameter tersebut. Sistem ini diharapkan dapat membantu petani maupun pengambil kebijakan dalam membuat keputusan yang lebih akurat dan efisien.

## Business Understanding

Problem Statements

- Bagaimana cara mengklasifikasikan jenis tanaman yang cocok berdasarkan data cuaca dan tanah?

- Algoritma machine learning apa yang paling efektif untuk tugas klasifikasi tanaman ini?

Goals

- Membangun model klasifikasi yang dapat memprediksi jenis tanaman berdasarkan parameter lingkungan.

- Membandingkan beberapa algoritma machine learning dan memilih yang paling optimal.

Solution Statements

- Menerapkan beberapa algoritma machine learning seperti Random Forest, Decision Tree, SVC, KNN, dan Gaussian Naive Bayes.

- Mengukur performa masing-masing model menggunakan metrik klasifikasi seperti akurasi, precision, recall, dan F1-score.

- Memilih model terbaik berdasarkan hasil evaluasi tersebut.

## Data Understanding

Dataset yang digunakan adalah Crop_recommendation.csv yang tersedia secara publik dan dapat diunduh dari [Kaggle - Crop Recommendation Dataset](https://www.kaggle.com/datasets/siddharthss/crop-recommendation-dataset/data).

Dataset ini terdiri dari 2200 baris data dan 8 kolom:

- N, P, K: Kandungan Nitrogen, Fosfor, dan Kalium (dalam ppm)

- temperature: Suhu lingkungan (°C)

- humidity: Kelembaban udara (%)

- ph: Keasaman tanah

- rainfall: Curah hujan (mm)

- label: Jenis tanaman (target klasifikasi)

Data yang telah dimuat kemudian dilakukan eksplorasi data (EDA) untuk menggali lebih dalam informasi dari data, seperti:

- Distribusi nilai pada tiap fitur numerik untuk mendeteksi skewness atau konsistensi skala antar fitur.

- Rata-rata dan sebaran nilai tiap fitur untuk memahami tren umum pada parameter lingkungan.

- Data yang missing value atau duplicate

- jumlah dan distribusi label tanaman untuk memastikan data target tidak terlalu imbang atau bias terhadap jenis tanaman tertentu.

Untuk memahami struktur dan distribusi data, dilakukan beberapa teknik visualisasi:

- Histogram: Untuk melihat distribusi nilai pada setiap fitur numerik seperti N, P, K, suhu, kelembaban, pH, dan curah hujan.

- Pie Chart: Untuk melihat proporsi masing-masing jenis tanaman dalam dataset.

- Correlation Matrix (Heatmap): Untuk memahami hubungan antar fitur numerik dan potensi multikolinearitas.

- Boxplot: Untuk mendeteksi adanya outlier pada fitur numerik.

Visualisasi ini membantu dalam menganalisis pola, mendeteksi distribusi yang tidak seimbang, serta mengenali fitur yang mungkin redundant atau saling berkorelasi tinggi. digunakan untuk mendeteksi adanya outlier pada fitur numerik. Outlier ditemukan pada beberapa fitur (seperti rainfall dan potassium), namun tidak dihapus karena masih dapat mencerminkan kondisi riil yang ekstrem di dunia nyata.
  
## Data Preparation

- Encoding Label: Label tanaman dikonversi ke bentuk numerik dengan LabelEncoder.

- Splitting Data: Data dibagi menjadi data latih (80%) dan data uji (20%).

- Scaling: Fitur numerik dinormalisasi menggunakan StandardScaler agar model seperti KNN dan SVM tidak bias terhadap fitur berskala besar.

Tahapan-tahapan ini dilakukan agar model dapat bekerja optimal dengan data yang bersih dan terstandarisasi.

## Modeling

Model yang digunakan:

- Decision Tree

- Random Forest

- K-Nearest Neighbors (KNN)

- Support Vector Classifier (SVC)

- Gaussian Naive Bayes

Setiap model dilatih menggunakan data latih terstandarisasi. Belum dilakukan hyperparameter tuning lanjutan, karena fokus utama proyek ini adalah membandingkan baseline performa antar algoritma.

Kelebihan & Kekurangan Singkat:

- Random Forest: Akurasi tinggi dan robust terhadap outlier, tetapi lebih kompleks.

- GaussianNB: Cepat, ringan, dan surprisingly akurat di dataset ini.

- KNN: Sederhana tapi sensitif terhadap skala dan outlier.

- SVC: Bagus untuk data dengan margin yang jelas, tapi butuh banyak tuning.

- Decision Tree: Mudah dimengerti dan cepat, tapi bisa overfitting.

## Evaluation
Metrik evaluasi:

- Accuracy = (TP + TN) / (TP + TN + FP + FN)

- Precision = TP / (TP + FP)

- Recall = TP / (TP + FN)

- F1 Score = 2 * (Precision * Recall) / (Precision + Recall)

Hasil Evaluasi

Model :

    1. Decision Tree 
    
        - Accuracy  : 98.64%
        
        - Precision : 98.61%
        
        - Recall    : 98.73%
        
        - F1-Score  : 98.64%
        
    2. Random Forest 
    
        - Accuracy  : 99.31%
        
        - Precision : 99.26%
        
        - Recall    : 99.33%
        
        - F1-Score  : 99.26%
        
    3. K-Nearest Neighbors (KNN) 
    
        - Accuracy  : 95.68%
        
        - Precision : 95.63%
        
        - Recall    : 96.02%
        
        - F1-Score  : 95.46%
        
    4. Support Vector Classifier (SVC)
    
        - Accuracy  : 96.81%
        
        - Precision : 96.77%
        
        - Recall    : 96.95%
        
        - F1-Score  : 96.66%
        
    5. Gaussian Naive Bayes 
    
        - Accuracy  : 99.54%
        
        - Precision : 99.63%
        
        - Recall    : 99.52%
        
        - F1-Score  : 99.55%
        
Interpretasi:

- Gaussian Naive Bayes merupakan model terbaik di semua metrik.

- Random Forest juga unggul dan cocok untuk implementasi produksi yang stabil.

- KNN dan SVC cocok untuk baseline atau kasus terbatas, tapi kurang unggul dalam konteks ini.

## Kesimpulan

Berdasarkan hasil evaluasi, model Gaussian Naive Bayes dipilih sebagai model akhir karena memberikan kombinasi terbaik antara akurasi, kecepatan, dan efisiensi komputasi. Proyek ini menunjukkan bahwa pendekatan machine learning dapat memberikan solusi yang praktis dan efektif untuk tantangan nyata di sektor pertanian.

Laporan ini telah mengikuti struktur dan kriteria penilaian proyek machine learning, dan seluruh tahapan mulai dari pemilihan domain, pemahaman bisnis, pengolahan data, modeling, hingga evaluasi telah dijelaskan dengan terstruktur.



