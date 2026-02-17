## Scope Project (8 Langkah)

1. **Data Loading & Understanding (freMTPL2)**

   * Import data polis, exposure, jumlah klaim, dan (jika tersedia/di-merge) informasi severity/total loss.
   * Definisikan unit observasi: *policy-year*.
   * Output: ringkasan variabel, missing values, distribusi exposure & claim count.

2. **Data Preparation & Feature Engineering**

   * Bersihkan data (outlier handling, kategori jarang, encoding).
   * Buat fitur turunan yang relevan untuk pricing (mis. banding usia kendaraan, log kepadatan, region grouping).
   * Split data: train/valid/test (hindari leakage; stratify by ClaimNb bila perlu).
   * Output: dataset final siap modeling + data dictionary singkat.

3. **Frequency Model (Claim Count) — Baseline Aktuaria (GLM)**

   * Model **ClaimNb** dengan GLM (Poisson atau Negative Binomial) memakai **offset log(Exposure)**.
   * Cek goodness-of-fit, overdispersion, dan relativities faktor risiko.
   * Output: tabel koefisien/relativity + evaluasi (deviance / Poisson deviance) dan plot diagnostik.

4. **Severity Model (Claim Size) — Baseline Aktuaria**

   * Siapkan data klaim positif (ClaimNb>0) untuk severity.
   * Model severity (Gamma/Tweedie/Lognormal, tergantung ketersediaan target seperti claim amount).
   * Output: metrik error (mis. MAE/RMSE pada log scale) + interpretasi faktor penting.

5. **Pure Premium Construction (Technical Premium)**

   * Hitung **Expected Pure Premium = E[Frequency] × E[Severity]** untuk setiap polis.
   * Buat ringkasan bisnis: rata-rata premium per segmen (region, vehicle age band, power band).
   * Output: tabel “premium relativities” per segmen + sanity check (mis. tren masuk akal).

6. **Model Comparison: GLM vs Gradient Boosting (GBM) vs EBM**

   * Latih tiga pendekatan untuk memprediksi pure premium (atau masing-masing komponen freq & sev):

     * **GLM** sebagai baseline yang mudah diaudit
     * **GBM** (XGBoost/LightGBM/CatBoost) sebagai model akurasi tinggi
     * **EBM** (Explainable Boosting Machine) sebagai “interpretable ML”
   * Bandingkan performa: error metric + **calibration by decile** (prediksi vs aktual per decile).
   * Output: tabel perbandingan + grafik calibration/decile lift.

7. **Explainability: Global & Segment Insights**

   * **Global**: faktor paling berpengaruh (feature importance), bentuk hubungan variabel numerik (PDP/EBM shape).
   * **Segment**: cek konsistensi di segmen kunci (mis. efek vehicle age per region).
   * Output: “pricing insight notes” (1–2 halaman) berisi temuan yang bisa dipresentasikan.

8. **SHAP Reason Codes (Local Explanation) + Mini Underwriting Report**

   * Untuk model GBM (dan opsional EBM), buat **Reason Codes** per polis: 3–5 alasan utama yang menaikkan/menurunkan premium dibanding baseline.
   * Formatkan sebagai kalimat bisnis (contoh: “Premi naik terutama karena wilayah berisiko tinggi dan power kendaraan besar”).
   * Output: contoh 10 polis (case study) + template reason codes + (opsional) demo dashboard sederhana.


