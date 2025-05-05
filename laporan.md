# Laporan Proyek Machine Learning - Christian Wijaya

## **Domain Proyek**

Indonesia memimpin penjualan mobil di Asia Tenggara pada kuartal III tahun 2022 [1]. Indonesia juga merupakan produsen mobil terbesar kedua di Asia Tenggara [2]. Hal ini menunujukkan bahwa minat masyarakat Indonesia terhadap kendaraan satu ini cukup tinggi. 

Dipasaran saat ini terdapat berbagai macam mobil dengan tipe dan harga yang berbeda-beda. Mobil dengan tipe yang sama dapat memiliki harga yang berbeda-beda. Lantas hal apa saja yang menentukan harga mobil ?


## **Business Understanding**
Dengan mengetahui kriteria apa saja yang mempengaruhi harga mobil, prediksi harga pasar mobil dengan kriteria tertentu dapat dilakukan. Hal ini akan membantu penjual maupun pembeli mobil untuk memaksimalkan keuntungan.
### **Problem Statements**
- Apa saja fitur/kriteria yang mempengaruhi harga mobil?
- Berapa harga mobil dengan kriteria tertentu?

### **Goals**
- Mengetahui fitur/kriteria yang paling mempengaruhi harga mobil
- Membuat model machine learning untuk memprediksi harga mobil seakurat mungkin dari fitur-fitur yang ada. Kemudian memprediksi harga mobil dengan kriteria tertentu menggunakan model yang telah dibuat.

### **Solution statements**
- Mencari korelasi antara harga dengan fitur-fitur lain.
- Menggunakan 3 algoritma untuk mencapai solusi yang diinginkan. Kemudian menentukan algoritma yang memiliki hasil prediksi paling mendekati nilai sebenarnya

## **Data Understanding**
Data diambil dari situs web populer di Rusia dengan iklan penjualan mobil. Data dikumpulkan setiap jam dari seratus halaman pertama. Satu-satunya filter adalah wilayah yang akan dicari. Tanggal dan waktu pemilihan ditunjukkan di kolom "parse_date". Data yang digunakan pada proyek ini adalah data pada region 41.

Data bukan berasal dari Indonesia. Sehingga akan terdapat beberapa perbedaan, seperti dari harga (walau sudah diubah ke rupiah), jenis mobil dan sebagainya. Data ini digunakan sebagai gambaran umum.

