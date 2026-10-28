# Banking Domain Data Analysis

![Summary Dashboard](All_Dashboard/Summary.png)

![Summary Dashboard](All_Dashboard/Loan_Analysis.png)

![Summary Dashboard](All_Dashboard/Deposit_Analysis.png)

![Summary Dashboard](All_Dashboard/Home.png)

![Summary Dashboard](All_Dashboard/Drill_Through.png)

# Banking Dashboard Analysis — Power BI

## Project Overview

Proyek ini bertujuan untuk menganalisis data perbankan menggunakan **Microsoft Power BI** guna memahami karakteristik nasabah, kinerja portofolio pinjaman, simpanan, biaya perbankan, serta tingkat keterlibatan nasabah.

Melalui proses **Data Cleaning, Data Transformation, Data Modeling, DAX Measures**, dan **Data Visualization**, proyek ini menghasilkan dashboard interaktif yang mendukung **Business Intelligence**, **Exploratory Data Analysis (EDA)**, dan pengambilan keputusan berbasis data.

Dashboard memungkinkan pengguna mengeksplorasi indikator perbankan dari tingkat ringkasan hingga tingkat nasabah berdasarkan dimensi yang tersedia dalam data.

## Business Problem

Institusi perbankan perlu memahami hubungan antara karakteristik nasabah, aktivitas perbankan, portofolio pinjaman, simpanan, dan biaya layanan. Tanpa penyajian data yang terstruktur, informasi tersebut dapat sulit dibandingkan dan dievaluasi secara konsisten.

Proyek ini menggunakan Power BI untuk mengintegrasikan indikator utama ke dalam laporan interaktif. Dengan pendekatan tersebut, pengguna dapat meninjau kinerja perbankan, membandingkan segmen nasabah, serta menentukan area yang membutuhkan analisis lebih lanjut.

## Project Objectives

Tujuan utama proyek ini adalah:

1. Menyiapkan data perbankan agar dapat digunakan untuk analisis.
2. Mengembangkan Data Model dan perhitungan menggunakan DAX.
3. Menyajikan Key Performance Indicators (KPIs) melalui dashboard interaktif.
4. Mengeksplorasi distribusi portofolio pinjaman, simpanan, dan biaya perbankan.
5. Membandingkan metrik berdasarkan Income Band, Nationality, Banking Relationship, dan Engagement Timeframe.
6. Mendukung analisis lebih terperinci melalui fitur Drill Through.
7. Menyusun interpretasi dan rekomendasi bisnis berdasarkan hasil analisis yang telah diverifikasi.

## Tools & Technologies

- **Microsoft Power BI** — Dashboard Development dan Interactive Reporting.
- **Power Query** — Data Cleaning dan Data Transformation.
- **DAX (Data Analysis Expressions)** — Calculated Columns, Measures, dan Business Calculations.
- **Data Modeling** — Pengelolaan hubungan antartabel.
- **Exploratory Data Analysis (EDA)** — Eksplorasi distribusi dan pola data.
- **Data Visualization** — Penyajian metrik melalui visual interaktif.
- **Business Intelligence (BI)** — Penyampaian informasi analitis untuk mendukung evaluasi bisnis.

## Dataset Overview

Dataset yang digunakan mencakup informasi nasabah, hubungan perbankan, dan metrik keuangan. Dokumentasi proyek mengidentifikasi tabel-tabel berikut:

- **Banking Relationship** — Kategori hubungan nasabah dengan layanan perbankan.
- **Client-Banking** — Data nasabah dan metrik keuangan yang digunakan dalam analisis.
- **Gender** — Kategori gender nasabah.
- **Investment Advisor** — Informasi terkait Investment Advisor.
- **Period** — Informasi periode yang tersedia dalam Data Model.

Tabel-tabel tersebut digunakan untuk mendukung analisis berdasarkan karakteristik nasabah, kategori hubungan perbankan, dan indikator keuangan.

*Catatan: Sumber yang tersedia belum mencantumkan URL dataset publik, lisensi data, dan data dictionary lengkap.*

## Data Cleaning & Transformation

Tahap Data Preparation mencakup peninjauan struktur data dan penyiapan atribut yang dibutuhkan untuk analisis.

### 1. Engagement Timeframe

`Engagement Timeframe` digunakan untuk mengelompokkan nasabah berdasarkan kategori durasi keterlibatan dengan bank. Kolom ini mendukung perbandingan metrik keuangan antarkategori.

