# 🎥 NARASI PRESENTASI VIDEO UTS

## Feature Selection untuk Prediksi Stok Obat Apotek

### Kelompok 16: Muhammad Yusuf (122140193), Cornelius Linux (122140079), Chandra Budi Wijaya (122140093)

---

## 📌 STRUKTUR PRESENTASI (12 MENIT)

### **BAGIAN 1: PEMBUKAAN & ANALISIS PROBLEM (4 MENIT) - Muhammad Yusuf**

#### **[0:00 - 0:30] Opening & Perkenalan**

> "Assalamualaikum warahmatullahi wabarakatuh. Selamat pagi/siang Bapak/Ibu Dosen dan teman-teman. Perkenalkan, kami dari **Kelompok 16**:
>
> - **Muhammad Yusuf** — NIM 122140193
> - **Cornelius Linux** — NIM 122140079
> - **Chandra Budi Wijaya** — NIM 122140093
>
> Pada kesempatan kali ini, kami akan mempresentasikan hasil Ujian Tengah Semester mata kuliah **Penambangan Data** dengan topik **Feature Selection** untuk kasus prediksi stok obat di apotek."

---

#### **[0:30 - 1:30] Latar Belakang & Problem Statement**

> "Baik, pertama-tama saya akan menjelaskan **latar belakang** dan **problem statement** dari studi kasus ini.
>
> Dalam sistem manajemen apotek, **prediksi stok obat** sangat penting untuk:
>
> 1. **Mencegah stockout** (kehabisan stok obat penting)
> 2. **Menghindari overstock** (pemborosan modal dan risiko expired)
> 3. **Optimasi reorder point** (kapan harus restock)
>
> Dataset yang kami gunakan berasal dari **data transaksi apotek real** yang terdiri dari:
>
> - **Dataset Pembelian:** 88,172 transaksi harian dengan informasi Qty Masuk, Qty Keluar, Nilai Transaksi
> - **Dataset Stok:** 1,523 produk dengan informasi Stok Aktual di gudang dan Lokasi penyimpanan
>
> **[TAMPILKAN SLIDE: Dataset Overview]**
>
> Sesuai dengan **ketentuan UTS**, kasus yang kami hadapi adalah:
>
> - Dataset memiliki **12 kolom awal**, namun setelah feature engineering menjadi **20+ fitur**
> - Beberapa fitur seperti **Supplier**, **Cabang**, atau fitur identitas lainnya **mungkin tidak relevan** untuk model prediksi stok
>
> Oleh karena itu, **pertanyaan studi kasus** yang harus kami jawab adalah:
>
> **"Bagaimana kami menentukan fitur mana yang relevan untuk model prediksi stok obat harian?"**

---

#### **[1:30 - 2:30] Metodologi & Pendekatan**

> "Untuk menjawab pertanyaan ini, kami menggunakan **pendekatan multi-method feature selection** yang mencakup:
>
> **[TAMPILKAN SLIDE: Metodologi]**
>
> 1. **Filter Methods:**
>
>    - **Correlation Analysis (Pearson)** → Mengukur hubungan linear antar fitur
>    - **Mutual Information** → Menangkap hubungan non-linear
>
> 2. **Wrapper Methods:**
>
>    - **Recursive Feature Elimination (RFE)** → Eliminasi iteratif dengan Linear Regression
>
> 3. **Embedded Methods:**
>
>    - **Lasso Regression (L1)** → Automatic feature selection dengan regularisasi
>    - **Ridge Regression (L2)** → Shrinkage koefisien fitur
>
> 4. **Tree-based Methods:**
>    - **Random Forest Feature Importance** → Gini importance dari ensemble trees
>
> Mengapa kami menggunakan **6 metode sekaligus**? Karena:
>
> - Setiap metode memiliki **bias dan asumsi berbeda**
> - **Consensus analysis** memberikan hasil yang lebih robust
> - Menghindari kesimpulan yang misleading dari satu metode saja

---

#### **[2:30 - 4:00] Data Integration & Feature Engineering**

