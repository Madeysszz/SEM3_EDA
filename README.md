# EDA 2025: Faktor Meteorologi dan Kasus Influenza

Proyek **Analisis Data Eksploratif** oleh kelompok **Asking Bertanya** untuk mengeksplorasi hubungan faktor meteorologi dengan jumlah kasus influenza berdasarkan kelompok usia. Repositori ini juga memuat eksperimen peramalan jumlah kasus menggunakan ARIMA, SARIMA, SARIMAX, dan LSTM.

## Fokus Analisis

- Memahami struktur data, distribusi variabel, missing value, dan pola pencilan.
- Mengeksplorasi hubungan kondisi cuaca dengan kasus influenza secara keseluruhan, menurut tipe influenza, dan menurut kelompok usia.
- Mempelajari pola deret waktu serta mencoba model statistik dan deep learning untuk memprediksi jumlah kasus.

## Isi Repositori

| Notebook | Isi utama |
|---|---|
| [Github_AskingBertanya.ipynb](Github_AskingBertanya.ipynb) | Persiapan data, analisis univariat, bivariat, dan multivariat; histogram, boxplot, scatterplot, serta korelasi Spearman. |
| [ASKINK_NEW.ipynb](ASKINK_NEW.ipynb) | Eksplorasi deret waktu, dekomposisi, uji ADF, ACF/PACF, eksperimen ARIMA/SARIMA/SARIMAX, diagnostik residual, dan evaluasi peramalan. |
| [EDA_ASKINGBERTANYA_LSTM.ipynb](EDA_ASKINGBERTANYA_LSTM.ipynb) | Eksperimen LSTM dengan prediktor cuaca dan LSTM univariat menggunakan riwayat jumlah kasus. |

## Data yang Dibutuhkan

**Berkas dataset belum disertakan dalam repositori.** Masing-masing notebook membaca berkas berikut:

| Notebook | Berkas input yang dirujuk kode |
|---|---|
| `Github_AskingBertanya.ipynb` | `/content/DATA_ASLI.xlsx` |
| `ASKINK_NEW.ipynb` | `databaru_eda.csv` |
| `EDA_ASKINGBERTANYA_LSTM.ipynb` | `data_asking.csv` |

Ketiga input perlu disiapkan sesuai kebutuhan masing-masing notebook. Proses pembentukan kedua CSV dari berkas Excel belum didokumentasikan dalam repositori.

### Variabel yang Digunakan

| Kelompok | Nama kolom dalam analisis |
|---|---|
| Waktu | `Date` |
| Jumlah kasus | `Total` |
| Tipe influenza | `Influneza A`, `Influneza B` |
| Jenis kelamin | `Male`, `Female` |
| Kelompok usia | `BALITA`, `ANAK_ANAK`, `REMAJA`, `DEWASA`, `LANSIA` |
| Faktor cuaca | `Temperature`, `Relative humidity`, `Wind speed`, `Rainfall`, `Sunshine duration`, `Vapour pressure` |
| Suhu tambahan pada EDA | `Minimum Temperature`, `Maximum Temperature` |

Nama `Influneza A` dan `Influneza B` mengikuti ejaan pada kode. Dalam notebook EDA, kolom awal `A0k_A0k` diubah menjadi `ANAK_ANAK` dan `Lansia` menjadi `LANSIA`.

Notebook EDA juga membentuk `Kategori_Rentan` menggunakan kelompok usia dengan jumlah kasus tertinggi pada setiap baris. Label ini menggambarkan jumlah kasus dalam observasi tersebut; perhitungan tidak menggunakan ukuran populasi setiap kelompok usia.

## Alur Analisis

### Eksplorasi Data

- Pemeriksaan tipe data, missing value, dan statistik deskriptif.
- Pembersihan spasi pada nama kolom, konversi tanggal, serta konversi beberapa variabel cuaca menjadi numerik.
- Analisis distribusi melalui histogram, boxplot, skewness, dan kurtosis.
- Visualisasi hubungan cuaca dengan jumlah kasus melalui scatterplot.
- Analisis multivariat menggunakan matriks dan heatmap korelasi Spearman.

### Deret Waktu

Notebook `ASKINK_NEW.ipynb` mengeksplorasi deret `Total` melalui plot waktu, rolling mean dan standard deviation, dekomposisi musiman, uji Augmented Dickey-Fuller, differencing, serta ACF/PACF.