### 2. Engagement Days

`Engagement Days` menghitung jumlah hari sejak nasabah bergabung dengan bank hingga tanggal saat analisis dilakukan.

```DAX
Engagement Days =
DATEDIFF(
    'Clients - Banking'[Joined Bank],
    TODAY(),
    DAY
)
```

Nama kolom pada implementasi aktual perlu disesuaikan dengan nama yang digunakan di Power BI.

### 3. Income Band

`Income Band` mengelompokkan nasabah berdasarkan pendapatan sehingga metrik keuangan dapat dibandingkan antarkategori.

Dokumentasi proyek menyebutkan batas pendapatan di bawah 100.000 untuk kategori Low dan di bawah 300.000 untuk kategori Mid. Batas lengkap, label kategori, dan penanganan nilai lainnya perlu dikonfirmasi terhadap implementasi aktual.

### 4. Processing Fees

`Processing Fees` digunakan sebagai tarif dalam perhitungan biaya pemrosesan. Dokumentasi menyebutkan tarif `0.05` untuk kategori `High Processing Fees`.

Aturan untuk kategori lainnya perlu diverifikasi agar perhitungan `Total Fees` merepresentasikan definisi bisnis yang tepat.

## DAX Measures & Calculations

DAX digunakan untuk menghasilkan metrik agregat dan KPI yang mendukung analisis perbankan.

| Metric | Description |
|---|---|
| `Total Clients` | Jumlah nasabah unik. |
| `Bank Loan` | Nilai pinjaman perbankan. |
| `Business Lending` | Nilai pinjaman untuk kebutuhan bisnis. |
| `Credit Cards Balance` | Nilai saldo kartu kredit berdasarkan definisi model. |
| `Total Loan` | Gabungan Bank Loan, Business Lending, dan Credit Cards Balance. |
| `Bank Deposit` | Nilai simpanan perbankan. |
| `Savings Account` | Nilai simpanan pada rekening tabungan. |
| `Checking Accounts` | Nilai pada rekening transaksi. |
| `Foreign Currency Account` | Nilai pada rekening mata uang asing. |
| `Total Deposit` | Gabungan kategori simpanan sesuai perhitungan proyek. |
| `Total Fees` | Biaya yang dihitung menggunakan Total Loan dan Processing Fees. |
| `Total CC Amount` | Metrik jumlah kartu kredit sesuai definisi dashboard. |
| `Engagement Length` | Metrik durasi keterlibatan nasabah berdasarkan kolom engagement yang digunakan. |

### Representative DAX Measures

**Total Clients**

```DAX
Total Clients =
DISTINCTCOUNT('Clients - Banking'[Client ID])
```

**Bank Loan**

```DAX
Bank Loan =
SUM('Clients - Banking'[Bank Loans])
```

**Business Lending**

```DAX
Business Lending =
SUM('Clients - Banking'[Business Lending])
```

**Total Loan**

```DAX
Total Loan =
[Bank Loan] + [Business Lending] + [Credit Cards Balance]
```

**Bank Deposit**

```DAX
Bank Deposit =
SUM('Clients - Banking'[Bank Deposits])
```

**Total Deposit**

```DAX
Total Deposit =
[Bank Deposit]
    + [Savings Account]
    + [Foreign Currency Account]
    + [Checking Accounts]
```

**Total Fees**

```DAX
Total Fees =
SUMX(
    'Clients - Banking',
    [Total Loan] * 'Clients - Banking'[Processing Fees]
)
```

**Savings Account**

```DAX
Savings Account =
SUM('Clients - Banking'[Saving Accounts])
```

**Checking Accounts**

```DAX
Checking Accounts =
SUM('Clients - Banking'[Checking Accounts])
```

**Foreign Currency Account**

```DAX
Foreign Currency Account =
SUM('Clients - Banking'[Foreign Currency Account])
```

**Credit Cards Balance**

```DAX
Credit Cards Balance =
SUM('Clients - Banking'[Credit Card Balance])
```

**Total CC Amount**

```DAX
Total CC Amount =
SUM('Clients - Banking'[Amount of Credit Cards])
```

**Engagement Length**

```DAX
Engagment Length =
SUM('Clients - Banking'[Engagment Days])
```