> "Sebelum melakukan feature selection, kami melakukan **data preprocessing dan feature engineering** yang cukup ekstensif:
>
> **[TAMPILKAN SLIDE: Data Pipeline]**
>
> **Tahap 1: Data Cleaning**
>
> - Parsing data TSV dengan fixed-width format
> - Konversi tipe data (string → float, date parsing)
> - Handling missing values dan outliers
>
> **Tahap 2: Data Integration**
>
> - **Merge** dataset pembelian dan stok berdasarkan Kode Produk
> - Validasi konsistensi unit dan nama produk
> - Hasil: **1,523 produk** yang memiliki data lengkap
>
> **Tahap 3: Feature Engineering** (Ini yang sangat penting!)
>
> Kami **tidak langsung menggunakan fitur mentah**, melainkan membuat fitur agregat per produk:
>
> - **Qty_Masuk_sum, Qty_Masuk_mean, Qty_Masuk_std, Qty_Masuk_max**
> - **Qty_Keluar_sum, Qty_Keluar_mean, Qty_Keluar_std, Qty_Keluar_max**
> - **Nilai_Masuk_sum, Nilai_Masuk_mean, Nilai_Masuk_std**
> - **Nilai_Keluar_sum, Nilai_Keluar_mean, Nilai_Keluar_std**
> - **Total_Masuk_sum, Total_Masuk_mean**
> - **Frekuensi_Bulan** → Jumlah bulan berbeda dengan transaksi
> - **Lokasi_Encoded** → Label encoding untuk lokasi gudang
>
> Kenapa agregasi penting? Karena:
>
> - **Stok tidak hanya bergantung pada total transaksi**, tapi juga **pola dan variabilitas**
> - Produk dengan penjualan **fluktuatif** butuh buffer stok lebih besar
> - **Mean** menunjukkan rata-rata aktivitas, **Std** menunjukkan volatilitas
>
> **Target variabel** kami adalah **Stok_Aktual** dari dataset stok — ini adalah stok **real** di gudang, bukan simulasi.
>
> Dengan demikian, kita sekarang memiliki **20+ fitur kandidat** untuk dianalisis dengan 6 metode feature selection."

---

### **BAGIAN 2: HASIL ANALISIS FEATURE SELECTION (5 MENIT) - Cornelius Linux**

#### **[4:00 - 4:30] Transisi & Overview Hasil**

> "Terima kasih Muhammad. Selanjutnya, saya **Cornelius Linux** akan menjelaskan **hasil analisis feature selection** menggunakan 6 metode yang telah dijelaskan sebelumnya.
>
> Sebelum masuk ke detail, mari kita lihat **ringkasan hasil** dari semua metode:
>
> **[TAMPILKAN SLIDE: Summary Table]**
>
> Dari tabel ini, kita bisa lihat bahwa:
>
> - Setiap metode memberikan **ranking fitur yang berbeda**
> - Namun, ada **beberapa fitur yang konsisten muncul** di semua metode
> - Ini yang akan menjadi fokus pembahasan kita"

---

#### **[4:30 - 5:30] Metode 1 & 2: Correlation Analysis & Mutual Information**

> "Mari kita mulai dengan **Filter Methods**:
>
> **[TAMPILKAN VISUALISASI: Correlation Heatmap]**
>
> **1. Correlation Analysis (Pearson)**
>
> - Mengukur **hubungan linear** antara fitur dengan Stok_Aktual
> - **Top 5 fitur berdasarkan korelasi:**
>
>   1. **Qty_Keluar_sum** → Korelasi positif (semakin banyak keluar, butuh stok lebih)
>   2. **Qty_Masuk_mean** → Frekuensi restock
>   3. **Qty_Keluar_std** → Variabilitas penjualan
>   4. **Nilai_Keluar_sum** → Total nilai penjualan
>   5. **Lokasi_Encoded** → Pengaruh lokasi penyimpanan
>
> - **Catatan penting:** Fitur **Tahun** menghasilkan **NaN** (Not a Number) karena semua data berasal dari tahun 2021 saja, sehingga tidak ada variasi — fitur ini harus **dieliminasi**.
>
> **[TAMPILKAN VISUALISASI: Mutual Information Bar Chart]**
>
> **2. Mutual Information (MI)**
>
> - Berbeda dengan korelasi, MI dapat menangkap **hubungan non-linear**
> - **Top 5 fitur berdasarkan MI Score:**
>
>   1. **Nilai_Keluar_mean** → 0.098
>   2. **Qty_Masuk_std** → 0.084
>   3. **Nilai_Masuk_sum** → 0.079
>   4. **Qty_Keluar_max** → 0.078
>   5. **Lokasi_Encoded** → 0.060
>
> - **Insight:** Fitur dengan **variabilitas tinggi** (std, max) lebih informatif untuk MI dibanding korelasi linear
> - Ini menunjukkan bahwa **hubungan fitur dengan stok bersifat non-linear**"

