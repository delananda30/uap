# RPS Prediction using VGG19 Model

Repositori ini berisi proyek menggunakan model VGG19 yang telah dilatih sebelumnya untuk memprediksi gestur tangan Batu-Gunting-Kertas (RPS). Proyek ini juga mencakup penerapan model menggunakan Flask, yang memungkinkan pengguna berinteraksi dengan sistem prediksi melalui antarmuka web.

## Dataset

![Dataset](https://github.com/delananda30/rpsprediction/assets/71807981/274e1e6e-ab01-4fdb-b9a8-8ba6467647d4)

Dalam proyek ini, digunakan dataset berupa kumpulan citra gestur tangan yang mencakup tiga kategori utama: batu, gunting, dan kertas. Citra-citra ini merepresentasikan visual dari berbagai pose tangan, yang menjadi dasar pelatihan untuk model prediksi.

Dataset pelatihan terdiri dari 2520 sampel, yang dibagi menjadi subset training (75%), validation (15%), dan test (10%). Rincian jumlah data untuk masing-masing subset adalah sebagai berikut:
- **Training Set**: 1890 data
- **Validation Set**: 378 data
- **Test Set**: 252 data

![Augmentasi](https://github.com/delananda30/rpsprediction/assets/71807981/67d18abe-44df-4c38-be0a-d5ba1584253a)

Untuk memperkaya dataset, dilakukan augmentasi dengan mengaplikasikan variasi gambar. Tujuan dari augmentasi adalah membantu model untuk lebih baik dalam mengenali pola yang berbeda dalam kelas-kelas tersebut. Proses augmentasi diimplementasikan untuk memastikan bahwa model tidak hanya belajar dari contoh yang sama tetapi juga mampu mengatasi variasi yang mungkin terjadi dalam situasi dunia nyata.

## Model
VGG19 (Visual Geometry Group 19) adalah salah satu arsitektur model Deep Convolutional Neural Network (CNN) yang dikembangkan oleh Karen Simonyan dan Andrew Zisserman pada tahun 2014. Model ini memiliki 19 layer (hence the name) dan sangat terkenal karena keefektifannya dalam mengatasi tugas pengenalan gambar.

Base model menggunakan arsitektur VGG19 yang telah dilatih sebelumnya dengan bobot dari dataset ImageNet. Lapisan fully connected pada bagian atas dihapus dan diganti dengan lapisan baru untuk tujuan proyek, yaitu prediksi gestur tangan RPS.

Berikut adalah struktur model beserta visualisasi plot model:
```plaintext
Model: "sequential"
_________________________________________________________________
Layer (type)                Output Shape              Param #   
=================================================================
vgg19 (Functional)          (None, 7, 7, 512)         20024384  
                                                                 
global_average_pooling2d (  (None, 512)               0         
GlobalAveragePooling2D)                                         
                                                                 
dense (Dense)               (None, 128)               65664     
                                                                 
dense_1 (Dense)             (None, 3)                 387       
                                                                 
=================================================================
Total params: 20,090,435 (76.64 MB)
Trainable params: 66,051 (258.01 KB)
Non-trainable params: 20,024,384 (76.39 MB)
_________________________________________________________________
```
Struktur model ini terdiri dari tiga lapisan utama, yaitu VGG19 sebagai base model, lapisan Global Average Pooling untuk meratakan output, dan dua lapisan Dense (fully connected) untuk klasifikasi tiga kategori gestur tangan: batu, gunting, dan kertas.

<p align="center">
  <img src="https://github.com/delananda30/rpsprediction/assets/71807981/aa17a316-774b-4d4f-9792-0a567b90c5d3" alt="Plot Model">
</p>

Visualisasi plot model memberikan gambaran intuitif mengenai arsitektur dan hubungan antar lapisan dalam model tersebut.

Model dilatih dengan optimizer Adam dan fungsi loss categorical_crossentropy. Proses pelatihan dilakukan selama 10 epoch. Berikut adalah beberapa hasil pelatihan:

```plaintext
Epoch 1/10
60/60 [==============================] - 63s 885ms/step - loss: 0.9585 - accuracy: 0.6021 - val_loss: 0.7845 - val_accuracy: 0.7963
...
Epoch 10/10
60/60 [==============================] - 46s 768ms/step - loss: 0.1384 - accuracy: 0.9614 - val_loss: 0.1320 - val_accuracy: 0.9683
```
Berikut adalah grafik untuk menunjukkan perubahan loss dan akurasi pada setiap epoch selama pelatihan:
![Grafik Training](https://github.com/delananda30/rpsprediction/assets/71807981/0ab982e2-4969-464d-919c-46d96aa5246c)

Hasil evaluasi pada subset test menunjukkan tingkat akurasi yang tinggi:
```plaintext
Test Loss: 0.08905871212482452
Test Accuracy: 0.9920634627342224
```
Model dievaluasi lebih lanjut dengan melakukan prediksi pada subset test dan menghasilkan classification report:

```plaintext
              precision    recall  f1-score   support

       paper       0.99      0.99      0.99        84
        rock       1.00      0.99      0.99        84
    scissors       0.99      1.00      0.99        84

    accuracy                           0.99       252
   macro avg       0.99      0.99      0.99       252
weighted avg       0.99      0.99      0.99       252
```
Classification report pada subset test menegaskan kinerja yang sangat baik, dengan presisi, recall, dan f1-score di atas 99% untuk setiap kelas (paper, rock, scissors).

Secara keseluruhan, model ini menggunakan konsep transfer learning dan arsitektur VGG19 untuk mencapai akurasi di atas 99% setelah pelatihan dan validasi. Keberhasilan ini menunjukkan kemampuan model dalam mengklasifikasikan gestur tangan pada permainan Batu-Gunting-Kertas.

## Aplikasi Web
Repository ini berisi aplikasi web untuk memprediksi gestur tangan Batu-Gunting-Kertas (RPS) menggunakan model VGG19 yang telah dilatih sebelumnya. Struktur proyek ini mencakup direktori dan file-file berikut:

1. **_pycache_**: Direktori ini dapat berisi file Python yang telah dikompilasi (`.pyc`) yang dihasilkan oleh interpreter Python. Aman untuk mengabaikan folder ini saat berbagi atau melakukan commit ke version control.

2. **model**: Direktori ini menyimpan model yang telah dilatih dalam format file Hierarchical Data Format (HDF5) dengan ekstensi `.h5`.

3. **static**: Direktori ini berisi aset statis seperti Cascading Style Sheets (CSS) dan sumber daya tambahan.

   - **css**: Subdirektori ini berisi stylesheet CSS untuk mempercantik halaman web.
   - **assets**: Subdirektori ini menyimpan aset-aset lainnya, seperti gambar.

4. **templates**: Direktori ini mencakup templat HTML untuk merender halaman-halaman web.

   - **index.html**: Halaman utama untuk mengunggah gambar yang akan diprediksi.
   - **result.html**: Halaman yang menampilkan hasil prediksi beserta akurasi dan waktu prediksi.

5. **app.py**: Skrip Python ini berfungsi sebagai berkas aplikasi utama. Ini menggunakan kerangka kerja web Flask untuk membuat dan menjalankan aplikasi web. Skrip ini mencakup rute-rute yang diperlukan untuk menangani permintaan pengguna, mengunggah gambar, dan melakukan prediksi.

6. **requirements.txt**: Berkas ini mencantumkan paket-paket Python dan versi-versi mereka yang diperlukan untuk menjalankan aplikasi. Pasang dependensi ini menggunakan perintah `pip install -r requirements.txt`.

## Cara Menjalankan Aplikasi Web

1. Clone repository:
   ```bash
   git clone https://github.com/delananda30/rpsprediction.git
   cd rpsprediction
   ```

2. Pasang dependensi yang dibutuhkan:
   ```bash
   pip install -r requirements.txt
   ```

3. Jalankan aplikasi Flask:
   ```bash
   flask run
   ```

4. Buka web browser Anda dan kunjungi [http://localhost:5000](http://localhost:5000) untuk mengakses Aplikasi Web Prediksi Batu-Gunting-Kertas (RPS).

Silakan sesuaikan konten, gaya, dan fungsionalitas berdasarkan preferensi dan kebutuhan proyek Anda.

## Tampilan Aplikasi Web

Berikut adalah tampilan dari aplikasi web Prediksi Batu-Gunting-Kertas (RPS):

![Tampilan Utama](https://github.com/delananda30/rpsprediction/assets/71807981/57c9c70a-0a44-4fff-a404-8ba100cbcabd)

![Hasil Prediksi](https://github.com/delananda30/rpsprediction/assets/71807981/bce4a427-45e2-40e0-bcbd-859b71a5ad02)

Anda dapat melihat antarmuka pengguna yang sederhana dan intuitif untuk mengunggah gambar dan melihat hasil prediksi gestur tangan RPS.

## Authors
- [Dela Ananda Setyarini](https://github.com/delananda30)