Fungsi DAX yang digunakan atau relevan dengan dokumentasi perhitungan proyek meliputi `SUM()`, `DISTINCTCOUNT()`, `SUMX()`, `SWITCH()`, dan `DATEDIFF()`.

*Catatan validasi: Nama kolom terkait kartu kredit dan engagement berbeda pada beberapa bagian dokumentasi. Pastikan nama field, definisi metrik, serta formula DAX sesuai dengan implementasi aktual di file PBIX sebelum memublikasikan hasil.*

## Dashboard Overview

Dashboard terdiri dari lima halaman yang mendukung eksplorasi data dari ringkasan hingga detail nasabah.

### 1. Home

Menampilkan gambaran umum KPI dan indikator perbankan sebagai titik awal untuk memahami data dan menavigasi halaman analisis lainnya.

### 2. Loan Analysis

Berfokus pada portofolio pinjaman melalui metrik Bank Loan, Business Lending, dan Credit Cards Balance. Halaman ini juga mendukung eksplorasi berdasarkan dimensi seperti Banking Relationship, Nationality, Income Band, dan Engagement Timeframe sesuai visual yang tersedia.

### 3. Deposit Analysis

Menyajikan metrik simpanan yang mencakup Bank Deposit, Savings Account, Checking Accounts, dan Foreign Currency Account. Pengguna dapat mengeksplorasi distribusi simpanan berdasarkan kategori nasabah yang tersedia.

### 4. Summary

Menyediakan tampilan ringkasan indikator perbankan untuk membantu pengguna meninjau metrik utama secara menyeluruh.

### 5. Drill Through

Mendukung penelusuran lebih terperinci berdasarkan konteks atau kategori yang dipilih dari halaman sebelumnya. Detail yang dapat ditinjau bergantung pada field dan konfigurasi Drill Through dalam model Power BI.

## Key Business Questions

Pertanyaan bisnis berikut mengarahkan analisis yang dilakukan melalui dashboard.

### 1. Kinerja Nasabah dan Perbankan

Bagaimana gambaran kinerja perbankan secara keseluruhan berdasarkan jumlah nasabah, total pinjaman, total simpanan, total biaya, dan nilai kartu kredit yang direpresentasikan oleh metrik Total CC Amount?

### 2. Analisis Portofolio Pinjaman

Bagaimana distribusi portofolio pinjaman berdasarkan Bank Loan, Business Lending, dan Credit Cards Balance, serta kategori Banking Relationship mana yang memberikan kontribusi terbesar terhadap penyaluran pinjaman?

### 3. Segmentasi Nasabah Pinjaman

Bagaimana nilai Bank Loan bervariasi berdasarkan Nationality dan Income Band, serta segmen nasabah mana yang perlu dianalisis lebih lanjut?

### 4. Analisis Portofolio Simpanan

Bagaimana distribusi simpanan nasabah berdasarkan Bank Deposit, Savings Account, Checking Accounts, dan Foreign Currency Account?

### 5. Segmentasi Nasabah Simpanan

Bagaimana nilai Bank Deposit bervariasi berdasarkan Income Band dan Nationality, serta segmen mana yang memberikan kontribusi terbesar terhadap portofolio simpanan?

### 6. Analisis Keterlibatan Nasabah

Bagaimana jumlah pinjaman dan simpanan bervariasi berdasarkan kategori Engagement Timeframe, dan pola apa yang dapat diamati dari keterlibatan nasabah?

### 7. Pemantauan KPI Perbankan

Apa yang dapat diketahui dari gabungan KPI perbankan mengenai jumlah nasabah, pinjaman, simpanan, biaya, dan metrik keterlibatan nasabah?

### 8. Analisis Kinerja pada Tingkat Nasabah

Bagaimana metrik keuangan dan Engagement Length bervariasi antarindividu nasabah dan Investment Advisor, serta data nasabah mana yang perlu ditinjau lebih lanjut?

### 9. Analisis Biaya dan Loyalitas

Bagaimana distribusi Total Fees berdasarkan kategori Loyalty Classification, dan perbedaan apa yang perlu dianalisis lebih lanjut?

### 10. Analisis Nationality dan Keterlibatan Nasabah

Bagaimana Total Fees dan Engagement Length bervariasi berdasarkan Nationality, serta pola apa yang dapat diidentifikasi pada berbagai segmen nasabah untuk dianalisis lebih lanjut?