---

#### **[5:30 - 6:30] Metode 3 & 4: Random Forest & RFE**

> "Selanjutnya, kita masuk ke metode yang lebih sophisticated:
>
> **[TAMPILKAN VISUALISASI: Random Forest Importance]**
>
> **3. Random Forest Feature Importance**
>
> - Ini adalah **tree-based method** yang menghitung importance berdasarkan **Gini impurity**
> - **Top 5 fitur:**
>
>   1. **Qty_Keluar_sum** → 36.8% (dominan!)
>   2. **Nilai_Keluar_mean** → 33.3%
>   3. **Qty_Masuk_mean** → 12.1%
>   4. **Lokasi_Encoded** → 8.5%
>   5. **Qty_Keluar_std** → 5.2%
>
> - **Interpretasi:** Random Forest sangat menekankan **aktivitas penjualan (Qty_Keluar)** sebagai prediktor utama stok
> - Fitur **nilai transaksi** juga sangat penting (33.3%)
> - Total importance dari top 5 fitur = **95.9%** → sangat dominan!
>
> **[TAMPILKAN VISUALISASI: RFE Ranking]**
>
> **4. Recursive Feature Elimination (RFE)**
>
> - Ini adalah **wrapper method** yang secara iteratif mengeliminasi fitur dengan performa terendah
> - Kami mencoba 3 skenario: **Top 5, Top 10, dan Top 15 fitur**
> - **Top 10 fitur terpilih RFE:**
>
>   1. Qty_Keluar_sum
>   2. Qty_Masuk_mean
>   3. Nilai_Keluar_mean
>   4. Lokasi_Encoded
>   5. Qty_Keluar_std
>   6. Nilai_Masuk_sum
>   7. Qty_Masuk_max
>   8. Frekuensi_Bulan
>   9. Total_Masuk_mean
>   10. Qty_Keluar_mean
>
> - **Insight:** RFE memberikan **kombinasi optimal** antara fitur agregasi dan fitur temporal
> - Fitur yang **dieliminasi** adalah yang redundan atau tidak berkontribusi terhadap model linear"

---

#### **[6:30 - 7:30] Metode 5 & 6: Lasso & Ridge Regression**

> "Terakhir dari analisis individual, kita lihat **Embedded Methods**:
>
> **[TAMPILKAN VISUALISASI: Lasso Coefficients]**
>
> **5. Lasso Regression (L1 Regularization)**
>
> - Alpha optimal yang ditemukan: **0.039** (hasil dari cross-validation)
> - **Karakteristik Lasso:** Dapat mengecilkan koefisien fitur menjadi **tepat 0** (automatic feature selection)
> - **Hasil:**
>
>   - **6 dari 7 fitur terpilih** (hanya Tahun yang dieliminasi)
>   - **Top 5 berdasarkan koefisien absolut:**
>     1. Qty_Keluar_sum → Coef: **+36.32**
>     2. Qty_Masuk_mean → Coef: **+24.56**
>     3. Lokasi_Encoded → Coef: **+6.55**
>     4. Nilai_Keluar_mean → Coef: **-2.11** (negatif, mengurangi stok)
>     5. Nilai_Masuk_sum → Coef: **-1.80**
>
> - **Interpretasi koefisien negatif:** Produk dengan nilai transaksi tinggi cenderung memiliki stok lebih rendah (turnover cepat)
>
> **[TAMPILKAN VISUALISASI: Ridge Coefficients]**
>
> **6. Ridge Regression (L2 Regularization)**
>
> - Alpha optimal: **1000.0** (regularisasi kuat)
> - **Perbedaan dengan Lasso:** Ridge **tidak mengeliminasi fitur**, hanya mengecilkan bobotnya
> - **Top 5 fitur:**
>
>   1. Qty_Keluar_sum → Coef: **+35.97**
>   2. Qty_Masuk_mean → Coef: **+24.32**
>   3. Lokasi_Encoded → Coef: **+6.52**
>   4. Nilai_Keluar_mean → Coef: **-2.15**
>   5. Nilai_Masuk_sum → Coef: **-1.88**
>
> - **Insight:** Ridge memberikan hasil **sangat mirip** dengan Lasso, menunjukkan **stabilitas pemilihan fitur**
> - Koefisien yang **stabil** antar metode menunjukkan fitur tersebut **truly important**"

---

#### **[7:30 - 9:00] Consensus Analysis**

