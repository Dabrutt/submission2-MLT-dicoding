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
| Themes             | Tema-tema yang diangkat dalam anime                                      | object     |
| Demographics       | Sasaran demografis (shounen, seinen, josei, dll)                         | object     |
| Duration_Minutes   | Durasi tiap episode (dalam menit)                                        | float64    |
| Rating             | Kategori usia penonton (PG-13, R, dll)                                   | object     |
| Score              | Skor rata-rata dari pengguna MyAnimeList.net                             | float64    |
| Scored_Users       | Jumlah pengguna yang memberikan skor                                     | float64    |
| Ranked             | Peringkat berdasarkan skor tertinggi                                     | float64    |
| Popularity         | Peringkat berdasarkan popularitas (jumlah yang menambahkan ke daftar)    | int64      |
| Members            | Jumlah pengguna yang menambahkan anime ke daftar mereka                  | int64      |
| Favorites          | Jumlah pengguna yang menandai anime sebagai favorit                      | int64      |


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

**1. Rating**

Terdapat 545 entri pada kolom Rating yang bernilai kosong. Ini menunjukkan bahwa ada sejumlah anime yang tidak memiliki klasifikasi umur resmi seperti G (General), PG (Parental Guidance), R (Restricted), dan  sebagainya. Hal ini bisa terjadi karena:

- Anime tersebut belum diklasifikasikan secara resmi.
- Informasi tidak tersedia pada saat data dikumpulkan.

**2. Score**

Kolom ini memiliki 6.898 nilai kosong, yang merupakan jumlah tertinggi dibandingkan kolom lainnya. Ini berarti banyak anime dalam dataset tidak memiliki skor penilaian dari pengguna. Kemungkinan penyebabnya antara lain:

- Anime baru atau belum rilis sehingga belum memiliki cukup ulasan.
- Anime yang kurang dikenal atau memiliki sedikit penonton.
- Data skor tidak tersedia saat pengumpulan.

### Deteksi Data Duplikat
`df.duplicated().sum()`

> np.int64(0)

Tidak ditemukan data duplikat

### Deteksi outlier
```
# Deteksi Outlier dengan metode IQR
def detect_outliers(data):
    outlier_summary = {}
    for column in data.select_dtypes(include=np.number).columns:
        Q1 = data[column].quantile(0.25)
        Q3 = data[column].quantile(0.75)
        IQR = Q3 - Q1
        lower_bound = Q1 - 1.5 * IQR
        upper_bound = Q3 + 1.5 * IQR
        outliers = data[(data[column] < lower_bound) | (data[column] > upper_bound)]
        outlier_summary[column] = len(outliers)

    return outlier_summary

# Menjalankan fungsi untuk dataset (tanpa kolom target)
indicators_columns = df.drop(columns=['risk_score'], errors='ignore')
outlier_counts = detect_outliers(indicators_columns)

# Menampilkan jumlah outlier per kolom
print("Jumlah outlier per kolom:")
for col, count in outlier_counts.items():
    print(f"{col}: {count}")
```
> Jumlah outlier per kolom:
> </br>ID: 0
> </br>Score: 81
> </br>Popularity: 0

Dalam proses eksplorasi data, ditemukan bahwa nilai outlier hanya teridentifikasi pada kolom Score. Oleh karena itu, fokus analisis terhadap outlier cukup diarahkan pada kolom Score saja.

