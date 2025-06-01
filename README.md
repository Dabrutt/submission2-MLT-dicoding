# Laporan Proyek Machine Learning - Muhammad Daffa Eka Pramudita

# Project Overview
Industri anime Jepang merupakan salah satu sektor hiburan global yang mengalami pertumbuhan pesat, dengan jumlah judul anime yang terus bertambah setiap tahunnya. Platform seperti [MyAnimeList](https://myanimelist.net/) menyediakan katalog besar yang mencakup ribuan judul anime beserta informasi pendukung seperti genre, studio, skor pengguna, dan popularitas. Namun, banyaknya pilihan sering kali membuat pengguna kesulitan menemukan anime yang sesuai dengan preferensi mereka. Oleh karena itu, pengembangan **sistem rekomendasi anime yang efektif** menjadi sangat penting untuk meningkatkan pengalaman pengguna.

Selain itu, pemahaman terhadap **genre yang paling populer** serta **faktor-faktor yang memengaruhi popularitas** dapat memberikan wawasan berharga bagi studio dan produser anime dalam merancang konten yang lebih relevan dengan minat pasar.

**Mengapa Masalah Ini Penting untuk Diselesaikan**
- Pengguna membutuhkan bantuan dalam menavigasi ribuan pilihan anime.
- Rekomendasi yang relevan dapat meningkatkan retensi pengguna dan kepuasan menonton.
- Produser dapat menggunakan data genre dan popularitas untuk menyusun strategi produksi konten yang lebih tepat sasaran.

Menurut riset yang dilakukan oleh Kumar et al. (2020), sistem rekomendasi berbasis konten dan kolaboratif telah berhasil meningkatkan kepuasan pengguna pada platform hiburan seperti Netflix dan Spotify [1]. Sistem yang sama dapat diadaptasi untuk platform anime. Selain itu, penelitian dari Ismail et al. (2021) menyatakan bahwa genre, popularitas, dan demografi merupakan fitur penting dalam membangun sistem rekomendasi anime yang akurat [2].

Dengan memanfaatkan data dari MyAnimeList, proyek ini bertujuan untuk:
- Menganalisis genre anime yang paling populer berdasarkan data agregat pengguna.
- Membangun prototipe sistem rekomendasi menggunakan algoritma content-based dan collaborative filtering.

# Business Understanding
### Problem Statement
- Bagaimana cara membuat sistem rekomendasi anime yang baik dan relevan berdasarkan preferensi pengguna dan karakteristik anime?
- Apa genre anime yang paling populer di kalangan pengguna MyAnimeList?
### Goals
- Mengembangkan sistem rekomendasi anime yang dapat menyarankan anime relevan kepada pengguna berdasarkan minat mereka.
- Mengidentifikasi genre anime yang paling populer berdasarkan data seperti popularity rank, jumlah anggota (members), dan jumlah favorit (favorites).
### Solution Statement
- Menerapkan algoritma sistem rekomendasi seperti Content-Based Filtering yang berdasarkan pada kemiripan fitur anime seperti genre dan demographic
- Melakukan analisis eksploratif terhadap data MyAnimeList untuk menghitung dan membandingkan popularitas setiap genre.

# Data Understanding 

Dataset yang digunakan diperoleh dari Kaggle dengan link: [Anime Database 2022](https://www.kaggle.com/datasets/harits/anime-database-2022)

Dataset ini berisi informasi lengkap mengenai anime yang terdaftar di situs MyAnimeList hingga tahun 2022. Informasi yang tersedia meliputi judul anime, genre, studio, jumlah episode, rating usia, skor pengguna, popularitas, demografi penonton, dsb. Dataset ini memiliki 21.460 baris data dengan total 28 kolom fitur yang dapat digunakan untuk keperluan eksplorasi data, analisis popularitas, maupun pengembangan sistem rekomendasi anime.

| Nama Fitur         | Fungsi Fitur                                                             | Tipe Data  |
|--------------------|---------------------------------------------------------------------------|------------|
| ID                 | ID anime di MyAnimeList.net                                              | int64      |
| Title              | Judul asli dari anime                                                    | object     |
| Synonyms           | Nama lain dari anime                                                     | object     |
| Japanese           | Judul anime dalam bahasa Jepang                                          | object     |
| English            | Judul anime dalam bahasa Inggris                                         | object     |
| Synopsis           | Ringkasan cerita anime                                                   | object     |
| Type               | Tipe anime (TV, Movie, OVA, dll)                                         | object     |
| Episodes           | Jumlah episode dalam anime                                               | float64    |
| Status             | Status penayangan anime (belum tayang, sedang tayang, sudah selesai)     | object     |
| Start_Aired        | Tanggal atau tahun mulai tayang                                          | object     |
| End_Aired          | Tanggal atau tahun selesai tayang                                        | object     |
| Premiered          | Musim tayang perdana (misalnya: Spring 2023)                             | object     |
| Broadcast          | Jadwal siaran anime                                                      | object     |
| Producers          | Daftar produser                                                          | object     |
| Licensors          | Daftar pemegang lisensi distribusi                                       | object     |
| Studios            | Studio yang memproduksi anime                                            | object     |
| Source             | Sumber cerita anime (manga, novel, original, dll)                        | object     |
| Genres             | Daftar genre anime (aksi, drama, komedi, dll)                            | object     |
| Themes             | Tema-tema yang diangkat da

## EDA
### 1. Distribusi Skor Anime
![image](https://github.com/user-attachments/assets/e54b8b74-b1a1-4422-bc38-a83fa15cd547)

Distribusi skor anime pada grafik di atas menunjukkan pola menyerupai distribusi normal (bell-shaped curve), dengan mayoritas anime memiliki skor antara 5.5 hingga 7.5. Skor minimum berada di bawah 3, dan skor maksimum mendekati 9. Hal ini menunjukkan bahwa sebagian besar anime yang dinilai memiliki kualitas yang dianggap cukup hingga baik oleh pengguna MyAnimeList.

### 2. 15 Genre Anime Terpopuler
![image](https://github.com/user-attachments/assets/14e03478-a2de-4ade-8a9a-297c55df692f)

Grafik ini menampilkan genre yang paling sering muncul dalam daftar anime:
- Genre `Comedy`, `Action`, dan `Fantasy` mendominasi dengan jumlah judul terbanyak.
- Genre seperti `Mystery`, `Sports`, dan `Avant Garde` memiliki jumlah judul yang jauh lebih sedikit.
- Adanya label `Unknown` menunjukkan kemungkinan data yang tidak lengkap atau tidak terklasifikasi.
Hal ini mengindikasikan bahwa preferensi produksi anime cenderung pada genre yang ringan dan dapat diterima oleh berbagai kalangan.

### 3. Rata-rata Skor Per Genre
![image](https://github.com/user-attachments/assets/3ed743eb-ae8e-425e-ad2b-976d427c248d)

Grafik ini menunjukkan rata-rata skor pengguna berdasarkan genre:
- Genre `Award Winning`, `Mystery`, dan `Suspense` cenderung memperoleh skor rata-rata tertinggi, meskipun tidak termasuk genre terpopuler.
- Genre `Comedy` yang populer justru memiliki rata-rata skor yang lebih rendah dibanding genre lainnya.
Ini menunjukkan bahwa popularitas genre tidak selalu berbanding lurus dengan kualitas atau penerimaan pengguna.

## 💡 Insight Awal

- **Genre populer tidak selalu memiliki skor tinggi.** Contohnya, genre Comedy sangat populer namun memiliki skor rata-rata yang lebih rendah dari genre lain seperti Award Winning atau Mystery.
- **Distribusi skor yang normal** menunjukkan adanya penyebaran penilaian yang adil dari pengguna.
- Genre dengan skor tinggi dapat dijadikan fokus dalam sistem rekomendasi.

# Data Preparation
Dalam sistem rekomendasi content-based filtering, kita fokus pada fitur-fitur deskriptif dari konten itu sendiri, seperti genre, studio, sinopsis, atau tipe. Tujuannya adalah merekomendasikan item yang mirip berdasarkan karakteristik isi, bukan popularitas atau interaksi pengguna.

```
df.drop(['Synonyms', 'Japanese','Episodes', 'Start_Aired',
         'End_Aired','Premiered', 'Broadcast','Producers', 'Licensors','Source',
         'Themes','Duration_Minutes','Scored_Users',
         'Ranked', 'Members', 'Favorites'],axis=1,inplace=True)
```
`df.columns`

> Index(['ID', 'Title', 'English', 'Synopsis', 'Type', 'Status', 'Studios',
       'Genres', 'Demographics', 'Rating', 'Score', 'Popularity'],
      dtype='object')


Fitur yang tetap dipertahankan adalah:
- Title, English — digunakan sebagai identitas utama atau fitur teks.
- Synopsis — dapat digunakan dalam NLP untuk ekstraksi fitur konten cerita.
- Type, Status — memberi gambaran format dan status anime.
- Studios — sebagai karakteristik produksi yang mungkin mencerminkan gaya cerita.
- Genres, Demographics, Rating — fitur inti untuk mengukur kemiripan konten.
- Score — bisa digunakan untuk filter kualitas, bukan untuk kemiripan.

### Deteksi Missing Value
![image](https://github.com/user-attachments/assets/bb22b0f0-c01e-4a25-ab24-b6de155a63f4)
1. Rating
    Terdapat 545 entri pada kolom Rating yang bernilai kosong. Ini menunjukkan bahwa ada sejumlah anime yang tidak memiliki klasifikasi umur resmi seperti G (General), PG (Parental Guidance), R (Restricted), dan  sebagainya. Hal ini bisa terjadi karena:
    - Anime tersebut belum diklasifikasikan secara resmi.
    - Informasi tidak tersedia pada saat data dikumpulkan.