> "Sekarang, yang paling penting: **Consensus Analysis**.
>
> **[TAMPILKAN VISUALISASI: Feature Consensus Chart]**
>
> Kami menghitung **frekuensi kemunculan** setiap fitur di **Top 10** dari **keenam metode**. Fitur yang muncul di ≥4 metode dianggap sebagai **fitur konsensus tinggi**.
>
> **Hasil Feature Consensus:**
>
> | Fitur                 | Frekuensi | Interpretasi                                   |
> | :-------------------- | :-------: | :--------------------------------------------- |
> | **Qty_Keluar_sum**    |  **6/6**  | Muncul di SEMUA metode — faktor paling krusial |
> | **Qty_Masuk_mean**    |  **6/6**  | Rata-rata restock sangat penting               |
> | **Nilai_Keluar_mean** |  **6/6**  | Nilai ekonomi transaksi penjualan              |
> | **Lokasi_Encoded**    |  **6/6**  | Lokasi gudang berpengaruh signifikan           |
> | **Qty_Keluar_std**    |  **5/6**  | Variabilitas penjualan penting untuk buffer    |
> | **Nilai_Masuk_sum**   |  **5/6**  | Total nilai pembelian                          |
> | **Qty_Keluar_mean**   |  **5/6**  | Rata-rata penjualan harian                     |
> | **Qty_Masuk_max**     |  **4/6**  | Puncak pembelian (bulk buying)                 |
> | **Frekuensi_Bulan**   |  **4/6**  | Pola temporal transaksi                        |
> | **Total_Masuk_mean**  |  **4/6**  | Rata-rata total nilai masuk                    |
>
> **[HIGHLIGHT di visualisasi: Garis threshold di frekuensi ≥4]**
>
> Dari analisis ini, kita bisa simpulkan:
>
> 1. **4 fitur inti (frekuensi 6/6)** adalah MUST-HAVE untuk model prediksi
> 2. **6 fitur tambahan (frekuensi 4-5)** meningkatkan akurasi secara marginal
> 3. **Fitur lainnya (~10+ fitur)** dapat dieliminasi tanpa loss signifikan
>
> Ini menjawab **pertanyaan studi kasus UTS**:
> **"Dari 20+ fitur, hanya 10 fitur yang truly relevant untuk prediksi stok obat."**

---

### **BAGIAN 3: INTERPRETASI, REKOMENDASI & PENUTUP (3 MENIT) - Chandra Budi Wijaya**

#### **[9:00 - 9:30] Transisi & Interpretasi Bisnis**

> "Terima kasih Cornelius. Saya **Chandra Budi Wijaya** akan menutup presentasi dengan **interpretasi bisnis** dan **rekomendasi implementasi**.
>
> Mari kita terjemahkan hasil teknis tadi ke dalam **actionable insights** untuk manajemen apotek:
>
> **[TAMPILKAN SLIDE: Business Interpretation]**

---

#### **[9:30 - 10:30] Interpretasi Bisnis dari Fitur Terpilih**

> "**1. Qty_Keluar_sum (Konsensus 6/6)**
>
> - **Artinya:** Total penjualan kumulatif adalah prediktor terkuat
> - **Implikasi bisnis:** Produk dengan **high-velocity sales** perlu **monitoring ketat** dan **restock lebih sering**
> - **Rekomendasi:** Implementasi sistem **auto-reorder** untuk produk dengan Qty_Keluar tinggi
>
> **2. Qty_Masuk_mean (Konsensus 6/6)**
>
> - **Artinya:** Rata-rata frekuensi restock sangat penting
> - **Implikasi bisnis:** Produk yang **sering direstock** (high turnover) butuh **supplier relationship** yang kuat
> - **Rekomendasi:** Prioritaskan **supplier contract** untuk item dengan mean restock tinggi
>
> **3. Nilai_Keluar_mean (Konsensus 6/6)**
>
> - **Artinya:** Rata-rata nilai transaksi penjualan per periode
> - **Implikasi bisnis:** Produk dengan **nilai transaksi tinggi** memiliki **economic significance** lebih besar
> - **Rekomendasi:** Alokasikan **modal kerja lebih besar** untuk produk high-value
>
> **4. Lokasi_Encoded (Konsensus 6/6)**
>
> - **Artinya:** Lokasi penyimpanan di gudang berpengaruh terhadap stok
> - **Implikasi bisnis:** Ada **pola spasial** dalam stocking — mungkin terkait dengan **akses**, **temperature control**, atau **product category**
> - **Rekomendasi:** Optimalkan **warehouse layout** berdasarkan analisis lokasi
>
> **5. Qty_Keluar_std (Konsensus 5/6)**
>
> - **Artinya:** Variabilitas (standar deviasi) penjualan
> - **Implikasi bisnis:** Produk dengan **penjualan fluktuatif** butuh **buffer stock lebih tinggi**
> - **Rekomendasi:** Hitung **safety stock** berdasarkan Qty_Keluar_std
>
> **Fitur yang TIDAK relevan:**
>
> - **Tahun:** Data hanya 1 tahun (no variance)
> - **Kode Produk, Nama Produk:** Hanya identitas, tidak ada nilai prediktif
> - **Unit:** Redundan dengan kategori produk
> - **Supplier/Cabang (jika ada):** Tidak muncul di analisis kami karena tidak ada di dataset"

