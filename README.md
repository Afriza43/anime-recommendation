# Sistem Rekomendasi Anime dengan Content-Based Filtering dan Collaborative Filtering

## Project Overview

Dalam beberapa tahun terakhir, popularitas anime telah meningkat secara signifikan di seluruh dunia. Dikutip dari jurnal "Kepopuleran dan Penerimaan Anime Jepang di Indonesia", penggemar anime kini lebih sering menikmati anime melalui layanan streaming atau aplikasi khusus anime seperti Netflix dan AbemaTV. Selain itu, platform seperti MyAnimeList menyediakan tempat bagi penggemar anime untuk menonton, mengulas, dan memberikan peringkat pada anime favorit mereka.

Seiring dengan pertumbuhan jumlah pengguna dan variasi anime yang terus berkembang, MyAnimeList memiliki kebutuhan untuk memahami bagaimana preferensi pengguna terbentuk serta bagaimana berbagai faktor memengaruhi rating yang diberikan pada anime. Hal ini penting agar platform dapat meningkatkan kualitas rekomendasi anime yang lebih sesuai dengan preferensi setiap individu.

Dikutip dari jurnal "Rekomendasi Anime dengan Latent Semantic Indexing Berbasis Sinopsis Genre", semakin banyaknya jumlah anime yang beredar saat ini membuat penonton sering kali kesulitan menemukan anime yang sesuai dengan selera mereka. Oleh karena itu, dengan adanya dataset preferensi pengguna yang besar dari MyAnimeList, kita dapat mengeksplorasi pola preferensi ini untuk membangun sistem rekomendasi yang lebih baik dan relevan, sehingga membantu pengguna menemukan anime yang sesuai dengan minat mereka.

Referensi:

