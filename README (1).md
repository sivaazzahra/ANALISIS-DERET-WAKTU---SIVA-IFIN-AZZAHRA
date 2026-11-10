# Analisis Intervensi pada Peramalan Konsumsi Beras Kabupaten Bojonegoro

**Perbandingan Model RegARIMA dan Double Exponential Smoothing**

Mata Kuliah Analisis Deret Waktu — Program Studi Sains Data
Universitas Pembangunan Nasional Veteran Jawa Timur

---

## Tentang

Repositori ini memuat tahap awal penelitian, yaitu uji stasioneritas terhadap data
neraca beras Kabupaten Bojonegoro periode Januari 2021 sampai Desember 2023.

Seluruh variabel pada neraca beras diuji untuk menentukan mana yang layak dimodelkan
sebagai deret waktu, dibuktikan ketidakstasionerannya melalui uji formal dan bukti
visual, lalu ditetapkan ordo differencing yang diperlukan.

| Keterangan | Isi |
|---|---|
| Sumber data | BPS Kabupaten Bojonegoro |
| Periode | Januari 2021 – Desember 2023 |
| Jumlah observasi | 36 bulan |
| Jumlah variabel diuji | 5 |
| Variabel terpilih | Konsumsi Beras (ton) |

---

## Hasil Penapisan Variabel

Dua uji dengan arah hipotesis berlawanan digunakan agar kesimpulannya saling
mengonfirmasi.

| Uji | Hipotesis nol | Tidak stasioner bila |
|---|---|---|
| Augmented Dickey-Fuller (ADF) | Terdapat unit root | p-value > 0,05 |
| Kwiatkowski-Phillips-Schmidt-Shin (KPSS) | Deret stasioner | p-value < 0,05 |

| Variabel | ADF p | KPSS p | Kesimpulan |
|---|---|---|---|
| Luas Panen (ha) | 0,0000 | 0,1000 | Stasioner |
| Produksi Gabah (ton) | 0,0000 | 0,1000 | Stasioner |
| Produksi Beras (ton) | 0,0000 | 0,1000 | Stasioner |
| **Konsumsi Beras (ton)** | **0,8910** | **0,0171** | **Tidak stasioner** |
| Surplus/Defisit (ton) | 0,0000 | 0,1000 | Stasioner |

Dari lima variabel, hanya konsumsi beras yang tidak stasioner. Empat variabel lain
berfluktuasi tajam mengikuti musim panen namun selalu kembali ke rata-rata yang sama.

---

## Hasil Uji Transformasi

| Transformasi | ADF p | KPSS p | Kesimpulan |
|---|---|---|---|
| Data asli (level) | 0,8910 | 0,0171 | Tidak stasioner |
| **Differencing 1 (d=1)** | **0,0000** | **0,1000** | **Stasioner** |
| Differencing 2 (d=2) | 0,0000 | 0,0417 | Overdifferencing |
| Log lalu differencing 1 | 0,0000 | 0,1000 | Setara dengan d=1 |

Ordo differencing yang digunakan: **d = 1**. Differencing kedua menyebabkan uji KPSS
kembali menolak kestasioneran, sehingga tergolong overdifferencing. Transformasi log
memberi hasil setara dengan differencing biasa, sehingga tidak diperlukan.

---

## Temuan atas Struktur Data

Terdapat patahan struktural berupa lompatan **616 ton** antara Desember 2022 dan
Januari 2023, sementara perubahan bulanan normal hanya berkisar 1 sampai 15 ton.
Lompatan ini berasal dari pemutakhiran basis data jumlah penduduk, bukan dari
perubahan perilaku konsumsi, karena angka konsumsi dihitung dari jumlah penduduk
dikalikan konsumsi beras per kapita.

Pengujian dengan variabel dummy intervensi menghasilkan koefisien 612,74 dengan
t-statistik 213,27 dan p-value di bawah 0,001, serta R² sebesar 0,9993. Patahan
tersebut menjelaskan hampir seluruh variasi perubahan bulanan.

Temuan ini menjadi dasar penggunaan model RegARIMA dengan variabel intervensi pada
tahap pemodelan.

---

## Pemilihan Model

| Model | Hasil pengujian | Keputusan |
|---|---|---|
| SARIMA | ACF lag 12 sebesar −0,143 pada level dan +0,011 setelah differencing | Tidak digunakan — tidak terdapat pola musiman |
| ARIMA | Dapat digunakan, namun tidak menangani patahan struktural | Baseline pembanding |
| **RegARIMA** | Variabel intervensi signifikan dengan R² 0,9993 | **Model utama** |

**Metode pembanding:** Double Exponential Smoothing (Holt's Linear Trend), dipilih
karena berasal dari keluarga metode yang berbeda dan sesuai untuk deret bertren tanpa
komponen musiman.

---

## Isi Repositori

| Berkas | Keterangan |
|---|---|
| `Stasioneritas_Neraca_Beras.ipynb` | Notebook analisis, sudah berisi seluruh keluaran |
| `Data_Neraca_Beras_Bojonegoro_2021_2023.csv` | Data mentah |
| `01_neraca_beras.pdf` | Produksi terhadap kebutuhan konsumsi |
| `02_lima_variabel.pdf` | Perbandingan perilaku kelima variabel |
| `03_patahan_struktural.pdf` | Perubahan antar bulan dan titik patahan |
| `04_rolling_statistics.pdf` | Rata-rata dan simpangan baku bergerak |
| `05_acf_pacf.pdf` | Korelogram data asli |
| `06_setelah_differencing.pdf` | Deret dan ACF setelah differencing |

---

## Cara Menjalankan

1. Unduh `Stasioneritas_Neraca_Beras.ipynb` dan `Data_Neraca_Beras_Bojonegoro_2021_2023.csv`
2. Buka notebook di Google Colab
3. Unggah berkas CSV ke sesi Colab
4. Jalankan seluruh sel melalui menu *Runtime → Run all*

Paket yang dibutuhkan: `pandas`, `numpy`, `matplotlib`, `statsmodels`.

---

## Tahap Selanjutnya

1. Identifikasi ordo p dan q melalui plot ACF dan PACF data hasil differencing
2. Estimasi parameter model RegARIMA dengan variabel intervensi
3. Pemodelan pembanding menggunakan Double Exponential Smoothing
4. Diagnostik residual menggunakan uji Ljung-Box
5. Evaluasi akurasi peramalan menggunakan RMSE, MAE, dan MAPE