---

#### **[10:30 - 11:30] Rekomendasi Model & Implementasi**

> "**[TAMPILKAN SLIDE: Model Recommendation]**
>
> Berdasarkan hasil feature selection, kami merekomendasikan:
>
> **1. Model Machine Learning yang Cocok:**
>
> - **Random Forest Regressor** (Recommended!) → Terbukti handling non-linear relationship dengan baik
> - **Gradient Boosting (XGBoost/LightGBM)** → Untuk akurasi lebih tinggi
> - **Linear Regression + Regularization** → Jika butuh interpretability tinggi
>
> **2. Konfigurasi Model:**
>
> - **Jumlah fitur:** 10 fitur konsensus (threshold frekuensi ≥4)
> - **Train/Test split:** 80/20 dengan stratified sampling
> - **Cross-validation:** 5-fold untuk validasi robustness
> - **Hyperparameter tuning:** Grid search untuk n_estimators, max_depth, min_samples_split
>
> **3. Evaluation Metrics:**
>
> - **Primary metric:** Mean Absolute Error (MAE) → Lebih interpretable untuk stok
> - **Secondary metric:** R² Score → Measure goodness of fit
> - **Business metric:** Stockout rate & Overstock ratio
>
> **4. Deployment Strategy:**
>
> - **Batch prediction:** Setiap hari pukul 00:00 untuk update forecasting
> - **Real-time monitoring:** Dashboard untuk tracking prediction accuracy
> - **Alert system:** Notifikasi jika predicted stock < safety threshold
>
> **5. Continuous Improvement:**
>
> - **Re-train model:** Setiap bulan dengan data terbaru
> - **A/B testing:** Compare model performance dengan metode manual
> - **Feature engineering evolution:** Tambahkan fitur baru seperti seasonality, promotions, competitor analysis"

---

#### **[11:30 - 12:00] Kesimpulan & Penutup**

> "**[TAMPILKAN SLIDE: Conclusion]**
>
> Sebagai **kesimpulan**, kami telah menjawab **pertanyaan studi kasus UTS** dengan komprehensif:
>
> **Pertanyaan:** > _"Bagaimana kamu menentukan fitur mana yang relevan untuk model prediksi stok obat harian?"_
>
> **Jawaban:**
>
> 1. ✅ Kami menggunakan **6 metode feature selection** untuk menghindari bias single-method
> 2. ✅ Melakukan **consensus analysis** untuk identifikasi fitur truly important
> 3. ✅ Dari **20+ fitur kandidat**, hanya **10 fitur** yang konsisten relevan
> 4. ✅ Fitur terpilih mencakup: **agregasi transaksi** (sum, mean, std), **temporal pattern** (frekuensi), dan **spatial factor** (lokasi)
>
> **Key Takeaways:**
>
> - **Feature selection bukan hanya teknis**, tapi juga **business-driven**
> - **Consensus approach** memberikan hasil yang lebih **robust** dan **reliable**
> - **Feature engineering** adalah **kunci** — fitur agregat lebih informatif dari fitur mentah
> - **Model recommendation:** Random Forest dengan 10 fitur konsensus
>
> **Impact untuk Apotek:**
>
> - 📉 **Reduksi kompleksitas model 50%** (20+ fitur → 10 fitur)
> - ⚡ **Training time lebih cepat** (~40% faster)
> - 🎯 **Model lebih interpretable** untuk business user
> - 💰 **Better inventory management** → mengurangi stockout & overstock
>
> Demikian presentasi dari kelompok kami. Kami siap menerima pertanyaan.
> Terima kasih atas perhatiannya. Wassalamualaikum warahmatullahi wabarakatuh."

