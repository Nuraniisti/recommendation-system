# Laporan Proyek Machine Learning - Nurani Istiaen

## Project Overview
Di era digital, banyak platform menyediakan jutaan judul buku dari berbagai genre. Namun, semakin banyak pilihan justru dapat membingungkan pengguna dalam menemukan buku yang sesuai dengan minat mereka. Sistem rekomendasi hadir sebagai solusi untuk membantu pengguna menemukan buku yang relevan berdasarkan preferensi mereka, baik dari rating, maupun genre favorit.

## Business Understanding

### Problems Statement
- Ada kalanya pengguna kesulitan menemukan buku yang sesuai dengan preferensinya secara cepat.

- Platform belum menyediakan rekomendasi yang bersifat personal dan kontekstual.

- Kurangnya keterlibatan pengguna akibat pengalaman eksplorasi konten yang kurang relevan.

### Goals
- Mengembangkan sistem rekomendasi buku berbasis data pengguna dan karakteristik buku.

- Meningkatkan keterlibatan dan kepuasan pengguna melalui personalisasi konten.

- Mengoptimalkan waktu pencarian buku dan penggunaan platform.

### Solution Statements
- Menerapkan pendekatan user-based collaborative filtering untuk merekomendasikan buku.

- Menggunakan data seperti rating buku, user dan genre.

- Memastikan sistem rekomendasi memiliki akurasi terbaik.

