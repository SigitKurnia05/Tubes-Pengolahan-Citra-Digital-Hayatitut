# Deteksi Kondisi Daun Melati (Sehat, Kering, Menguning)

Tugas Besar Pengolahan Citra Digital (IF25-40410), Program Studi Teknik Informatika, Institut Teknologi Sumatera, 2026.

- **Kelompok:** Hayatitut Citra Vision (Kelas RB)
- **Topik:** 7 — Deteksi kondisi daun tanaman hias atau sayuran
- **Proposal:** [Google Docs](https://docs.google.com/document/d/1Xjy4mrjELiYlMW_zxPInEZwVWHCmU7W0a5cAKL31-zI/edit?usp=sharing) | [PDF (`docs/`)](docs/Proposal%20Tubes%20PCD%202026_2027%20-%20Hayatitut%20Citra%20Vision.pdf)

## Deskripsi

Proyek ini mengklasifikasikan kondisi daun melati (*Jasminum sambac*) ke dalam tiga kelas: **sehat**, **kering**, dan **menguning**, memakai pipeline pengolahan citra klasik. Satu citra daun diproses melalui pra-pemrosesan, segmentasi daun dari latar, ekstraksi fitur warna dan tekstur, lalu klasifikasi. Hasilnya dapat dicoba lewat aplikasi demo berbasis kamera.

Kontribusi proyek ada pada pipeline yang transparan dan terukur (termasuk ablasi tiap tahap), bukan pada arsitektur model yang paling canggih.

## Dataset

Data sekunder dari satu sumber: **Arabian Jasmine Leaf Condition Dataset (AJLCD-2025)**.

- Penulis: Rokonozzaman Ayon dkk.
- Mendeley Data, Versi 1, 21 Juli 2025
- DOI: [10.17632/nwdzwk89zs.1](https://doi.org/10.17632/nwdzwk89zs.1)
- Tautan: https://data.mendeley.com/datasets/nwdzwk89zs/1
- Lisensi: CC BY 4.0

Kelas yang dipakai:

| Kelas proyek | Folder dataset | Jumlah citra |
|---|---|---|
| Sehat | `Healthy` | 955 |
| Kering | `Dried Leaf` | 916 |
| Menguning | `Cholorosis` (ejaan folder asli) | 699 |

Data **tidak disertakan** di repositori. Unduh sendiri dari tautan di atas, lalu taruh tiga folder kelas di `data/raw/`. Lihat `data/README.md` bila sudah tersedia.

## Pipeline

1. Pra-pemrosesan (resize, CLAHE, koreksi pencahayaan)
2. Segmentasi daun (ExG, Otsu, HSV, morfologi)
3. Ekstraksi fitur (HSV, CIELAB, GLCM, LBP, Canny)
4. Klasifikasi (SVM RBF sebagai model utama; pembanding: KNN, decision tree, Random Forest, baseline Otsu, baseline aturan warna)
5. Evaluasi (akurasi, presisi, recall, macro-F1, confusion matrix, IoU, PSNR/SSIM)

## Struktur Repositori

```
.
├── app/        # aplikasi demo (situs berbasis kamera)
├── data/       # data mentah/olahan (tidak di-commit) dan splits/*.csv
├── docs/       # proposal, laporan akhir, slide
├── models/     # model terlatih dan scaler
├── results/    # metrik, confusion matrix, hasil uji lapangan
├── src/        # kode: praproses, segmentasi, fitur, model, evaluasi
├── LICENSE
└── README.md
```

## Instalasi

Prasyarat: Python 3.10 atau lebih baru (sesuaikan dengan versi yang dipakai tim).

```bash
python -m venv .venv
# Windows (PowerShell): .venv\Scripts\Activate.ps1
# macOS/Linux:          source .venv/bin/activate
pip install -r requirements.txt
```

> `requirements.txt` akan ditambahkan setelah daftar library tim ditetapkan.

## Cara Menjalankan

> Bagian ini diisi setelah kode tersedia. Rencana urutannya:

1. Unduh dataset ke `data/raw/`.
2. Buat pembagian data terstratifikasi 70/15/15 (dengan pemeriksaan duplikat), simpan ke `data/splits/`.
3. Latih dan evaluasi model.
4. Jalankan aplikasi demo dari folder `app/`.

## Evaluasi

Pembagian data terstratifikasi 70% latih, 15% validasi, 15% uji, dengan data uji hanya dipakai sekali di akhir. Validasi silang 5-fold dilakukan pada data latih.

Target kinerja:

| Ukuran | Target |
|---|---|
| IoU segmentasi daun | ≥ 0,80 |
| Akurasi 3 kelas | ≥ 85% |
| Macro F1-score | ≥ 0,83 |
| Selisih akurasi terhadap baseline | ≥ 5 poin persentase |
| Waktu inferensi pada aplikasi demo | ≤ 2 detik per citra |

Selain data uji dataset, sistem diuji pada foto daun melati yang diambil sendiri melalui aplikasi demo (uji lapangan). Hasilnya dilaporkan terpisah di `results/`.

## Keterbatasan

- Model dilatih pada satu spesies (melati), satu perangkat (Xiaomi M2102J20SG), dan satu lokasi (Sonargaon, Bangladesh). Kemampuan generalisasi ke tanaman, perangkat, atau lokasi lain tidak diuji.
- Aplikasi demo hanya ditujukan untuk daun melati.
- Kelas menguning dipetakan dari kelas *Chlorosis* pada dataset; kriteria pelabelannya tidak dijelaskan oleh pembuat dataset.

## Progress Pengerjaan

| Minggu | Tanggal | Kegiatan | Penanggung Jawab |
|---|---|---|---|
| 11 | 9–13 Nov 2026 | Penyusunan proposal, pengumpulan dan pelabelan data, baseline awal | Project Lead (proposal); Data Engineer & Annotator (unduh dan periksa data, pembagian data); Computer Vision Engineer (baseline awal) |
| 12 | 16–20 Nov 2026 | Implementasi tahapan pengolahan citra | Computer Vision Engineer (implementasi tahap 1 sampai 5); Data Engineer & Annotator (mask 30 citra); Application Engineer (kerangka aplikasi demo) |
| 13 | 23–27 Nov 2026 | Evaluasi dan ablasi | Evaluation & QA Analyst (evaluasi, ablasi, uji ketahanan); Computer Vision Engineer (penyetelan parameter) |
| 14 | 30 Nov–4 Des 2026 | Aplikasi demo, penyempurnaan, penyusunan laporan | Application Engineer (aplikasi dan video demo); Project Lead (laporan); seluruh anggota (penulisan bagian masing-masing) |
| 15 | 7–11 Des 2026 | Presentasi dan demo (sesuai jadwal) | Seluruh anggota |
| 16 | 14–18 Des 2026 | Presentasi dan demo (sesuai jadwal) | Seluruh anggota |

## Tim

**Kelompok:** Hayatitut Citra Vision (Kelas RB)

| No | NIM | Nama | Peran |
|---|---|---|---|
| 1 | 123140033 | Sigit Kurnia Hartawan | Project Lead |
| 2 | 123140031 | Muhammad Romadhon S | Data Engineer & Annotator |
| 3 | 123140003 | Muhammad Fadhilah Akbar | Computer Vision Engineer |
| 4 | 123140068 | Giovan Lado | Evaluation & QA Analyst |
| 5 | 123140072 | Bayu Brigas Novaldi | Application Engineer |

Dosen pengampu: Mohamad Idris, S.Si., M.Sc. (Kelas RB)

## Sitasi dan Lisensi

Kode dalam repositori ini berlisensi MIT (lihat `LICENSE`). Dataset AJLCD-2025 berlisensi CC BY 4.0 dan dimiliki pembuatnya; mohon sitasi sumber aslinya bila menggunakan data.

R. Ayon et al., "Arabian jasmine leaf condition dataset (AJLCD-2025)," Mendeley Data, V1, 2025, doi: 10.17632/nwdzwk89zs.1.