Eksperimen mencakup ARIMA dan model musiman. Konfigurasi SARIMA yang digunakan dalam evaluasi adalah `order=(3, 0, 1)` dan `seasonal_order=(3, 1, 1, 7)`. SARIMAX memakai konfigurasi tersebut dengan variabel eksogen `Relative humidity`, `Wind speed`, `Sunshine duration`, dan `Vapour pressure`.

### LSTM

| Pengaturan | Dengan prediktor cuaca | Univariat |
|---|---|---|
| Input | Riwayat enam variabel cuaca | Riwayat `Total` |
| Panjang jendela | 30 observasi | 90 observasi |
| Target | `Total` pada observasi berikutnya | `Total` pada observasi berikutnya |
| Arsitektur | LSTM 128 → LSTM 64 → Dense 32 → Dense 1 | LSTM 128 → LSTM 64 → Dense 32 → Dense 1 |
| Optimizer / loss | Adam / MSE | Adam / MSE |
| Epoch / batch size | 150 / 32 | 150 / 32 |

## Evaluasi dalam Notebook

| Eksperimen | Pembagian yang tertulis dalam kode | Metrik yang dihitung |
|---|---|---|
| SARIMA | 14 observasi terakhir sebagai test | MAE, MSE, RMSE, MAPE |
| SARIMAX | 14 observasi terakhir sebagai test, dengan eksogen pada periode test | MAE, MSE, RMSE, MAPE |
| LSTM dengan cuaca | 80% train dan 20% test, `shuffle=False`; `validation_split=0.1` saat training | MSE, MAE |
| LSTM univariat | 80% train dan 20% test, `shuffle=False` | MAE |

Skema tersebut mengikuti urutan baris. Kode belum mengurutkan tanggal secara eksplisit, sehingga urutan waktu input perlu dipastikan terlebih dahulu. Output eksperimen tersimpan dalam notebook, tetapi belum direproduksi ulang untuk dokumentasi ini.

## Menyiapkan Analisis

1. Unduh repositori atau clone melalui terminal:

   ```bash
   git clone https://github.com/Madeysszz/EDA_2025.git
   cd EDA_2025
   ```

2. Buka notebook yang ingin dipelajari pada lingkungan notebook Python 3.
3. Siapkan dataset sesuai tabel input dan sesuaikan lokasi pembacaan berkas. Path `/content/DATA_ASLI.xlsx` pada notebook EDA perlu disesuaikan jika dijalankan di lingkungan lain.
4. Pastikan dependensi yang diimpor tersedia: **pandas, NumPy, Matplotlib, seaborn, SciPy, statsmodels, scikit-learn, dan TensorFlow/Keras**. Pembacaan Excel juga memerlukan engine yang sesuai pada lingkungan pandas.
5. Perhatikan catatan reproduksibilitas berikut sebelum menjalankan seluruh sel secara berurutan.

## Status Reproduksibilitas

- Dataset dan daftar versi dependensi belum tersedia; eksekusi dari awal sampai akhir belum diverifikasi.
- Notebook LSTM menggunakan `scaler_X` dan `scaler_Y` tanpa inisialisasi pada sel sebelumnya, sehingga membutuhkan perbaikan sebelum dapat dijalankan dari kernel bersih.
- Scaling LSTM dilakukan sebelum pembagian train/test. Imputasi rata-rata `Sunshine duration` pada notebook SARIMAX juga memakai seluruh data sebelum pembagian. Keduanya berpotensi membocorkan informasi test; evaluasi perlu diulang dengan preprocessing yang hanya di-fit pada train.
- Output evaluasi SARIMAX menyimpan peringatan konvergensi. Contoh ramalan 21 langkah juga memakai ulang eksogen historis sebagai placeholder, sehingga belum mewakili ramalan dengan informasi cuaca masa depan yang tersedia secara nyata.
- Seed dan perangkat training LSTM belum dicatat. Perbedaan input data dan panjang jendela juga perlu diperhatikan sebelum membandingkan skor antareksperimen.

## Anggota Kelompok

**Asking Bertanya**

| Nama | NPM |
|---|---|
| Nino Ahmadiy | 24083010049 |
| Muhammad Shaquille Syafiq | 24083010071 |
| M. Rafly Wahyudi | 24083010078 |
| Dani Shofi Nur Izza | 24083010124 |

Dokumentasi disusun berdasarkan kode dan output tersimpan pada notebook di [Madeysszz/EDA_2025](https://github.com/Madeysszz/EDA_2025).