## Data Understanding
**Informasi Dataset**      
   
   
Title : Book Recommendation Dataset
Source : Kaggle (https://www.kaggle.com/datasets/bharath011/heart-disease-classification-dataset/data)   
Owner : arashnic / Möbius    
License : CC0: Public Domain    
Visibility : Publik    
Usability : 10.00


Dataset yang digunakan adalah dataset Book-Crossing, yang digunakan untuk membangun sistem rekomendasi buku. Dataset terdiri dari tiga file CSV dengan detail sebagai berikut:   
- **Books.csv**, berisi informasi metadata tentang buku dengan 271.360 baris data dan 8 kolom.

| Nama Kolom          | Deskripsi                                   |
| ------------------- | ------------------------------------------- |
| ISBN                | Kode ISBN unik untuk setiap buku            |
| Book-Title          | Judul buku                                  |
| Book-Author         | Nama penulis buku                           |
| Year-Of-Publication | Tahun publikasi buku                        |
| Publisher           | Penerbit buku                               |
| Image-URL-S         | URL gambar buku ukuran kecil (dari Amazon)  |
| Image-URL-M         | URL gambar buku ukuran sedang (dari Amazon) |
| Image-URL-L         | URL gambar buku ukuran besar (dari Amazon)  |


- **Ratings.csv**, berisi data rating yang diberikan pengguna untuk buku dengan 1.149.780 baris data dan 3 kolom.

| Nama Kolom  | Deskripsi                     |
| ----------- | ----------------------------- |
| User-ID     | ID unik untuk setiap pengguna |
| ISBN        | Kode ISBN buku                |
| Book-Rating | Rating buku (skala 0–10)      |

- **Users.csv**, berisi informasi tentang pengguna yang memberikan rating dengan 278.858 dan 3 kolom.

| Nama Kolom | Deskripsi                     |
| ---------- | ----------------------------- |
| User-ID    | ID unik untuk setiap pengguna |
| Location   | Lokasi pengguna               |
| Age        | Usia pengguna                 |

**Books dataset**
| No. | Kolom               | Non-Null Count  | Dtype  |
| --- | ------------------- | --------------- | ------ |
| 1   | ISBN                | 271360 non-null | object |
| 2   | Book-Title          | 271360 non-null | object |
| 3   | Book-Author         | 271358 non-null | object |
| 4   | Year-Of-Publication | 271360 non-null | object |
| 5   | Publisher           | 271358 non-null | object |
| 6   | Image-URL-S         | 271360 non-null | object |
| 7   | Image-URL-M         | 271360 non-null | object |
| 8   | Image-URL-L         | 271357 non-null | object |

**Ratings dataset**
| No. | Kolom       | Non-Null Count   | Dtype  |
| --- | ----------- | ---------------- | ------ |
| 1   | User-ID     | 1149780 non-null | int64  |
| 2   | ISBN        | 1149780 non-null | object |
| 3   | Book-Rating | 1149780 non-null | int64  |

**Users dataset**
| No. | Kolom    | Non-Null Count  | Dtype   |
| --- | -------- | --------------- | ------- |
| 1   | User-ID  | 278858 non-null | int64   |
| 2   | Location | 278858 non-null | object  |
| 3   | Age      | 168096 non-null | float64 |

insight :    
- Semua kolom pada dataset books bertipe object, termasuk Year-Of-Publication yang seharusnya numerik.
- ketiga dataset memiliki jumlah data yang besar, namun juga terdapat missing value tertama pada kolom age di dataset users.


## Exploratory Data Analysis
**Informasi statistik**

| Nama Statistik | Penjelasan Singkat                                                                                                   |
| -------------- | -------------------------------------------------------------------------------------------------------------------- |
| **count**      | Jumlah data (non-null) pada kolom tersebut. Berguna untuk melihat apakah ada data yang hilang.                       |
| **unique**     | Jumlah nilai yang berbeda/unik dalam kolom (hanya berlaku untuk data kategorikal atau teks).                         |
| **top**        | Nilai yang paling sering muncul (modus) dalam kolom (hanya untuk kategorikal).                                       |
| **freq**       | Frekuensi atau jumlah kemunculan nilai "top" (modus) dalam kolom.                                                    |
| **mean**       | Rata-rata dari semua nilai dalam kolom numerik.                                                                      |
| **std**        | Standar deviasi, menggambarkan seberapa jauh sebaran data dari rata-rata. Semakin besar, semakin bervariasi datanya. |
| **min**        | Nilai terkecil dalam kolom.                                                                                          |
| **25%**        | Kuartil pertama (Q1): 25% data berada di bawah nilai ini.                                                            |
| **50%**        | Median atau kuartil kedua (Q2): 50% data berada di bawah nilai ini.                                                  |
| **75%**        | Kuartil ketiga (Q3): 75% data berada di bawah nilai ini.                                                             |
| **max**        | Nilai terbesar dalam kolom.                                                                                          |

**Ratings dataset**
| Statistik | User-ID         | Book-Rating     |
| --------- | --------------- | --------------- |
| Count     | 1.149780e+06    | 1.149780e+06    |
| Mean      | 1.403864e+05    | 2.866950e+00    |
| Std       | 8.056228e+04    | 3.854184e+00    |
| Min       | 2.000000e+00    | 0.000000e+00    |
| 25%       | 7.034500e+04    | 0.000000e+00    |
| 50%       | 1.410100e+05    | 0.000000e+00    |
| 75%       | 2.110280e+05    | 7.000000e+00    |
| Max       | 2.788540e+05    | 1.000000e+01    |


**Users dataset**
| Statistik | User-ID       | Age           |
| --------- | ------------- | ------------- |
| Count     | 278,858.00000 | 168,096.00000 |
| Mean      | 139,429.50000 | 34.75143      |
| Std       | 80,499.51502  | 14.42810      |
| Min       | 1.00000       | 0.00000       |
| 25%       | 69,715.25000  | 24.00000      |
| 50%       | 139,429.50000 | 32.00000      |
| 75%       | 209,143.75000 | 44.00000      |
| Max       | 278,858.00000 | 244.00000     |

insight :    
- Dengan 271.360 ISBN unik, dataset mencakup koleksi buku yang sangat beragam. Namun, hanya 242.135 judul unik menunjukkan beberapa buku memiliki duplikat judul (misalnya, edisi berbeda dari "Selected Poems" muncul 27 kali). Ini bisa memengaruhi rekomendasi jika tidak dibedakan berdasarkan ISBN.

- Agatha Christie (632 buku) adalah penulis paling produktif, menunjukkan popularitas genre misteri/kriminal. Ini bisa menjadi target rekomendasi untuk penggemar genre tersebut.

- Harlequin (7.535 buku) mendominasi sebagai penerbit, kemungkinan karena fokus pada novel romansa. Ini menunjukkan potensi segmentasi pengguna yang menyukai genre romansa.

- Tahun 2002 adalah yang paling umum (13.903 buku), menunjukkan dataset ini berfokus pada buku-buku modern (akhir 1990-an hingga awal 2000-an)

- Rata-rata usia pengguna adalah ~34,75 tahun (median 32), dengan standar deviasi 14,43, menunjukkan pengguna beragam dari usia muda (25% di bawah 24) hingga lebih tua (75% di bawah 44).

- Nilai Age minimum 0 dan maksimum 244 tidak realistis, menunjukkan outlier atau kesalahan input. Ini perlu dibersihkan


**Data duplikat**
|  dataset  |     data duplikat   |
|-----------|---------------------|
|  Books    |          0          |
|  Ratings  |          0          |
|  Users    |          0          |

insight :    
Tidak ditemukan data duplikat

**Missing Value**

**Books dataset**
| Kolom               | Jumlah Missing |
| ------------------- | -------------- |
| ISBN                | 0              |
| Book-Title          | 0              |
| Book-Author         | 2              |
| Year-Of-Publication | 0              |
| Publisher           | 2              |
| Image-URL-S         | 0              |
| Image-URL-M         | 0              |
| Image-URL-L         | 3              |

**Ratings dataset**
| Kolom       | Jumlah Missing |
| ----------- | -------------- |
| User-ID     | 0              |
| ISBN        | 0              |
| Book-Rating | 0              |

**Users dataset**
| Kolom    | Jumlah Missing |
| -------- | -------------- |
| User-ID  | 0              |
| Location | 0              |
| Age      | 110,762        |

insight :    
- pada dataset books, kolom Book-Title memiliki 2 missing value, Publisher memiliki 2 missing value dan Image-URL-L memiliki 3 missing value
- pada dataset users, kolom age memiliki 110.762 missing value dimana angka ini sangat besar.


### Exploratory Data Analysis - Univariate Analysis

![image](https://github.com/user-attachments/assets/f0072387-f2aa-471c-83ab-b8d9ca175e0c)

insight :    
- rating 0 menunjukkan mayoritas adalah rating implisit (pengguna melihat tanpa rating eksplisit)
- Rating 4, 5, dan 6 sangat sedikit, menunjukkan pengguna jarang memberikan rating netral, mungkin karena bias subjektivitas.
- Peningkatan rating 6-10 menunjukkan segmen pengguna aktif dengan preferensi kuat, cocok untuk rekomendasi berbasis genre.
- Banyaknya rating 0 menunjukkan pengguna pasif, sementara rating tinggi (8-10) menawarkan peluang promosi buku populer.

![image](https://github.com/user-attachments/assets/47f2ae03-873e-4cf6-b3cf-ec107e54fc8f)

insight :    
- Mayoritas pengguna terkonsentrasi di sekitar usia 50, menunjukkan puncak pada kelompok usia ini.
- Sangat sedikit pengguna di atas atau di bawah usia 50, dengan jumlah yang menurun drastis di luar rentang 50-100.
- Jumlah total pengguna tampaknya paling tinggi sekitar 40.000, dengan penurunan tajam di kedua sisi puncak.

Analisis kolom Year-Of-Publication dari dataset books menunjukkan :        
- Tahun 2002 memiliki jumlah buku terbanyak (13.903), diikuti oleh 2001 (13.715) dan 1999 (13.414). Tren menunjukkan penurunan jumlah buku seiring mundur ke tahun yang lebih lama, dengan 1994 memiliki jumlah terendah di antara 10 teratas (8.857).
- menunjukkan bahwa sebagian besar buku dalam dataset diterbitkan pada awal 2000-an, dengan puncak pada tahun 2002.

![image](https://github.com/user-attachments/assets/db701122-7357-46fc-a620-d1598fd575eb)

insight :    
- sebagian besar buku dalam dataset diterbitkan pada awal 2000-an.



### Exploratory Data Analysis - Multivariate Analysis

Hubungan antara usia pengguna (Age) dan rating buku (Book-Rating)

![image](https://github.com/user-attachments/assets/2147c456-a23e-47f5-b93a-883f23b6d554)

insight :    
- Banyak pengguna memberi rating 0
- Di atas usia 100, data terlihat sangat jarang — kemungkinan besar adalah outlier atau kesalahan input
- Titik-titik tersebar merata dari rating 0 hingga 10 di hampir semua kelompok usia. Artinya, usia pengguna tidak secara signifikan memengaruhi kecenderungan memberi rating rendah atau tinggi.
- Meskipun ada sebaran luas, rating 8–10 tetap muncul konsisten di semua kelompok usia.

![image](https://github.com/user-attachments/assets/1d83ffd2-69f2-47b6-a3d3-1faa7e3b868d)



## Data Preparation
Melakukan beberapa langkah untuk menangani nilai hilang (missing values) dalam dataset books dan menampilkan informasi setelahnya.   
- Mengganti nilai hilang di kolom 'Book-Author' dengan 'Unknown Author'.
- Mengganti nilai hilang di kolom 'Publisher' dengan 'Unknown Publisher'.
- Mengganti nilai hilang di kolom 'Image-URL-L' dengan string kosong ('').
- Mengganti nilai hilang di kolom 'Year-Of-Publication' dengan nilai median.

Melakukan beberapa langkah untuk menangani nilai hilang (missing values) dalam dataset users dan menampilkan informasi setelahnya.   
- Dengan mengisi baris yang hilang dengan nilai median

Melakukan penyaringan pada dataset ratings untuk mendapatkan buku-buku populer berdasarkan jumlah rating. Dengan menentukan ambang batas minimum jumlah rating, yaitu 50.   

Melakukan penyaringan lebih lanjut pada dataset ratings_filtered untuk mendapatkan pengguna aktif berdasarkan jumlah rating yang mereka berikan.   

Menggabungkan dataset ratings_filtered dengan subset dari dataset books berdasarkan kolom 'ISBN' untuk membuat dataset akhir bernama final_data.   

Membuat matriks pengguna-buku berdasarkan dataset final_data menggunakan fungsi pivot_table.  


## Modeling   
Menerapkan dekomposisi nilai singular terpotong (Truncated SVD) pada matriks user_book_matrix untuk reduksi dimensi.    

Menghitung kesamaan antar pengguna (cosine_similiarity) berdasarkan matriks yang telah direduksi dan menyimpannya dalam bentuk DataFrame.   

Membagi data menjadi data pelatihan (train_data) dan data pengujian (test_data) dengan rasio 80:20 dan menerapkan transformasi SVD pada data pelatihan dan pengujian.   

## Model Evaluation  
Merekonstruksi matriks prediksi dari hasil reduksi dimensi.    

Menghitung performa model dengan Root Mean Squared Error (RMSE), yaitu ukuran seberapa jauh prediksi model dari nilai sebenarnya.    
= Model RMSE: 0.9564    

insight :    
Angka 0.9564 mengindikasikan bahwa model mampu memprediksi rating yang cukup mendekati kenyataan, sehingga model bisa digunakan sebagai dasar sistem rekomendasi.     

contoh rekomendasi buku untuk satu pengguna tertentu dengan parameter fungsi :

| Parameter            | Penjelasan                                                 |
| -------------------- | ---------------------------------------------------------- |
| `user_id`            | ID pengguna yang ingin diberi rekomendasi                  |
| `user_similarity_df` | DataFrame yang berisi skor kemiripan antar pengguna        |
| `user_book_matrix`   | Matriks interaksi user–buku (misal: rating atau skor lain) |
| `books`              | DataFrame berisi metadata buku (ISBN, Judul, Penulis)      |
| `n`                  | Jumlah buku rekomendasi yang ingin ditampilkan             |

Menampilkan contoh rekomendasi   
Top 10 Rekomendasi Buku untuk User 243

| No | ISBN       | Judul Buku                             | Penulis            | Skor |
| -- | ---------- | -------------------------------------- | ------------------ | ---- |
| 1  | 0440234743 | *The Testament*                        | John Grisham       | 7.4  |
| 2  | 0452264464 | *Beloved (Plume Contemporary Fiction)* | Toni Morrison      | 3.2  |
| 3  | 0971880107 | *Wild Animus*                          | Rich Shapero       | 2.8  |
| 4  | 0345402871 | *Airframe*                             | Michael Crichton   | 2.8  |
| 5  | 0345417623 | *Timeline*                             | Michael Crichton   | 2.8  |
| 6  | 0446310786 | *To Kill a Mockingbird*                | Harper Lee         | 2.5  |
| 7  | 0449005615 | *Seabiscuit: An American Legend*       | Laura Hillenbrand  | 2.5  |
| 8  | 0060168013 | *Pigs in Heaven*                       | Barbara Kingsolver | 2.4  |
| 9  | 0671888587 | *I'll Be Seeing You*                   | Mary Higgins Clark | 2.4  |
| 10 | 0553582747 | *From the Corner of His Eye*           | Dean Koontz        | 2.4  |


## Kesimpulan
Sistem rekomendasi berhasil memberikan saran buku personalisasi berdasarkan kesamaan dengan pengguna lain. Ini menunjukkan bahwa pendekatan user-based collaborative filtering berjalan dengan baik.

Rekomendasi yang diberikan didasarkan pada rata-rata rating dari pengguna yang mirip, sehingga buku-buku yang muncul adalah yang secara konsisten disukai oleh kelompok pengguna dengan preferensi serupa.

Hasil rekomendasi mencakup judul buku, pengarang, dan skor prediksi, yang memberi gambaran seberapa relevan atau populer buku tersebut bagi user target.

==========