- [Rekomendasi Anime dengan Latent Semantic Indexing Berbasis Sinopsis Genre](https://www.google.com/url?q=https%3A%2F%2Fwww.researchgate.net%2Fprofile%2FHapnes-Toba%2Fpublication%2F274712918_Rekomendasi_Anime_dengan_Latent_Semantic_Indexing_Berbasis_Sinopsis_Genre%2Flinks%2F5527488f0cf2e486ae40fd8b%2FRekomendasi-Anime-dengan-Latent-Semantic-Indexing-Berbasis-Sinopsis-Genre.pdf)
- [Kepopuleran dan Penerimaan Anime Jepang Di Indonesia](https://www.google.com/url?q=https%3A%2F%2Fejournal.unitomo.ac.id%2Findex.php%2Fayumi%2Farticle%2Fview%2F2808)

## Business Understanding

### Problem Statements

- Berdasarkan data mengenai pengguna, bagaimana membuat sistem rekomendasi anime yang dipersonalisasi dengan teknik content-based filtering?
- Dengan data rating dan genre anime yang ada, bagaimana situs MyAnimeList dapat merekomendasikan anime lain yang mungkin disukai dan belum pernah dilihat oleh penonton anime?

### Goals

Untuk menjawab problem statement di atas, dibuatlah sistem rekomendasi dengan tujuan atau goals sebagai berikut:

- Menghasilkan sejumlah rekomendasi anime yang dipersonalisasi untuk pengguna dengan teknik content-based filtering.
- Menghasilkan sejumlah rekomendasi anime yang sesuai dengan preferensi penonton dan belum pernah dilihat oleh penonton tersebut sebelumnya dengan teknik collaborative filtering.

  ### Solution statements

  - Content-based Filtering: Menggunakan data genre untuk membuat rekomendasi berdasarkan anime yang telah disukai pengguna sebelumnya.
  - Collaborative Filtering: Menggunakan pendekatan berbasis kesamaan pengguna untuk merekomendasikan anime yang ditonton oleh pengguna dengan preferensi serupa.

## Data Understanding

Dataset yang digunakan berisi tentang data anime yang diambil dari situs MyAnimeList. Terdapat 2 dataset berformat .csv, yaitu:

- Anime.csv : berisi informasi tentang anime dari id anime, nama anime, genre, member komunitas, dan tipe penayangan.
- Rating.csv, yang berisi penilaian rating anime oleh user

Dataset: [Anime Recommendations Database](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database).

### Variabel-variabel pada Anime Recommendations Database adalah sebagai berikut:

#### Anime.csv

Deskripsi variabel yang ada pada Anime.csv :

- anime_id: ID unik dari setiap anime.
- name: Nama lengkap dari anime.
- genre: Daftar genre anime yang dipisahkan oleh koma.
- type: Jenis anime, seperti TV, Movie, atau OVA.
- episodes: Jumlah episode untuk setiap anime (jika film, jumlahnya adalah 1).
- rating: Rata-rata rating dari komunitas untuk anime tersebut, dari skala 1 sampai 10.
- members: Jumlah anggota komunitas yang tergabung dalam grup anime tersebut.

#### Rating.csv

Deskripsi variabel yang ada pada Rating.csv :

- user_id - id pengguna yang dihasilkan secara acak dan tidak dapat diidentifikasi.
- anime_id - anime yang diberi rating oleh pengguna ini.
- rating - rating dengan skala 1 - 10 yang ditetapkan pengguna ini (bernilai -1 jika pengguna menontonnya tetapi tidak memberikan rating).

#### Exploratory Data Analysis

1. Distribusi Rating Anime

![distribusi_rating_anime](https://github.com/user-attachments/assets/3ac76f4c-2399-47e6-abb0-dea8595754ec)

Sebagaian besar anime yang ada mendapatkan rating antara 6 - 7 dari skala 1 - 10

2. Distribusi Jenis Anime

![distribusi_jenis_anime](https://github.com/user-attachments/assets/e99166d6-10b8-4f2d-8031-7b0f37887fb5)

Sekitar 57% dari anime yang ada, disajikan dalam bentuk serial TV dan tambahan OVA

3. Anime dengan Rating Tertinggi

![top_highest_anime](https://github.com/user-attachments/assets/afaf3c86-30a2-4786-9059-e05b86cee806)

Anime tertinggi di anime adalah "Taka no Tsume 8". Selain itu, anime berjudul Gintama memiliki 2 seasons dengan rating yang tinggi

4. Jumlah Anime berdasarkan Genre

![jumlah_anime_based_genre](https://github.com/user-attachments/assets/da8c09ad-e624-48c8-bfab-6e3e99b0e7a1)

Anime ber genre Comedy berjumlah paling banyak, kemudian diikuti genre Action, Adventure, dan Fantasy

5. Anime dengan Member Komunitas Terbanyak

![top_highest_anime_community](https://github.com/user-attachments/assets/0fddd8af-82a9-49da-bb1f-822dde8d159c)

Death Note menjadi anime yang memiliki member paling banyak dalam komunitas anime nya. Diikuti dengan anime Shingeki no Kyojin dan Sword Art Online

## Data Preparation

Pada bagian ini, kita menerapkan beberapa teknik Data Preparation untuk mempersiapkan data sebelum membangun model sistem rekomendasi. Berikut adalah tahapan-tahapan yang dilakukan:

- Menggabungkan Dataset:
  Dataset anime dan rating digabungkan berdasarkan kolom 'anime_id' untuk mendapatkan data lengkap yang mencakup informasi rating pengguna terhadap anime tertentu.

  Alasan: Menggabungkan data penting untuk mendapatkan hubungan antara pengguna dan anime yang mereka beri rating. Hal ini diperlukan untuk analisis lebih lanjut dan pembuatan sistem rekomendasi.

- Mengubah Nama Kolom:
  Kolom 'rating_user' diganti namanya menjadi 'user_rating' untuk kejelasan.

  Alasan: Memberikan nama yang lebih deskriptif pada kolom sehingga lebih mudah dipahami dan tidak menyebabkan kebingungan.

- Cek dan Hapus Data Kosong:
  Dilakukan pengecekan terhadap data kosong, dan semua data yang hilang ditangani sesuai kebutuhan.

- Membuat Salinan Dataset:
  Dataset asli disalin ke dalam variabel baru (anime_data) untuk menjaga data mentah tetap aman dan bisa digunakan kembali di kemudian hari jika diperlukan.

  Alasan: Menjaga data asli tetap utuh selama proses pemrosesan dan pemodelan.

- Konversi ke Bentuk List:
  Beberapa kolom seperti 'anime_id', 'name', dan 'genre' diubah menjadi list agar mudah diakses dan dimanipulasi.

- Mempersiapkan Dataset untuk Rekomendasi Berdasarkan Genre:
  Dataset baru (anime_new) dibentuk dengan hanya menyertakan kolom yang relevan untuk analisis rekomendasi berbasis genre, yaitu 'id', 'anime_name', dan 'genre'.

  Alasan: Mengisolasi kolom-kolom yang relevan untuk membangun model berbasis genre.

- Cek dan Hapus Duplikat:
  Dilakukan pengecekan terhadap data duplikat, dan jika ditemukan, data duplikat dihapus.

  Alasan: Menghindari duplikasi yang bisa mempengaruhi hasil analisis dan akurasi model.

- Memecah Genre Anime:
  Genre anime yang ada dalam satu string dipisahkan menggunakan teknik preprocessing, menghapus delimiter (tanda koma) sehingga setiap genre bisa diolah secara independen.

  Alasan: Genre dalam format list atau string gabungan tidak bisa langsung digunakan oleh model machine learning seperti TfidfVectorizer. Oleh karena itu, preprocessing diperlukan agar setiap genre bisa diproses secara terpisah.

## Modeling

Pada tahapan ini, kita membangun dua model sistem rekomendasi: Content-Based Filtering dan Collaborative Filtering untuk memberikan rekomendasi anime.

1. Content-Based Filtering (berbasis genre anime menggunakan TF-IDF dan Cosine Similarity)
   Model ini menggunakan pendekatan berbasis konten, di mana rekomendasi diberikan berdasarkan kesamaan genre antara anime.

### Algoritma

- TF-IDF (Term Frequency-Inverse Document Frequency):
  Setiap genre dianalisis dan diubah menjadi representasi numerik dengan TfidfVectorizer.
  Alasan: TF-IDF digunakan untuk mengukur bobot kepentingan genre yang muncul di setiap anime.

- Cosine Similarity:
  Mengukur kesamaan antara anime berdasarkan TF-IDF dari genre untuk memberikan rekomendasi anime dengan genre yang mirip.
  Alasan: Cosine Similarity adalah salah satu teknik paling efektif untuk menghitung kemiripan antara dua vektor (dalam hal ini, representasi genre anime).

  Kelebihan:

  - Mudah diimplementasikan dan memberikan rekomendasi berdasarkan konten (genre) yang mirip.
  - Cocok digunakan ketika tidak ada banyak data pengguna yang tersedia (cold-start problem).
    Kekurangan:
  - Terbatas pada informasi yang ada di metadata (genre). Tidak bisa menangkap preferensi pengguna secara eksplisit.

### Output

![Top 5 Recommendation](https://github.com/user-attachments/assets/ec75d9fb-bc5e-453a-9278-e5cc4efccb2b)

2. Collaborative Filtering (menggunakan model RecommenderNet)
   Pendekatan Collaborative Filtering ini didasarkan pada preferensi pengguna dan item. Model ini dilatih menggunakan embedding untuk memetakan hubungan antara pengguna dan anime yang mereka beri rating.

### Algoritma:

RecommenderNet Model:
Model deep learning yang menggunakan embedding untuk membuat prediksi berdasarkan data rating pengguna. Model ini di-train menggunakan data rating untuk memprediksi rating anime yang mungkin disukai oleh pengguna lain.

Kelebihan:

- Menangkap preferensi pengguna secara lebih akurat.
- Bisa mempelajari pola hubungan antar pengguna dan item yang lebih kompleks.
  Kekurangan:
- Membutuhkan banyak data pengguna untuk menghasilkan rekomendasi yang baik.
- Proses training model lebih kompleks dan memerlukan waktu yang lebih lama.

### Output

![Top 10 Recommendation](https://github.com/user-attachments/assets/b5fa8550-fd14-437f-8591-8b68eb0abc32)

## Evaluation

Dalam proyek ini, metrik evaluasi yang digunakan adalah Root Mean Squared Error (RMSE). RMSE adalah ukuran yang biasa digunakan untuk mengevaluasi kualitas prediksi dalam model rekomendasi. RMSE mengukur perbedaan antara nilai yang diprediksi oleh model dan nilai aktual dalam dataset. Nilai RMSE yang lebih rendah menunjukkan bahwa model memberikan prediksi yang lebih akurat.

![Rumus RMSE](https://media.geeksforgeeks.org/wp-content/uploads/20200622171741/RMSE1.jpg)

### Hasil Evaluasi

- Training Loss: 0.5203
- Training RMSE: 0.0910
- Validation Loss: 0.5966
- Validation RMSE: 0.2094

Interpretasi: Nilai RMSE yang rendah menunjukkan bahwa model memberikan prediksi yang cukup baik untuk data pelatihan, dengan hasil RMSE sekitar 0.0910. Pada data validasi, nilai RMSE meningkat menjadi 0.2094, yang menunjukkan adanya perbedaan kecil antara prediksi dan data aktual.