![image](https://github.com/user-attachments/assets/8015d351-8c86-4876-a379-65ada4748c6f)

> IQR untuk kolom 'Score': 1.29
> </br>Batas bawah: 3.86
> </br>Batas atas : 9.02
> </br>Jumlah outlier pada kolom 'Score': 81

Outlier dalam data `Score` mencerminkan opini atau penilaian pengguna yang sangat ekstrim. Dalam konteks sistem rekomendasi, nilai-nilai ini penting untuk dipertahankan, karena membantu mengenali pola preferensi pengguna yang lebih jelas dan tajam.

### Data Cleaning
`df_clean = df.dropna()`

```
print(f"Jumlah baris sebelum data cleansing: {df.shape[0]}")
print(f"Jumlah baris sesudah data cleansing: {df_clean.shape[0]}")
```

> Jumlah baris sebelum data cleansing: 21460
> Jumlah baris sesudah data cleansing: 14465

Saya hanya menghapus data yang mengandung nilai kosong, karena sebagian besar data yang hilang terdapat pada kolom rating dan skor. Hal ini menunjukkan bahwa anime tersebut kemungkinan masih baru atau bahkan belum dirilis, sehingga dapat memengaruhi hasil analisis akhir.

### Ekstraksi Fitur
```
# Gabungkan kolom konten (Genre + Studio + Demographics) menjadi satu string deskriptif
df_clean['Content'] = df_clean['Genres'] + ', ' + df_clean['Demographics'].fillna('')
```

Kode ini digunakan untuk menggabungkan kolom Genres dan Demographics ke dalam satu kolom baru bernama Content. Tujuannya adalah menyatukan informasi deskriptif dari suatu anime agar dapat dianalisis sebagai satu kesatuan teks.

```
# Normalisasi teks genre (hilangkan null dan lowercase)
df_clean['Content'] = df_clean['Content'].fillna('').str.lower()
```

Setelah kolom Content terbentuk, langkah ini bertujuan untuk melakukan normalisasi teks dengan mengubah semua huruf menjadi huruf kecil (lowercase) dan menghilangkan nilai kosong. Ini penting untuk memastikan konsistensi format teks sebelum dilakukan proses vektorisasi.

```
from sklearn.feature_extraction.text import TfidfVectorizer

# TF-IDF Vectorization pada 'content'
tfidf = TfidfVectorizer(stop_words='english')
tfidf_matrix = tfidf.fit_transform(df_clean['Content'])

# Cek bentuk matriks
print(f"TF-IDF matrix shape: {tfidf_matrix.shape}")
```
> TF-IDF matrix shape: (14465, 32)

Blok kode ini menerapkan teknik TF-IDF untuk mengubah teks pada kolom Content menjadi bentuk numerik. TF-IDF digunakan untuk menilai seberapa penting suatu kata dalam deskripsi anime dibandingkan dengan seluruh koleksi data, sekaligus mengabaikan kata-kata umum (stop words) berbahasa Inggris.

Kemudian `print(f"TF-IDF matrix shape: {tfidf_matrix.shape}")` digunakan untuk menampilkan ukuran matriks TF-IDF yang terbentuk. Ukuran tersebut menunjukkan jumlah anime (baris) dan jumlah fitur unik (kata-kata penting) yang berhasil diekstraksi dari kolom Content.

# Modelling
## Cosine Similarity

```
from sklearn.metrics.pairwise import linear_kernel

# Hitung cosine similarity antar anime
cosine_sim = linear_kernel(tfidf_matrix, tfidf_matrix)
```

```
# Buat index judul ke index dataframe
indices = pd.Series(df_clean.index, index=df_clean['Title']).drop_duplicates()
```

```
# Fungsi sistem rekomendasi
def recommend(Title, num_recommendations=5):
    if Title not in indices:
        return f"Anime '{Title}' tidak ditemukan."

    idx = indices[Title]
    sim_scores = list(enumerate(cosine_sim[idx]))

    # Urutkan berdasarkan skor kemiripan
    sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True)
    sim_scores = sim_scores[1:num_recommendations+1]  # abaikan diri sendiri

    anime_indices = [i[0] for i in sim_scores]

    # Tampilkan kolom penting
    return df_clean[['Title', 'English', 'Content', 'Synopsis', 'Studios', 'Status', 'Rating', 'Score']].iloc[anime_indices].reset_index(drop=True)
```

Pada tahap modeling, sistem rekomendasi yang dibangun menggunakan pendekatan content-based filtering dengan algoritma cosine similarity sebagai inti perhitungannya. Algoritma ini digunakan untuk mengukur tingkat kemiripan antar item (dalam hal ini, anime) berdasarkan informasi konten yang telah direpresentasikan dalam bentuk vektor melalui teknik TF-IDF.

Cosine similarity bekerja dengan menghitung sudut (cosine) antara dua vektor dalam ruang berdimensi tinggi. Nilai cosine similarity berkisar antara 0 hingga 1, di mana nilai 1 menunjukkan dua vektor sangat mirip (arahnya sama), dan nilai 0 menunjukkan tidak ada kemiripan sama sekali. Dalam konteks ini, vektor yang dibandingkan adalah representasi teks dari kolom Content, yang berisi gabungan genre dan demografi masing-masing anime.

Dengan menggunakan fungsi linear_kernel dari pustaka sklearn, perhitungan cosine similarity dilakukan antar semua pasangan anime dalam dataset. Hasilnya adalah matriks kemiripan yang menyimpan nilai cosine antara setiap pasangan anime. Matriks ini menjadi dasar bagi sistem rekomendasi untuk menampilkan top-N recommendation, yaitu daftar anime yang paling mirip dengan judul yang diberikan pengguna berdasarkan skor kemiripan tertinggi.

## Inference
```
title = "Naruto"
rekomendasi = recommend(title)

print(f"Rekomendasi untuk '{title}':\n")
rekomendasi
```
> Rekomendasi untuk 'Naruto':
> | Title                   | English              | Content                              | Synopsis                                                                 | Studios       | Status           | Rating                      | Score |
> |-------------------------|----------------------|---------------------------------------|--------------------------------------------------------------------------|---------------|------------------|------------------------------|--------|
> | Hunter x Hunter (2011)  | Hunter x Hunter      | action, adventure, fantasy, shounen  | Hunters devote themselves to accomplishing hazardous tasks...            | Madhouse      | Finished Airing  | PG-13 - Teens 13 or older    | 9.041  |
> | Naruto: Shippuuden      | Naruto Shippuden     | action, adventure, fantasy, shounen  | It has been two and a half years since Naruto left...                    | Pierrot       | Finished Airing  | PG-13 - Teens 13 or older    | 8.241  |
> | One Piece               | One Piece            | action, adventure, fantasy, shounen  | Gol D. Roger was known as the "Pirate King," the strongest...            | Toei Animation| Currently Airing | PG-13 - Teens 13 or older    | 8.671  |
> | Nanatsu no Taizai       | The Seven Deadly Sins| action, adventure, fantasy, shounen  | In a world similar to the European Middle Ages...                        | A-1 Pictures  | Finished Airing  | PG-13 - Teens 13 or older    | 7.701  |
> | Bleach                  | Bleach               | action, adventure, fantasy, shounen  | Ichigo Kurosaki is an ordinary high schooler—until he...                | Pierrot       | Finished Airing  | PG-13 - Teens 13 or older    | 7.881  |

```
title = "Tenki no Ko"
rekomendasi = recommend(title)

print(f"Rekomendasi untuk '{title}':\n")
rekomendasi
```
> Rekomendasi untuk 'Tenki no Ko':
> | Title                          | English                        | Content                              | Synopsis                                                                 | Studios            | Status           | Rating                      | Score |
> |--------------------------------|--------------------------------|---------------------------------------|--------------------------------------------------------------------------|---------------------|------------------|------------------------------|--------|
> | ReLIFE                         | ReLIFE                         | drama, romance, slice of life, unknown | Dismissed as a hopeless loser by those around...                         | TMS Entertainment   | Finished Airing  | PG-13 - Teens 13 or older    | 7.981  |
> | Byousoku 5 Centimeter          | 5 Centimeters Per Second       | drama, romance, slice of life, unknown | What happens when two people love each other but...                      | CoMix Wave Films    | Finished Airing  | PG-13 - Teens 13 or older    | 7.591  |
> | Kimi no Suizou wo Tabetai     | I Want To Eat Your Pancreas    | drama, romance, slice of life, unknown | The aloof protagonist: a bookworm who is deeply...                       | Studio VOLN         | Finished Airing  | PG-13 - Teens 13 or older    | 8.561  |
> | Kotonoha no Niwa              | The Garden of Words            | drama, romance, slice of life, unknown | On a rainy morning in Tokyo, Takao Akizuki, an...                        | CoMix Wave Films    | Finished Airing  | PG-13 - Teens 13 or older    | 7.911  |
> | Josee to Tora to Sakana-tachi | Josee, the Tiger and the Fish  | drama, romance, slice of life, unknown | Equipped with his passion for diving and admiration...                   | Bones               | Finished Airing  | PG-13 - Teens 13 or older    | 8.441  |


Contoh diatas merupakan penerapan dari model `recomended` yang sudah kita buat sebelumnya. Ketika pengguna memasukkan judul anime sebagai input, sistem akan membandingkan vektor konten anime tersebut dengan seluruh anime lain dalam dataset, kemudian memberikan daftar top-N anime yang paling mirip berdasarkan skor kemiripan tertinggi.

Contohnya saya menjalankan kode kedua yang memberikan daftar rekomendasi anime/film yang mirip atau berkaitan dengan judul "Tenki no Ko" (Weathering with You). Rekomendasi tersebut disajikan dalam bentuk tabel dengan beberapa kolom yang mendeskripsikan setiap judul rekomendasi.

# Evaluation

### Metrik Evaluasi yang Digunakan

Pada evaluasi ini, digunakan dua metrik utama untuk menilai performa sistem rekomendasi, yaitu **Precision@k** dan **Recall@k**.

- **Precision@k**  
  Precision@k mengukur proporsi rekomendasi yang benar-benar relevan dari keseluruhan **k** rekomendasi yang diberikan.  
  Formula Precision@k adalah:
    
  $$\text{Precision@k} = \frac{|\text{Relevant Found}|}{k}$$ 

  Di mana:  
  - $$\text{Relevant Found}$$ adalah jumlah rekomendasi yang sesuai dengan ground truth (rekomendasi relevan).  
  - $$k$$ adalah jumlah rekomendasi yang dihasilkan oleh sistem.

- **Recall@k**  
  Recall@k mengukur proporsi rekomendasi relevan yang berhasil ditemukan oleh sistem dari total rekomendasi relevan yang ada dalam ground truth.  
  Formula Recall@k adalah:  

  $$\text{Recall@k} = \frac{|\text{Relevant Found}|}{|\text{Ground Truth}|}$$

  Di mana:  
  - $$|\text{Ground Truth}|$$ adalah jumlah total item relevan sebenarnya.

---

### Cara Kerja Evaluasi

```
ground_truth = {
    "Naruto": {"Bleach", "One Piece", "Hunter x Hunter (2011)", "Dragon Ball Z", "One Punch Man"},
}
```

```
def evaluate_recommendation(title, k=5):
    recommended_df = recommend(title, k)
    if isinstance(recommended_df, str):  # Jika error
        return recommended_df

    recommended_titles = set(recommended_df['Title'])
    true_relevant = ground_truth.get(title, set())

    if not true_relevant:
        return f"Tidak ada ground truth untuk '{title}'"

    relevant_found = recommended_titles.intersection(true_relevant)
    precision = len(relevant_found) / k
    recall = len(relevant_found) / len(true_relevant)

    # Cetak hasil
    print(f"\n🎯 Evaluasi untuk: {title}")
    print("-" * 40)
    print(f"Recommended Titles : {', '.join(recommended_titles)}")
    print(f"Ground Truth       : {', '.join(true_relevant)}")
    print(f"Relevant Found     : {', '.join(relevant_found) if relevant_found else 'None'}")
    print("-" * 40)
    print(f"✅ Precision@{k}     : {precision:.3f}")
    print(f"✅ Recall@{k}        : {recall:.3f}")
    print(f"📌 Found             : {len(relevant_found)} / {len(true_relevant)}")

    return None
```

```
evaluate_recommendation("Naruto", k=5)
```

> 🎯 Evaluasi untuk: Naruto
>  
> Recommended Titles : Nanatsu no Taizai, Hunter x Hunter (2011), Bleach, Naruto: Shippuuden, One Piece  
> Ground Truth       : One Punch Man, Hunter x Hunter (2011), Dragon Ball Z, One Piece, Bleach  
> Relevant Found     : Hunter x Hunter (2011), Bleach, One Piece
> 
> ✅ Precision@5     : 0.600  
> ✅ Recall@5        : 0.600  
> 📌 Found          : 3 / 5  

Fungsi `evaluate_recommendation` bekerja dengan cara sebagai berikut:

1. Memanggil fungsi `recommend(title, k)` untuk mendapatkan daftar rekomendasi sebanyak **k** item.
2. Mengubah daftar rekomendasi tersebut menjadi set untuk memudahkan pencarian irisan (intersection).
3. Mengambil data ground truth, yaitu daftar item relevan yang seharusnya direkomendasikan untuk judul tertentu.
4. Mencari irisan antara rekomendasi dan ground truth untuk mengetahui berapa banyak rekomendasi yang relevan ditemukan.
5. Menghitung nilai precision dan recall berdasarkan jumlah rekomendasi relevan yang ditemukan.
6. Menampilkan hasil evaluasi secara rinci, meliputi daftar rekomendasi, ground truth, item relevan yang ditemukan, serta nilai precision dan recall.

# Menjawab Pertanyaan

### 1. Bagaimana cara membuat sistem rekomendasi anime yang baik dan relevan berdasarkan preferensi pengguna dan karakteristik anime?

Untuk mengatasi permasalahan ini, dikembangkan fungsi recommend() yang mengimplementasikan sistem rekomendasi anime menggunakan pendekatan content-based filtering. Sistem ini menerima judul anime dari pengguna sebagai input, kemudian mencari anime lain yang mirip dengan input tersebut berdasarkan karakteristik seperti genre, dan demographics. Tingkat kemiripan antar anime dihitung dengan memperhatikan atribut-atribut tersebut agar rekomendasi yang diberikan sesuai dengan preferensi pengguna.

```
# Input dari pengguna
user_input = input("Masukkan judul anime: ")

# Jalankan rekomendasi
recommendations = recommend(user_input, num_recommendations=5)

# Tampilkan hasil
if isinstance(recommendations, str):
    print(recommendations)
else:
    print("\n🎉 Rekomendasi untuk:", user_input)
    display(recommendations)
```

> Masukkan judul anime: **Kaguya-sama wa Kokurasetai: Tensai-tachi no Renai Zunousen**

> 🎉 **Rekomendasi untuk: Kaguya-sama wa Kokurasetai: Tensai-tachi no Renai Zunousen**
> | Title                                      | English                          | Content       | Synopsis                                                                                      | Studios       | Status           | Rating                     | Score |
> |--------------------------------------------|---------------------------------|---------------|-----------------------------------------------------------------------------------------------|---------------|------------------|----------------------------|-------|
> | Kaguya-sama wa Kokurasetai? Tensai-tachi no Renai Zunousen | Kaguya-sama Love is War Season 2 | comedy, seinen | After a slow but eventful summer vacation, Shu...                                            | A-1 Pictures  | Finished Airing  | PG-13 - Teens 13 or older  | 8.651 |
> | Sakamoto Desu ga?                          | Haven't You Heard? I'm Sakamoto | comedy, seinen | Sophisticated, suave, sublime; all words which...                                             | Studio Deen   | Finished Airing  | PG-13 - Teens 13 or older  | 7.561 |
> | Grand Blue                                 | Grand Blue Dreaming              | comedy, seinen | Iori Kitahara moves to the coastal town of Izu...                                            | Zero-G        | Finished Airing  | PG-13 - Teens 13 or older  | 8.431 |
> | Himouto! Umaru-chan                        | Himouto! Umaru-chan             | comedy, seinen | People are not always who they appear to be, a...                                            | Doga Kobo     | Finished Airing  | PG-13 - Teens 13 or older  | 7.121 |
> | Asobi Asobase                             | Asobi Asobase Workshop of Fun   | comedy, seinen | During recess, Olivia, a foreign transfer stud...                                            | Lerche        | Finished Airing  | PG-13 - Teens 13 or older  | 8.191 |
 
Hasil rekomendasi untuk input "Kaguya-sama wa Kokurasetai: Tensai-tachi no Renai Zunousen" menampilkan anime dengan genre dan suasana yang mirip, seperti comedy dan seinen, dari studio terkenal, serta rating yang baik. Sistem ini mampu memberikan daftar anime yang sesuai tema dan kualitasnya, sehingga membantu pengguna menemukan anime baru yang cocok dengan selera mereka. Cara ini membuat sistem jadi mudah digunakan, tepat sasaran, dan membantu meningkatkan pengalaman menonton anime.

### 2. Apa genre anime yang paling populer di kalangan pengguna MyAnimeList?

![image](https://github.com/user-attachments/assets/54af4220-a2ba-4576-a976-41ca16d626bd)

Grafik ini menunjukkan bahwa genre anime yang paling banyak dibuat adalah Comedy, disusul oleh Action dan Fantasy, yang berarti banyak orang suka anime yang lucu dan penuh aksi. Genre seperti Adventure, Sci-Fi, dan Drama juga cukup populer, karena banyak yang tertarik dengan cerita petualangan, teknologi, dan emosi. Sementara itu, genre seperti Romance, Slice of Life, dan Supernatural masih cukup sering muncul, tapi tidak sebanyak genre utama. Genre seperti Horror, Sports, dan Ecchi punya jumlah yang lebih sedikit, mungkin karena peminatnya lebih sedikit juga. Ada juga kategori Unknown yang cukup besar, kemungkinan karena datanya belum lengkap atau belum diklasifikasikan dengan jelas.

# Referensi

[1] Kumar, V., Pathak, M., & Choudhury, T. (2020). *A Comparative Study of Recommender Systems in E-commerce*. International Journal of Advanced Computer Science and Applications, 11(2), 517–523. doi:10.14569/IJACSA.2020.0110265

[2] Ismail, M., Siregar, M. R., & Pinem, S. A. (2021). *Anime Recommendation System Based on Genre and Popularity Using Content-Based Filtering*. Journal of Computer Science and Engineering, 25(1), 34–42.