## Key Insights & Business Implications

Bagian ini menjelaskan pola yang perlu diperiksa pada dashboard dan bagaimana hasilnya dapat diterjemahkan menjadi pertimbangan bisnis. Temuan spesifik harus ditetapkan berdasarkan nilai aktual, bukan diasumsikan sebelum visualisasi diverifikasi.

| Area Analisis | Insight yang Perlu Dievaluasi | Business Implication |
|---|---|---|
| Overall Banking Performance | Tinjau nilai KPI untuk memahami skala nasabah, pinjaman, simpanan, biaya, dan metrik kartu kredit. | Menentukan indikator utama yang perlu dipantau secara berkala. |
| Loan Portfolio | Identifikasi komponen pinjaman dan Banking Relationship dengan kontribusi terbesar berdasarkan nilai aktual. | Membantu menentukan area portofolio yang memerlukan evaluasi lebih lanjut. |
| Loan Segmentation | Bandingkan Bank Loan berdasarkan Income Band dan Nationality, dengan mempertimbangkan jumlah nasabah tiap segmen. | Mendukung evaluasi karakteristik segmen dan kesesuaian layanan. |
| Deposit Portfolio | Evaluasi kontribusi kategori rekening terhadap Total Deposit. | Membantu memahami komposisi simpanan dan menentukan area yang layak ditinjau lebih lanjut. |
| Deposit Segmentation | Bandingkan Bank Deposit berdasarkan Income Band dan Nationality. | Mendukung analisis distribusi simpanan antarsegmen nasabah. |
| Customer Engagement | Periksa variasi metrik pinjaman dan simpanan berdasarkan Engagement Timeframe serta Engagement Length. | Membantu mengeksplorasi pola keterlibatan nasabah tanpa menyimpulkan hubungan sebab-akibat. |
| KPI Monitoring | Evaluasi perubahan KPI berdasarkan filter dan dimensi yang tersedia. | Mendukung pemantauan kinerja berdasarkan konteks analisis yang berbeda. |
| Customer-Level Analysis | Telusuri detail nasabah dan Investment Advisor melalui Drill Through jika informasi tersebut tersedia. | Membantu memeriksa konteks di balik hasil agregasi dan mengidentifikasi data yang memerlukan peninjauan. |
| Fees and Loyalty | Tinjau distribusi Total Fees berdasarkan Loyalty Classification. | Mendukung evaluasi pola biaya antarkategori sebelum merumuskan tindakan bisnis. |
| Nationality and Engagement | Bandingkan Total Fees dan Engagement Length berdasarkan Nationality. | Membantu mengidentifikasi perbedaan yang relevan untuk analisis lanjutan. |

**Catatan:** Tabel ini merupakan kerangka interpretasi dan implikasi bisnis. Kategori tertinggi, tren dominan, atau hubungan antarmetrik belum dinyatakan sebagai temuan final tanpa verifikasi hasil aktual dashboard. Perbedaan nilai agregat juga tidak otomatis berarti perbedaan rata-rata per nasabah.

## Business Value

Banking Dashboard Analysis mengubah data perbankan menjadi informasi terstruktur melalui KPI, Data Visualization, dan Interactive Reporting. Nilai bisnisnya terletak pada kemampuan dashboard untuk menyediakan dasar analisis yang konsisten, membantu pengguna meninjau kondisi perbankan, dan mengarahkan evaluasi lebih lanjut.

Nilai bisnis yang didukung oleh proyek ini meliputi:

- **Performance Monitoring** — Menyediakan gambaran indikator utama untuk membantu pemantauan kinerja perbankan.
- **Portfolio Analysis** — Mendukung evaluasi komposisi portofolio pinjaman dan simpanan sebagai dasar peninjauan alokasi dan karakteristik portofolio.
- **Customer Segmentation** — Memungkinkan perbandingan metrik keuangan antarkategori nasabah untuk menemukan perbedaan yang relevan bagi analisis bisnis.
- **Customer Engagement Analysis** — Membantu mengeksplorasi pola keterlibatan nasabah dan variasinya terhadap metrik keuangan.
- **Financial Performance Analysis** — Mendukung peninjauan Total Fees dan indikator keuangan lain untuk menentukan area yang membutuhkan pemeriksaan lebih lanjut.
- **Customer-Level Analysis** — Memungkinkan penelusuran detail melalui Drill Through untuk memvalidasi konteks di balik hasil agregasi.
- **Data-Driven Decision Making** — Menyediakan landasan analitis bagi penyusunan rekomendasi yang didasarkan pada temuan terverifikasi dan KPI yang dapat dipantau.

