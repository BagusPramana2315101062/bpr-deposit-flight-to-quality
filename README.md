# Analisis Perilaku Simpanan Deposito BPR di Indonesia

[![Open in Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/BagusPramana2315101062/bpr-deposit-flight-to-quality/blob/main/UAS_DS_BPR_Flight_to_Quality.ipynb)

Proyek ini menganalisis perilaku simpanan deposito Bank Perekonomian Rakyat di Indonesia sebagai indikasi awal risiko *flight to quality* menggunakan pendekatan data science.

Analisis dilakukan menggunakan data mingguan periode 2021 sampai 2024 dengan empat tipe deposan, yaitu BPR, HNWI, Corporate, dan Retail.

## Topik Proyek

**Topik:** Stabilitas Sistem Keuangan

**Subtopik:** Risiko *flight to quality* dana pihak ketiga dan dampaknya terhadap stabilitas sistem perbankan.

**Judul lengkap:**

> Analisis Perilaku Simpanan Deposito BPR di Indonesia sebagai Indikasi Risiko Flight to Quality Menggunakan Pendekatan Data Science

## Latar Belakang

Deposito merupakan salah satu sumber pendanaan penting bagi Bank Perekonomian Rakyat. Perubahan nilai deposito, konsentrasi dana pada kelompok deposan tertentu, dan volatilitas simpanan dapat memengaruhi stabilitas pendanaan bank.

Dalam kondisi ketidakpastian, deposan dapat mengubah preferensi dananya menuju institusi atau instrumen yang dianggap lebih aman. Perilaku tersebut berkaitan dengan konsep *flight to quality*.

Proyek ini tidak membuktikan terjadinya *flight to quality* secara kausal. Analisis hanya digunakan untuk membaca indikasi awal risiko berdasarkan pola simpanan, konsentrasi dana, volatilitas, dominasi deposan, dan perubahan deposit dari waktu ke waktu.

## Tujuan Proyek

Proyek ini bertujuan untuk:

1. Menganalisis tren simpanan deposito dari waktu ke waktu.
2. Membandingkan karakteristik deposito berdasarkan tipe deposan.
3. Mengukur volatilitas dan konsentrasi sumber dana.
4. Mengidentifikasi tipe deposan yang paling dominan.
5. Menguji perbedaan distribusi deposito antar tipe deposan.
6. Membandingkan beberapa model prediksi total deposito.
7. Mengidentifikasi fitur historis yang paling berpengaruh terhadap prediksi.
8. Menyusun interpretasi dan rekomendasi pemantauan risiko pendanaan.

## Dataset

Dataset yang digunakan adalah:

**Dataset for deposit rural banks in Indonesia**

Dataset dipublikasikan melalui Mendeley Data dan didukung oleh artikel ilmiah:

> Tedyono, R., Madyan, M., Harymawan, I., & Margono, H. (2025). Dataset for deposit rural banks in Indonesia: A trend analysis from 2021 to 2024. *Data in Brief, 60*, Article 111601.

Sumber dataset:

- [Mendeley Data](https://data.mendeley.com/datasets/ztxbdzhdws)
- DOI dataset: [10.17632/ztxbdzhdws.2](https://doi.org/10.17632/ztxbdzhdws.2)
- DOI artikel: [10.1016/j.dib.2025.111601](https://doi.org/10.1016/j.dib.2025.111601)

Karakteristik dataset:

| Karakteristik | Keterangan |
|---|---|
| Periode | 9 Agustus 2021 sampai 26 Agustus 2024 |
| Frekuensi | Mingguan |
| Jumlah observasi bersih | 608 observasi |
| Tipe deposan | BPR, HNWI, Corporate, dan Retail |
| Sheet utama | `Type depositor` |
| Target utama | `Total_Deposit` |

File Excel memiliki tiga sheet:

1. `Trend deposit`
2. `Type depositor`
3. `Registered User & Deposan`

Analisis utama difokuskan pada sheet `Type depositor` karena memuat periode, tipe deposan, nilai total deposit, dan persentase kontribusi setiap tipe deposan.

Dataset tidak disimpan langsung di repository ini. Pengguna dapat mengunduh dataset dari Mendeley Data melalui tautan sumber resmi di atas.

## Variabel Utama

| Variabel | Keterangan |
|---|---|
| `Period` | Tanggal observasi |
| `Week` | Minggu observasi |
| `Month` | Bulan observasi |
| `Type_Depositor` | Kategori deposan |
| `Total_Deposit` | Nilai total deposito |
| `Percentage` | Persentase kontribusi tipe deposan |
| `deposit_growth` | Pertumbuhan deposito dari periode sebelumnya |
| `log_deposit_growth` | Pertumbuhan deposito berbasis logaritma |
| `lag_deposit_1` | Nilai deposito satu periode sebelumnya |
| `lag_deposit_4` | Nilai deposito empat periode sebelumnya |
| `rolling_mean_4` | Rata-rata deposito dalam empat periode |
| `rolling_std_4` | Volatilitas deposito dalam empat periode |
| `HHI` | Tingkat konsentrasi deposito |

## Tahapan Analisis

Tahapan analisis dalam proyek ini meliputi:

1. Pemahaman masalah.
2. Pengumpulan dan pemahaman data.
3. Pembersihan dan preprocessing data.
4. Audit missing value dan duplikasi.
5. Exploratory Data Analysis.
6. Analisis tren dan distribusi deposito.
7. Feature engineering.
8. Analisis volatilitas deposito.
9. Pengukuran konsentrasi dengan HHI.
10. Analisis deposan dominan.
11. Uji statistik Kruskal-Wallis.
12. Uji post-hoc Mann-Whitney dengan koreksi Bonferroni.
13. Pemodelan prediksi total deposito.
14. Evaluasi dan perbandingan model.
15. Analisis error berdasarkan tipe deposan.
16. Permutation feature importance.
17. Penyusunan interpretasi dan rekomendasi.

## Preprocessing Data

Proses preprocessing yang dilakukan meliputi:

- Menghapus baris judul dan baris kosong dari file Excel.
- Memilih enam kolom utama.
- Mengubah nama kolom agar lebih mudah digunakan.
- Mengubah kolom `Period` menjadi format datetime.
- Mengubah nilai deposito menjadi numerik.
- Membersihkan simbol persen.
- Menghapus baris yang tidak lengkap pada variabel utama.
- Memeriksa missing value.
- Memeriksa duplikasi.
- Mengurutkan data berdasarkan tipe deposan dan periode.

Setelah preprocessing, dataset utama memiliki 608 observasi dan tidak memiliki missing value maupun duplikasi pada variabel utama.

## Feature Engineering

Beberapa fitur baru dibuat untuk mendukung analisis.

### Deposit Growth

`deposit_growth` digunakan untuk mengukur persentase perubahan deposito dari periode sebelumnya.

Fitur ini menghasilkan beberapa nilai ekstrem karena terdapat periode dengan nilai deposit sebelumnya sangat kecil.

### Log Deposit Growth

`log_deposit_growth` dibuat untuk meredam pengaruh pertumbuhan yang sangat ekstrem.

### Lag Features

- `lag_deposit_1` menunjukkan nilai deposito satu periode sebelumnya.
- `lag_deposit_4` menunjukkan nilai deposito empat periode sebelumnya.

### Rolling Features

- `rolling_mean_4` menunjukkan rata-rata deposito dalam empat periode.
- `rolling_std_4` menunjukkan volatilitas deposito dalam empat periode.

### Herfindahl-Hirschman Index

HHI digunakan untuk mengukur tingkat konsentrasi deposito berdasarkan proporsi setiap tipe deposan.

Nilai HHI yang semakin tinggi menunjukkan bahwa deposito semakin terkonsentrasi pada kelompok deposan tertentu.

## Uji Statistik

### Kruskal-Wallis

Uji Kruskal-Wallis digunakan untuk mengetahui apakah terdapat perbedaan distribusi total deposit antara BPR, Corporate, HNWI, dan Retail.

Hipotesis:

- H0: Tidak terdapat perbedaan distribusi total deposit antar tipe deposan.
- H1: Terdapat perbedaan distribusi total deposit antar tipe deposan.

Hasil:

| Statistik | Nilai |
|---|---:|
| Kruskal-Wallis Statistic | 243,0908 |
| P-value | 2,042254 × 10⁻⁵² |

Karena p-value lebih kecil dari 0,05, H0 ditolak. Terdapat perbedaan distribusi total deposit yang signifikan antar tipe deposan.

### Mann-Whitney Post-Hoc

Hasil uji pasangan menunjukkan:

| Pasangan | Hasil |
|---|---|
| BPR dan Corporate | Berbeda signifikan |
| BPR dan HNWI | Berbeda signifikan |
| BPR dan Retail | Berbeda signifikan |
| Corporate dan HNWI | Berbeda signifikan |
| Corporate dan Retail | Tidak berbeda signifikan |
| HNWI dan Retail | Berbeda signifikan |

Corporate dan Retail merupakan pasangan dengan distribusi deposit yang paling mirip.

## Modelling

Tiga model dibandingkan dalam proyek ini:

1. Linear Regression
2. Random Forest Regressor
3. Gradient Boosting Regressor

Fitur yang digunakan:

- `Type_Depositor`
- `Year`
- `Month_Number`
- `lag_deposit_1_billion`
- `lag_deposit_4_billion`
- `rolling_mean_4_prev_billion`
- `rolling_std_4_prev_billion`

Target model:

- `Target_Deposit_Billion`

Data dibagi berdasarkan waktu:

| Data | Periode | Jumlah |
|---|---|---:|
| Training | Sebelum 1 Januari 2024 | 452 observasi |
| Testing | 1 Januari sampai 26 Agustus 2024 | 140 observasi |

Pembagian berdasarkan waktu digunakan untuk menghindari kebocoran informasi dari masa depan ke data training.

## Hasil Evaluasi Model

| Model | MAE | RMSE | MAPE | R² |
|---|---:|---:|---:|---:|
| Linear Regression | 8,5742 | 12,8546 | 39,30% | 0,7919 |
| Random Forest | 9,2058 | 13,0747 | 37,99% | 0,7847 |
| Gradient Boosting | 9,7321 | 14,7773 | 35,57% | 0,7249 |

Linear Regression dipilih sebagai model terbaik karena memiliki MAE dan RMSE terendah serta R² tertinggi.

Nilai R² sebesar 0,7919 menunjukkan bahwa model mampu menjelaskan sekitar 79,19% variasi total deposit pada data testing.

R² tidak diartikan sebagai tingkat akurasi. Model juga masih memiliki MAPE sebesar 39,30%, sehingga hasil prediksi lebih tepat digunakan sebagai alat eksplorasi dan pemantauan awal.

## Hasil Utama

Beberapa temuan utama proyek ini adalah:

- Total deposit memiliki kecenderungan meningkat, tetapi tetap berfluktuasi.
- BPR dan HNWI memiliki nilai deposito yang jauh lebih besar dibanding Corporate dan Retail.
- BPR menjadi tipe deposan dominan pada 82 periode.
- HNWI menjadi tipe deposan dominan pada 68 periode.
- BPR dan HNWI mendominasi 150 dari 159 periode.
- Rata-rata HHI sebesar 0,4475 menunjukkan adanya konsentrasi dana.
- Retail memiliki volatilitas relatif tertinggi berdasarkan koefisien variasi.
- BPR memiliki volatilitas nominal jangka pendek tertinggi berdasarkan rolling standard deviation.
- Terdapat perbedaan signifikan nilai deposito antar tipe deposan.
- Linear Regression menjadi model terbaik dalam eksperimen.
- Model lebih sulit memprediksi nilai deposito HNWI dan BPR.
- Pola historis deposito lebih penting dibandingkan fitur kalender sederhana.

## Ringkasan Nilai Deposito

| Tipe deposan | Rata-rata mingguan | Median | Nilai maksimum |
|---|---:|---:|---:|
| BPR | Rp38,86 miliar | Rp37,75 miliar | Rp139,65 miliar |
| HNWI | Rp32,17 miliar | Rp23,70 miliar | Rp115,83 miliar |
| Corporate | Rp6,57 miliar | Rp5,53 miliar | Rp28,84 miliar |
| Retail | Rp7,26 miliar | Rp3,84 miliar | Rp28,19 miliar |

## Analisis Error Berdasarkan Tipe Deposan

| Tipe deposan | MAE |
|---|---:|
| HNWI | Rp14,17 miliar |
| BPR | Rp12,73 miliar |
| Corporate | Rp4,75 miliar |
| Retail | Rp2,64 miliar |

Model memiliki error terbesar pada HNWI dan BPR. Kedua kelompok tersebut memiliki nilai nominal lebih besar dan pola yang lebih sulit diprediksi.

## Feature Importance

Urutan fitur berdasarkan permutation importance:

1. `lag_deposit_4_billion`
2. `rolling_mean_4_prev_billion`
3. `lag_deposit_1_billion`
4. `rolling_std_4_prev_billion`
5. `Type_Depositor`
6. `Month_Number`
7. `Year`

Hasil tersebut menunjukkan bahwa pola historis deposito lebih berpengaruh terhadap prediksi dibandingkan faktor kalender.

## Interpretasi Risiko Flight to Quality

Proyek ini tidak membuktikan bahwa *flight to quality* telah terjadi.

Dataset tidak memuat data mengenai:

- Perpindahan dana antarbank.
- Alasan deposan menarik atau memindahkan dana.
- Identitas bank individual.
- Kondisi kesehatan setiap bank.
- NPL, CAR, dan LDR.
- Tingkat bunga penjaminan LPS.
- Persepsi deposan terhadap keamanan bank.

Hasil proyek hanya menunjukkan indikasi risiko yang relevan dengan isu *flight to quality*, terutama melalui:

- Konsentrasi dana.
- Dominasi deposan besar.
- Volatilitas deposito.
- Perubahan tajam nilai simpanan.
- Kesulitan model dalam memprediksi deposan besar.

## Rekomendasi

Berdasarkan hasil analisis, beberapa rekomendasi yang dapat dipertimbangkan adalah:

1. Memantau kontribusi BPR dan HNWI secara rutin.
2. Mengembangkan indikator peringatan dini berbasis pertumbuhan deposit, rolling volatility, dan HHI.
3. Mengurangi ketergantungan pada satu atau dua kelompok deposan.
4. Memperluas basis deposan Retail dan Corporate.
5. Menyusun skenario tekanan likuiditas ketika deposan besar menarik dana.
6. Menggunakan model prediksi sebagai alat pendukung, bukan sebagai satu-satunya dasar keputusan.
7. Menggabungkan data deposito dengan NPL, LDR, CAR, suku bunga, dan indikator likuiditas pada penelitian lanjutan.

## Keterbatasan

Keterbatasan proyek ini meliputi:

- Dataset bersifat agregat.
- Tidak terdapat identitas bank individual.
- Tidak terdapat data perpindahan dana antarbank.
- Tidak terdapat alasan deposan menarik dana.
- Tidak terdapat indikator kesehatan bank.
- Tidak dilakukan pengujian pengaruh tingkat bunga deposito.
- Nilai HHI pada periode awal dipengaruhi oleh belum lengkapnya tipe deposan yang tercatat.
- Model hanya diuji pada data Januari sampai Agustus 2024.
- Hasil tidak dapat digeneralisasi sebagai kondisi seluruh BPR di Indonesia.
- Model belum melalui deployment maupun validasi operasional.

## Struktur Repository

```text
bpr-deposit-flight-to-quality/
├── README.md
└── UAS_DS_BPR_Flight_to_Quality.ipynb
```

## Cara Menjalankan Notebook

1. Klik tombol **Open in Google Colab** di bagian atas README.
2. Unduh dataset dari Mendeley Data.
3. Jalankan seluruh cell dari atas ke bawah.
4. Saat diminta mengunggah dataset, pilih file Excel dataset.
5. Pastikan nama file pada notebook sesuai dengan nama file yang diunggah.

Notebook juga dapat dijalankan secara lokal menggunakan Jupyter Notebook atau JupyterLab.

## Library yang Digunakan

- pandas
- NumPy
- Matplotlib
- SciPy
- scikit-learn
- openpyxl

Instalasi library secara lokal:

```bash
pip install pandas numpy matplotlib scipy scikit-learn openpyxl
```

## Tools

- Python
- Google Colab
- Jupyter Notebook
- pandas
- NumPy
- Matplotlib
- SciPy
- scikit-learn
- Microsoft Excel

## Referensi Utama

- Tedyono, R., Madyan, M., Harymawan, I., & Margono, H. (2025). Dataset for deposit rural banks in Indonesia: A trend analysis from 2021 to 2024. *Data in Brief, 60*, Article 111601.
- Tedyono, R. (2025). *Dataset for deposit rural banks in Indonesia* (Version 2) [Data set]. Mendeley Data.
- Basel Committee on Banking Supervision. (2008). *Principles for sound liquidity risk management and supervision*.
- Caballero, R. J., & Krishnamurthy, A. (2008). Collective risk management in a flight to quality episode. *The Journal of Finance, 63*(5), 2195–2230.
- Hyndman, R. J., & Athanasopoulos, G. (2021). *Forecasting: Principles and practice*.
- Breiman, L. (2001). Random forests. *Machine Learning, 45*(1), 5–32.
- Friedman, J. H. (2001). Greedy function approximation: A gradient boosting machine. *The Annals of Statistics, 29*(5), 1189–1232.

## Author

GitHub: [BagusPramana2315101062](https://github.com/BagusPramana2315101062)

Mata Kuliah: Data Science

Tahun: 2026

## Disclaimer

Repository ini dibuat untuk keperluan akademik.

Seluruh hasil analisis harus dibaca sesuai dengan keterbatasan dataset. Repository ini tidak memberikan rekomendasi investasi, penilaian kesehatan bank, maupun kesimpulan resmi mengenai stabilitas industri perbankan Indonesia.