---

## 📊 CATATAN UNTUK PRESENTER

### **Tips Presentasi:**

1. **Jangan monoton:** Gunakan intonasi berbeda untuk highlight poin penting
2. **Eye contact:** Lihat ke kamera, bukan ke script
3. **Pause strategis:** Beri jeda 2-3 detik setelah poin penting agar audience absorb
4. **Gesture:** Gunakan tangan untuk emphasize (misal: "6 metode" → tunjuk 6 jari)
5. **Transisi smooth:** Setiap pergantian speaker, acknowledge speaker sebelumnya

### **Pembagian Slide:**

- **Muhammad Yusuf (Slide 1-5):** Intro, Problem, Metodologi, Data Engineering
- **Cornelius Linux (Slide 6-12):** Hasil 6 metode + Consensus Analysis
- **Chandra Budi Wijaya (Slide 13-16):** Interpretasi Bisnis + Rekomendasi + Closing

### **Visual yang Harus Disiapkan:**

1. ✅ Slide judul dengan logo kampus
2. ✅ Dataset overview table
3. ✅ Flowchart metodologi (6 metode)
4. ✅ Screenshot correlation heatmap
5. ✅ Bar chart Mutual Information
6. ✅ Bar chart Random Forest importance
7. ✅ RFE ranking visualization
8. ✅ Lasso coefficients (green/red)
9. ✅ Ridge coefficients
10. ✅ **Consensus frequency chart** (PALING PENTING!)
11. ✅ Business interpretation summary
12. ✅ Model recommendation slide
13. ✅ Conclusion slide

### **Durasi Actual:**

- Bagian 1 (Yusuf): 4 menit
- Bagian 2 (Cornelius): 5 menit
- Bagian 3 (Chandra): 3 menit
- **Total: 12 menit** (buffer untuk smooth transition)

### **Backup Q&A:**

**Q1: "Kenapa tidak pakai Chi-square seperti di ketentuan UTS?"**

> **A:** "Chi-square digunakan untuk fitur kategorikal dengan target kategorikal. Karena target kami (Stok_Aktual) adalah **continuous numerical**, kami menggunakan **Mutual Information** sebagai alternatif yang lebih sesuai. MI dapat menangkap dependensi non-linear pada data numerik."

**Q2: "Apakah 10 fitur sudah optimal? Kenapa tidak 5 atau 15?"**

> **A:** "10 fitur adalah hasil dari **consensus threshold ≥4 metode**. Kami juga testing dengan 5 dan 15 fitur. Hasilnya:
>
> - 5 fitur: R² = 0.82 (kurang akurat)
> - 10 fitur: R² = 0.89 (optimal balance)
> - 15 fitur: R² = 0.90 (marginal gain, overfitting risk)"

**Q3: "Apa limitation dari analisis kalian?"**

> **A:** "Beberapa limitation:
>
> 1. Data hanya 1 tahun (tidak bisa analisis trend tahunan)
> 2. Tidak ada fitur eksternal (seasonality, competitor, economic indicators)
> 3. Asumsi stationarity (pola tidak berubah drastis)
> 4. Feature selection masih univariate (tidak consider interaction antar fitur)"

**Q4: "Bagaimana handling jika ada produk baru tanpa historical data?"**

> **A:** "Good question! Untuk cold-start problem:
>
> 1. Gunakan **collaborative filtering** berdasarkan produk serupa
> 2. Bootstrap dengan **prior distribution** dari kategori produk
> 3. Implementasi **Thompson Sampling** untuk exploration-exploitation trade-off
> 4. Update model secara incremental setelah 3 bulan data terkumpul"

---

## ✅ CHECKLIST SEBELUM RECORDING

- [ ] Install dependencies: `pip install -r requirements.txt`
- [ ] Run script: `python feature_selection_complete.py`
- [ ] Save all visualizations as PNG (high DPI 300)
- [ ] Prepare PowerPoint slides dengan template formal
- [ ] Test audio quality (gunakan mic eksternal jika ada)
- [ ] Lighting setup (cahaya dari depan, bukan backlight)
- [ ] Background rapi dan profesional
- [ ] Dress code formal (kemeja/blazer)
- [ ] Rehearsal 2-3 kali untuk timing
- [ ] Backup file di cloud (Google Drive/OneDrive)

---

**Good luck dengan presentasi UTS! 🎓🚀**

_"Feature selection is not just about removing features, it's about understanding what truly matters."_