Link data: [Kaggle : Car Sales](https://www.kaggle.com/datasets/ekibee/car-sales-information?select=region41_en.csv).

Data memiliki 1.498.740 baris dan 17 kolom. 
### Variabel-variabel pada Kaggle : Car Sales dataset adalah sebagai berikut:
- brand     : merk mobil
- name      : nama model mobil
- bodyType  : tipe body mobil
- color     : warna mobil
- fuelType  : jenis bahan bakar yan digunakan
- year      : tahun pembuatan mobil
- mileage   : jarak yang sudah ditempuh mobil
- transmission : jenis trasmisi
- power     : jumlah daya kuda
- price     : harga dalam mata uang Russia
- vehicleConfiguration : nama konfigurasi model mobil
- engineName   : nama mesin 
- engineDisplacement   : kapasitas mesin dalam liter
- date : tanggal publikasi iklan
- location : kota lokasi iklan
- link : link ke iklan
- parse_date : tanggal dan waktu penerimaan informasi tentang iklan

Dari 17 variable di atas terdapat 13 variable dengan tipe data object, 3 dengan tipe data float dan 1 variable dengan tipe data int.

### **Variable yang tidak diperlukan**
Ada 4 variable (kolom) yang berhubungan dengan iklan penjualan mobil, yaitu "date", "location", "link", "parse_date". 

Variable-variable tersebut akan dihapus (drop) kecuali "location". Tanggal dan link iklan seharusnya tidak akan mempengaruhi harga mobil.
Variable location tidak dihapus karena saya ingin mengetahui apakah location akan berpegaruh terhadap harga mobil.

### **Missing Value**
Berikut ini jumlah missing value pada setiap kolom:
- brand                        0
- name                         0
- bodyType                     0
- color                    49806
- fuelType                  6595
- year                    583041
- mileage                   6864
- transmission              7401
- power                    14240
- price                        0
- vehicleConfiguration    583041
- engineName              583234
- engineDisplacement      583874
- location                     0

Bisa dilihat bahwa jumlah missing value cukup banyak. Missing value harus segera diatasi supaya tidak akan memengaruhi proses analisis dan hasil akhir. Penanganan missing value yang digunakan kali ini adalah dengan cara menghilangkan baris-baris yang terdapat missing value.

Setelah missing value dihilangkan jumlah baris berkurang menjadi 878396 

### **Outliers**
Berikut ini adalah boxplot untuk mendeteksi outliers pada beberapa fitur numerik.

![boxplot year](https://user-images.githubusercontent.com/108395348/204117910-8270d934-c177-42ed-817d-2315250b4a77.png)

Gambar 1.1 Boxplot fitur year

![boxplot mileage](https://user-images.githubusercontent.com/108395348/204118031-7298a2c9-b356-4433-8d28-00fccbdb4603.png)

Gambar 1.2 Boxlpot fitur mileage

![boxplot power](https://user-images.githubusercontent.com/108395348/204118096-e2f60fce-7fb8-44b8-8678-68674ffda256.png)

Gambar 1.3 Boxplot fitur power

![boxplot price](https://user-images.githubusercontent.com/108395348/204118120-b33b924b-eb1e-4a81-be75-dbc61e288882.png)

Gambar 1.4 Boxplot fitur price


Pada kasus ini outliers ditangani dengan teknik IQR method. IQR adalah singkatan dari Inter Quartile Range. Untuk lebih memahami IQR silahkan klik [disini](https://statisticsbyjim.com/basics/interquartile-range/)

> IQR = Q3 - Q1

Outliers yang diidentifikasi oleh boxplot (boxplot outliers) didefinisikan sebagai data yang nilainya 1.5 QR di atas Q3 (batas atas) atau 1.5 QR di bawah Q1 (batas bawah)

> Batas atas = Q3 + 1.5 * IQR 

> Batas bawah = Q1 - 1.5 * IQR

Setelah nilai outliers dihilangkan jumlah baris data menjadi 800642

### **Univariate Analysis**
#### **Categorical Features**
![barchart brand](https://user-images.githubusercontent.com/108395348/204118378-9a9676a7-c250-4936-8994-b479ea753bb2.png)

Gambar 2.1 Grafik jumlah mobil pada setiap brand

Dari gambar 2.1 bisa dilihat bahwa mobil dengan brand Toyota berjumlah jauh lebih banyak dibandingkan brand lainnya.

![barchart bodyType](https://user-images.githubusercontent.com/108395348/204118669-627af9ab-9d26-40f0-9aba-986a7583c3a1.png)

Gambar 2.2 Grafik jumlah mobil pada setiap body type

Dari gambar 2.2 bisa dilihat bahwa sebagian besar mobil memiliki body type "jeep 5 doors"

![barchart color](https://user-images.githubusercontent.com/108395348/204118902-4a656a90-69b0-4278-8e41-cb01dc07c845.png))

Gambar 2.3 Grafik jumlah mobil pada setiap color

Dilihat dari gambar 2.3, sebagian besar mobil memiliki warna putih, abu-abu dan hitam.

![barchart fuelType](https://user-images.githubusercontent.com/108395348/204119010-2608eaf5-587f-4c0a-a3aa-96f5deea7b32.png)

Gambar 2.4 Grafik jumlah mobil pada setiap fuelType

Dilihat dari gambar 2.4, sebagian besar mobil berbahan bakar bensin.

![barchart transmission](https://user-images.githubusercontent.com/108395348/204119073-d6e831f2-104b-4a0a-b6ed-29a60eaa4991.png)

Gambar 2.5 Grafik jumlah mobil pada setiap tranmission

Dari gambar 2.5 bisa dilihat bahwa sebagian besar mobil memiliki transmisi automatic.

![barchart engineDisplacement](https://user-images.githubusercontent.com/108395348/204119159-90a31ba1-c2e3-43f8-98bd-526d75c73e7f.png)

Gambar 2.6 Grafik jumlah mobil pada setiap engineDisplacement

Dari gambar 2.6 bisa dilihat bahwa sebagian besar mobil memiliki kapasitas mesin 2.0 Liter disusul dengan yang berkapasitas mesin 1.5 Liter.

![barchart location](https://user-images.githubusercontent.com/108395348/204119252-e3e73e59-377e-44ad-b27b-23bdb1604be6.png)

Gambar 2.7 Grafik jumlah mobil pada setiap location

Dari gambar 2.7 bisa dilihat bahwa sebagian besar mobil berada pada daerah Petropavlovsk-Kamchatskij.


#### **Numerical Features**

![grafik numericalvalue](https://user-images.githubusercontent.com/108395348/204119306-2e3c1e37-6c58-48e4-9387-0b0b5eb69a9b.png)

Gambar 2.8 Grafik jumlah mobil pada fitur numerik

Berdasarkan gambar 2.8, bisa dilihat dari grafik price bahwa peningkatan harga mobil cenderung sebanding dengan penurunan jumlah sampel. Atau dapat dibilang semakin mahal mobil jumlahnya semakin sedikit.

### **Multivariate Analysis**
#### **Categorical Features**
Dari hasil multivariate analysis (lengkapnya bisa dilihat di file file Jupyter Notebook) bisa dibilang bahwa fitur-fitur ketegorikal memang mempengaruhi harga. Hanya 1 fitur yang rata-rata harga cenderung mirip, yaitu fitur fuelType (bisa dilihat pada gambar 3.1).

![grafik harga-fueltype](https://user-images.githubusercontent.com/108395348/204119446-7baeecac-76cf-48a4-92f2-0c3ec7b518d1.png)

Gambar 3.1 
Grafik Rata-rata harga terhadap fitur fuelType

#### **Numerical Features**

![pairplot fitur numerik](https://user-images.githubusercontent.com/108395348/204119868-c9c4843f-540f-49ba-b946-d09a50543858.png)

Gambar 3.2 Plot relasi masing-masing fitur numerik

![matriks korelasi fitur numerik](https://user-images.githubusercontent.com/108395348/204119948-c5073160-cb71-45ed-89ee-7a67c085abf6.png)

Gambar 3.3 Matriks Korelasi pada fitur numerik

Berdasarkan gambar 3.2 dan 3.3 bisa dilihat bahwa nilai korelasi antara fitur "year" dan "price" cukup tinggi. Sedangkan nilai korelasi antara fitur "mileage" dan "power" dengan price tidak terlalu tinggi.


## **Data Preparation**
### **Encoding Fitur Kategori**
Encoding adalah salah satu tahap praproses data sebelum diproses dengan algoritma machine learning. Dalam mengerjakan projek data science ataupun machine learning, kita akan sangat mungkin menemukan satu atau beberapa fitur yang bertipe kategori. Komputer tidak dapat memproses data bertipe kategori sehingga kita harus mengubah data tersebut menjadi berbentuk bilangan. Proses ini disebut dengan encoding.

Encoding yang dipakai kali ini adalah Label Encoding. Label encoding mengubah setiap nilai dalam kolom menjadi angka yang berurutan berdasarkan abjad alfabet.

### **Train-Test-Split**
Train/test split adalah salah satu metode yang dapat digunakan untuk mengevaluasi performa model machine learning. Metode evaluasi model ini membagi dataset menjadi dua bagian yakni bagian yang digunakan untuk training data dan untuk testing data dengan proporsi tertentu.

Proses ini dilakukan sebelum transformasi data, tujuannya adalah agar kita tidak mengotori data uji (test) dengan informasi yang kita dapat dari data latih (train). 

Proporsi pembagian data latih dan uji yang digunakan kali ini adalah 90:10

### **Standarisasi**
Standarisasi adalah teknik dalam melakukan perubahan skala, dimana data yang dimiliki akan diubah sehingga memiliki rata rata = 0 (terpusat) dan standar deviasi = 1.

Standarisasi membantu untuk membuat fitur data menjadi bentuk yang lebih mudah diolah oleh algoritma. 

Kali ini kita akan menggunakan teknik StandarScaler dari library Scikitlearn. StandardScaler melakukan proses standarisasi fitur dengan mengurangkan mean (nilai rata-rata) kemudian membaginya dengan standar deviasi untuk menggeser distribusi.  StandardScaler menghasilkan distribusi dengan standar deviasi sama dengan 1 dan mean sama dengan 0. Sekitar 68% dari nilai akan berada di antara -1 dan 1.


## **Modeling**
Pengembangan model kali ini akan menggunakan 3 algoritma. Dari 3 algoritma tersebut akan ditentukan algoritma mana yang memiliki hasil terbaik.
Algoritma yang digunakan:
1. K-Nearest Neighbor
2. Random Forest
3. Boosting Algorithm

### **Modeling dengan KNN**
KNN bekerja berdasarkan prinsip bahwa setiap titik data yang berdekatan satu sama lain akan berada di kelas yang sama. Dengan kata lain, KNN mengklasifikasikan titik data baru berdasarkan kemiripan. 

Dalam KNN ada 2 hal penting yang perlu diperhatikan, yaitu menentukan banyaknya k tetangga terdekat dan menentukan metrik jarak. Dua hal ini juga akan menjadi parameter dalam pembuatan model

#### **Kelebihan KNN**
1. Mudah diterapkan.
Mengingat kesederhanaan dan akurasi algoritma, KNN merupakan salah satu pengklasifikasi pertama yang sebaiknya dipelajari oleh data scientist pemula.

2. Mudah beradaptasi.
Saat sampel training baru ditambahkan, algoritma KNN menyesuaikan untuk ikut memperhitungkan data baru karena semua data pelatihan disimpan ke dalam memori.

3. Memiliki sedikit hyperparameter
KNN hanya membutuhkan nilai k dan metrik jarak, yang relatif lebih sedikit jika dibandingkan dengan algoritma machine learning lainnya.

#### **Kekurangan KNN**
1. Tidak berfungsi dengan baik pada dataset berukuran besar.
Untuk dataset berukuran besar, cost untuk menghitung jarak antara titik baru dan setiap titik yang ada sangat besar dan cenderung menurunkan kinerja algoritma.

2. Kurang cocok untuk dimensi tinggi.
Algoritma KNN tidak bekerja dengan baik pada data berdimensi tinggi karena dengan jumlah dimensi yang besar, menjadi sulit bagi algoritma untuk menghitung jarak di setiap dimensi.

3. Perlu penskalaan fitur.
Kita perlu melakukan penskalaan fitur (standarisasi dan normalisasi) sebelum menerapkan algoritma KNN ke kumpulan data apa pun. Jika kita tidak melakukannya, KNN dapat menghasilkan prediksi yang salah.

4. Sensitif terhadap noise data, missing values dan outliers.
KNN sensitif terhadap noise dalam dataset. Kita perlu secara manual memasukkan nilai yang hilang dan menghapus outlier.

#### **Menentukan banyaknya k tetangga terdekat**
Mendefinisikan k dapat menjadi tindakan penyeimbang karena nilai yang berbeda dapat menyebabkan overfitting atau underfitting. Nilai k yang lebih rendah dapat memiliki varians yang tinggi, tetapi bias yang rendah. Sedangkan nilai k yang lebih besar dapat menyebabkan bias yang tinggi dan varians yang lebih rendah. Secara keseluruhan, disarankan untuk memilih nilai k berupa angka ganjil untuk menghindari ikatan dalam klasifikasi.

#### **Menentukan metrik jarak**
 Untuk menentukan titik data mana yang paling dekat dengan titik kueri tertentu, jarak antara titik kueri dan titik data lainnya perlu dihitung. Metrik jarak ini membantu membentuk batasan keputusan, yang mengarahkan kueri partisi ke kelas yang berbeda. 

Untuk menemukan titik serupa terdekat, kita bisa menggunakan perhitungan jarak seperti Euclidean distance, Hamming distance, Manhattan distance dan Minkowski distance.


#### **Code yang digunakan:**

Untuk membuat model KNN, kita menggunakan KNeighborsRegressor dari library Scikit-learn [3]. 

Kita menggunakan k = 11 tetangga (dapat dilihat dari nilai n_neighbors) dan metric Euclidean untuk mengukur jarak antara titik.

Rumus metric Euclidean:
$$ d(x,y) = \sqrt{\sum_{i=1}^n (x_i - y_i)^2} $$

### **Modeling dengan Random Forest**

Random Forest adalah algoritma pembelajaran supervised. “Forest” yang dibangunnya adalah kumpulan decision tree, biasanya dilatih dengan metode “bagging”. Ide umum dari metode bagging adalah kombinasi model pembelajaran meningkatkan hasil keseluruhan. 

Sederhananya, Random Forest membangun beberapa decision tree dan menggabungkannya untuk mendapatkan prediksi yang lebih akurat dan stabil

#### **Kelebihan Random Forest**
1. Kuat terhadap data outlier.
2. Bekerja dengan baik dengan data non-linear.
3. Risiko overfitting lebih rendah.
4. Berjalan secara efisien pada kumpulan data yang besar.
5. Akurasi yang lebih baik daripada algoritma klasifikasi lainnya.

#### **Kekurangan Random Forest**
1. Random Forest cenderung bias saat berhadapan dengan variabel kategorikal.
2. Waktu komputasi pada dataset berskala besar relatif lambat
3. Tidak cocok untuk metode linier dengan banyak fitur sparse

#### **Code yang digunakan**
Untuk membuat model Random Forest, kita menggunakan RandomForestRegressor dari library Scikit-learn [4]. 

Berikut adalah penejelasan parameter-parameter yang digunakan pada model:
- n_estimator: jumlah trees (pohon) di forest. 
- max_depth: kedalaman atau panjang pohon. Ia merupakan ukuran seberapa banyak pohon dapat membelah (splitting) untuk membagi setiap node ke dalam jumlah pengamatan yang diinginkan.
- random_state: digunakan untuk mengontrol random number generator yang digunakan. 
- n_jobs: jumlah job (pekerjaan) yang digunakan secara paralel. Ia merupakan komponen untuk mengontrol thread atau proses yang berjalan secara paralel. n_jobs=-1 artinya semua proses berjalan secara paralel.

### **Modeling dengan Boosting Algorithm**
Algoritma yang menggunakan teknik boosting bekerja dengan membangun model dari data latih. Kemudian ia membuat model kedua yang bertugas memperbaiki kesalahan dari model pertama. Model ditambahkan sampai data latih terprediksi dengan baik atau telah mencapai jumlah maksimum model untuk ditambahkan. 

Seperti namanya, boosting, algoritma ini bertujuan untuk meningkatkan performa atau akurasi prediksi. Caranya adalah dengan menggabungkan beberapa model sederhana dan dianggap lemah (weak learners) sehingga membentuk suatu model yang kuat (strong ensemble learner)

Pada kasus ini, kita akan menggunakan metode adaptive boosting. Salah satu metode adaptive boosting yang terkenal adalah AdaBoost. Inti dari algoritma AdaBoost adalah memberikan suatu bobot lebih pada observasi yang tidak tepat (weak classification)

### **Kelebihan Boosting Algorithm**
- Kemudahan implementasi. 
Boosting memiliki algoritme yang mudah dipahami dan mudah ditafsirkan yang belajar dari kesalahannya. Algoritme ini tidak memerlukan pra-pemrosesan data apa pun, dan memiliki rutinitas bawaan untuk menangani data yang hilang. 

- Pengurangan bias. 
Bias adalah adanya ketidakpastian atau ketidakakuratan dalam hasil machine learning. Algoritme boosting menggabungkan beberapa pembelajar lemah dalam metode berurutan, yang secara iteratif meningkatkan pengamatan. Pendekatan ini membantu mengurangi bias tinggi yang umum terjadi pada model machine learning.

- Efisiensi komputasional.
Algoritme boosting memprioritaskan fitur yang meningkatkan akurasi prediktif selama pelatihan. Algoritme boosting dapat membantu mengurangi atribut data dan menangani set data besar secara efisien.

### **Kekurangan Boosting Algorithm**
- Kelemahan terhadap data outlier.
Model boosting rentan terhadap outlier atau nilai data yang berbeda dari set data lainnya. Karena setiap model mencoba untuk memperbaiki kesalahan pendahulunya, outlier dapat mengubah hasil secara signifikan.

- Implementasi waktu nyata (real time)
Algoritma boosting sulit digunakan untuk implementasi waktu nyata karena algoritmanya lebih kompleks dibandingkan proses lainnya. 

### **Code yang digunakan**
Untuk membuat model Adaboost, kita menggunakan AdaBoostRegressor dari library Scikit-learn [5]. 

Berikut adalah penejelasan parameter-parameter yang digunakan pada model:
- n_estimators: jumlah maksimum estimator di mana peningkatan dihentikan. 
- learning_rate: bobot yang diterapkan pada setiap regressor di masing-masing proses iterasi boosting.
- random_state: digunakan untuk mengontrol random number generator yang digunakan.

## **Evaluation**
Metrik evaluasi yang akan kita gunakan adalah RMSE (Root Mean Squared Error).
RMSE merupakan besarnya tingkat kesalahan hasil prediksi, dimana semakin kecil (mendekati 0) nilai RMSE maka hasil prediksi akan semakin akurat.

Cara Menghitung Root Mean Square Error (RMSE) adalah dengan menguadratkan selisih antara nilai sebenarnya dengan nilai prediksi pada setiap data. Kemudian jumlahkan seluruh nilai tersebut lalu bagi dengan banyaknya data. Selanjutnya akar kuadratkan hasil bagi tersebut.

Rumusnya adalah sebagai berikut:

$$ RMSE = \sqrt{\sum_{t=1}^n (A_t - F_t)^2 \over n} $$

Dimana :
- At = Nilai sebenarnya
- Ft = Nilai prediksi
- n = banyaknya data

Berikut ini adalah nilai RMSE dari setiap model:
|          | train         | test          |
|----------|---------------|---------------|
| KNN      | 31023.352455  | 31905.359027  |
| RF       | 29699.766703  | 30217.471719  |
| Boosting | 395273.934376 | 395752.050134 |

Nilai RMSE dari data train dan data test pada setiap model tidak berbeda jauh. Hal ini menunjukkan bahwa model sudah *good fit*

Dari tabel di atas bisa kita lihat bahwa model yang menggunakan algoritma Random Forest memiliki nilai RMSE terkecil, yaitu sebesar 29699.766703 pada data train dan 30217.471719 pada data test.

Nilai tersebut tidak berbeda jauh dengan model yang menggunakan algoritma K-Nearest Neighbor (KNN) yang memiliki nilai RMSE sebesar 31023.352455 pada data train dan 31905.359027 pada data test.

Dilihat dari nilai RMSE-nya model dengan algoritma Boosting memiliki performa yang yang jauh lebih buruk dibanding model lainnya.

Berikut ini beberapa nilai prediksi dari ketiga model:
| Data ke | nilai sebenarnya | presiksi KNN | prediksi RF | prediksi Boosting |
|---------|------------------|--------------|-------------|-------------------|
| 603808  | 380000           | 380000.0     | 380000.0    | 661322.0          |
| 336632  | 1500000          | 1500000.0    | 1500000.0   | 1427617.1         |
| 1057788 | 1900000          | 1976923.1    | 1941150.2   | 2137028.5         |
| 157571  | 790000           | 747692.3     | 754254.5    | 929413.0          |



## Kesimpulan
Kita telah mengetahui fitur apa saja yang memiliki pengaruh yang cukup besar maupun cukup kecil terhadap harga mobil dari proses Multivariate Analysis.

Kita juga telah membuat model machine learning untuk memprediksi harga mobil dengan fitur tertentu menggunakan 3 algoritma yang berbeda (K-Nearest Neighbor, Random Forest, Boosting).

Dalam kasus ini, model dengan algoritma Random Forest ditetapkan menjadi algoritma terbaik. Hal ini dikarenakan model tersebut memilik nilai RMSE terkecil diantara model lainnya, yaitu sebesar 29699.766703 pada data train dan 30217.471719 pada data test.

Masih terdapat ruang untuk meningkatkan performa model. Salah satu caranya adalah dengan melakukan *hyperparameter tuning*.

Referensi :

[1] Annur, Cindy Mutia. 2022. "Indonesia Pimpin Penjualan Mobil di Asia Tenggara pada Kuartal III 2022". Tersedia: [tautan](https://databoks.katadata.co.id/datapublish/2022/11/09/indonesia-pimpin-penjualan-mobil-di-asia-tenggara-pada-kuartal-iii-2022). Diakses pada: November 2022.

[2] Annur, Cindy Mutia. 2022. "Bukan Indonesia, Ini Produsen Mobil Terbesar di Asia Tenggara". Tersedia: [tautan](https://databoks.katadata.co.id/datapublish/2022/11/09/bukan-indonesia-ini-produsen-mobil-terbesar-di-asia-tenggara). Diakses pada: November 2022.

[3] Scikit-learn Documentation. Tersedia: [tautan](http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsRegressor.html#). Diakses pada: November 2022.

[4] Scikit-learn Documentation. Tersedia: [tautan](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html#examples-using-sklearn-ensemble-randomforestregressor). Diakses pada: November 2022.

[5] Scikit-learn Documentation. Tersedia: [tautan](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.AdaBoostRegressor.html#). Diakses pada: November 2022.



