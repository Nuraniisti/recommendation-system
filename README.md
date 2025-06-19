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
- Menerapkan pendekatan collaborative filtering untuk merekomendasikan buku.

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

**Data duplikat**
|  dataset  |     data duplikat   |
|-----------|---------------------|
|  Books    |          0          |
|  Ratings  |          0          |
|  Users    |          0          |

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


### Exploratory Data Analysis - Univariate Analysis

![image](https://github.com/user-attachments/assets/77500cc7-4df3-4637-b7f4-3a337e01870e)     

![image](https://github.com/user-attachments/assets/904e45fc-1f90-492d-ad33-5115a107b173)

![image](https://github.com/user-attachments/assets/c0b30f50-a8bd-450b-8679-3cbeb5201b8d)



### Exploratory Data Analysis - Multivariate Analysis

![image](https://github.com/user-attachments/assets/7268a8f1-4934-4031-b416-d72e10717d76)

![image](https://github.com/user-attachments/assets/0e75fbce-9440-4a94-abfe-2854ac83ee9b)


## Data Preparation

