# EkoProject
For learning purposes

This repository was created for the purpose of completing the Mid-Semester Exam (UTS) assignment in the Data Mining course. This project contains the implementation of program code using the Python language developed based on a specific case study that has been determined. All codes, documentation, and folder structures have been arranged in such a way that they are easy to understand, run, and modify by anyone who accesses this repository.

The purpose of this project is to train students' understanding of programming concepts, algorithm logic, and practical application of the theories that have been learned during lectures. This repository also aims to be a learning portfolio that can be accessed publicly via GitHub.

Thank you.

------------------------------------------------------------------------------------------------------------------------------------
Penjelasan Lengkap Kode Program Python
Project ini bertujuan untuk melakukan klasifikasi jenis serangan jaringan (Dari file yang saya saya pakai) dengan menggunakan algoritma Decision Tree Classifier pada data yang diperoleh dalam bentuk file CSV dari Google Drive. Dataset yang digunakan berisi data serangan seperti MITM ARP Spoofing, DoS ICMP Flood, dan MQTT DoS Publish Flood. Berikut adalah penjelasan mendalam dari masing-masing bagian kode:

1. Import Library yang Dibutuhkan
   ```python
   import pandas as pd
Baris ini mengimpor library pandas, yang merupakan library utama untuk melakukan manipulasi dan analisis data berbasis tabel (DataFrame). Library ini digunakan untuk membaca file CSV dan melakukan operasi penggabungan data, seleksi kolom, dan lainnya.

2. Mount Google Drive
   ```python
   from google.colab import drive
   drive.mount('/content/drive')
Kode ini digunakan ketika program dijalankan di Google Colab. Fungsinya adalah menghubungkan Google Colab dengan akun Google Drive, sehingga file yang berada di Drive saya bisa diakses langsung dari Colab. Setelah perintah ini dijalankan, pengguna akan diminta otorisasi untuk mengakses Drive-nya.

4. Membaca Dataset CSV dari Google Drive
   ```python
   folder_path = '/content/drive/My Drive/Dataset/'
   file1 = pd.read_csv(folder_path + 'MITM ARP Spoofing.csv')
   file2 = pd.read_csv(folder_path + 'DoS ICMP Flood.csv')
   file3 = pd.read_csv(folder_path + 'MQTT DoS Publish Flood.csv')
Di sini, tiga file CSV dibaca menggunakan fungsi pd.read_csv(). File-file ini berisi data dari tiga jenis serangan yang berbeda. Semua file berada dalam folder bernama Dataset yang terletak di direktori utama Google Drive pengguna.

4. Menggabungkan Ketiga Dataset Menjadi Satu
   ```python
   DataConcatenation = pd.concat([file1, file2, file3], ignore_index=True)
Data dari ketiga file CSV kemudian digabungkan menjadi satu kesatuan menggunakan fungsi pd.concat(). Opsi ignore_index=True digunakan agar indeks dari data baru direset ulang secara otomatis mulai dari nol.

5. Memisahkan Kolom Fitur dan Target (Label)
   ```python
   x = DataConcatenation.iloc[:, 7:76]
   y = DataConcatenation.iloc[:, 83:84]
- x adalah variabel yang menyimpan fitur (feature) atau atribut yang akan digunakan sebagai input ke model machine learning. Di sini, fitur diambil dari kolom ke-7 hingga kolom ke-75.
- y adalah label (target output) dari dataset, yang berisi jenis serangan yang ingin diprediksi. Diambil dari kolom ke-83.


6. Membagi Dataset Menjadi Data Latih dan Uji
   ```python
   from sklearn.model_selection import train_test_split
   x_train, x_test, y_train, y_test = train_test_split(x, y, test_size=0.2, random_state=42)
   
Data yang telah dipisahkan kemudian dibagi lagi menjadi dua bagian, yaitu:
- Data training (pelatihan): digunakan untuk melatih model machine learning.
- Data testing (pengujian): digunakan untuk mengevaluasi performa model.

Fungsi ```train_test_split``` dari sklearn digunakan untuk proses ini, dengan test_size=0.2 artinya 20% data digunakan sebagai data uji, dan random_state=42 untuk memastikan hasil pembagian tetap konsisten.

7. Membuat dan Melatih Model Decision Tree
   ```python
   from sklearn.tree import DecisionTreeClassifier

   q = DecisionTreeClassifier()
   q.fit(x_train, y_train)
   y_pred = q.predict(x_test)
Kode ini membuat model klasifikasi dengan algoritma Decision Tree. Model ini dilatih menggunakan data training (x_train, y_train) dan kemudian digunakan untuk memprediksi data testing (x_test) yang hasilnya disimpan dalam y_pred.

8. Menghitung dan Menampilkan Akurasi Model
   ```python
   from sklearn.metrics import accuracy_score

   accuracy = accuracy_score(y_test, y_pred)
   accuracy_percent = round(accuracy * 100, 2)
   print(f"Akurasi: {accuracy_percent}%")
Akurasi dihitung menggunakan accuracy_score, yang membandingkan hasil prediksi dengan data sebenarnya. Nilai akurasi kemudian diubah menjadi persentase dan ditampilkan di konsol.

9. Visualisasi Pohon Keputusan
   ```python
   import matplotlib.pyplot as plt
   from sklearn import tree
   import numpy as np

   fig = plt.figure(figsize=(10, 7))
   tree.plot_tree(q, feature_names=x.columns.values, class_names=np.array(['MITM ARP Spoofing', 'DoS ICMP Flood', 'MQTT DoS Publish Flood']), filled=True)
   plt.show()
Pohon keputusan yang terbentuk divisualisasikan menggunakan fungsi plot_tree dari library sklearn.tree. Ini membantu pengguna memahami bagaimana model memutuskan sebuah prediksi berdasarkan fitur yang tersedia.

10. Membuat Confusion Matrix dan Visualisasi Heatmap
    ```python
    import seaborn as sns
    from sklearn import metrics

    label = np.array(['MITM ARP Spoofing', 'DoS ICMP Flood', 'MQTT DoS Publish Flood'])
    conf_matrix = metrics.confusion_matrix(y_test, y_pred)

    plt.figure(figsize=(10, 10))
    sns.heatmap(conf_matrix, annot=True, xticklabels=label, yticklabels=label, cmap='Blues')
    plt.xlabel('Prediksi')
    plt.ylabel('Fakta')
    plt.title('Confusion Matrix')
    plt.show()
Confusion Matrix digunakan untuk mengevaluasi performa klasifikasi dengan melihat jumlah prediksi benar dan salah dari masing-masing kelas. Heatmap yang dibuat dari seaborn memberikan tampilan visual yang mudah dipahami.