Nilai tersebut merupakan manfaat yang dapat didukung oleh kemampuan dashboard. Dampak bisnis aktual, seperti peningkatan pendapatan, retensi nasabah, atau penurunan risiko, memerlukan analisis lanjutan dan evaluasi hasil implementasi.

## Project Scope & Limitations

Proyek ini berfokus pada **descriptive analytics** dan eksplorasi data menggunakan Microsoft Power BI. Cakupannya meliputi Data Preparation, Data Modeling, DAX Calculations, KPI Reporting, dan Data Visualization.

Batasan yang perlu diperhatikan:

- Hasil analisis bergantung pada kualitas, cakupan, dan definisi data.
- Interpretasi KPI harus mengikuti formula yang diterapkan pada model.
- Perbedaan nilai agregat tidak selalu menunjukkan perbedaan rata-rata pada tingkat nasabah.
- Pola pada visualisasi tidak secara otomatis membuktikan hubungan sebab-akibat.
- Klaim mengenai segmen tertinggi atau terendah perlu diperiksa pada dashboard aktual.
- Proyek ini tidak menunjukkan implementasi model Machine Learning atau sistem prediksi persetujuan pinjaman.
- Analisis risiko kredit memerlukan indikator, metodologi, dan validasi tambahan yang tidak dapat diasumsikan tersedia dalam dashboard saat ini.

## Future Improvements

Pengembangan lebih lanjut yang dapat dipertimbangkan:

1. **Data Dictionary** — Mendokumentasikan definisi tabel, kolom, relasi, dan metrik.
2. **Data Quality Validation** — Memeriksa missing values, duplikasi Client ID, konsistensi kategori, dan rentang tanggal.
3. **DAX Optimization** — Meninjau efisiensi formula dan konsistensi definisi KPI.
4. **Trend Analysis** — Mengevaluasi perubahan indikator dari waktu ke waktu jika data historis tersedia.
5. **Customer Retention Analysis** — Mengembangkan analisis retensi apabila data yang diperlukan tersedia.
6. **Advanced Risk Analysis** — Menambahkan indikator risiko kredit jika field dan metodologi yang sesuai tersedia.
7. **Predictive Analytics** — Mengembangkan model prediktif sebagai proyek lanjutan dengan data, evaluasi, dan validasi yang memadai.
8. **Automated Data Refresh** — Menyiapkan pembaruan data terjadwal apabila sumber data dan infrastruktur mendukung.
9. **Actionable Recommendations** — Menghubungkan temuan terverifikasi dengan tindakan bisnis dan KPI untuk mengevaluasi hasilnya.

## Project Structure

Contoh struktur repository berikut dapat disesuaikan dengan file yang benar-benar disertakan:

```text
Banking-Dashboard/
│
├── README.md
├── Banking Dashboard.pbix
├── Banking Report.docx
└── ppt banking domain.pdf
```

Nama file pada contoh di atas merupakan placeholder dan perlu disesuaikan dengan nama file aktual.

## Conclusion

Banking Dashboard Analysis menunjukkan penerapan Microsoft Power BI untuk mengolah, memodelkan, dan memvisualisasikan data perbankan melalui DAX Measures, KPI Reporting, dan Interactive Dashboard.

Lima halaman—Home, Loan Analysis, Deposit Analysis, Summary, dan Drill Through—mendukung eksplorasi dari gambaran umum kinerja hingga penelusuran data nasabah. Melalui Key Business Questions dan evaluasi temuan, proyek ini menyediakan dasar untuk memahami portofolio, membandingkan segmen nasabah, serta menyusun rekomendasi berbasis data yang telah diverifikasi.

## Keywords

`Data Analyst` · `Microsoft Power BI` · `Power Query` · `DAX` · `Data Cleaning` · `Data Transformation` · `Data Modeling` · `Data Visualization` · `Business Intelligence` · `Exploratory Data Analysis` · `KPI` · `Customer Segmentation` · `Portfolio Analysis` · `Banking Analytics` · `Business Insights` · `Business Implications` · `Data-Driven Decision Making`
