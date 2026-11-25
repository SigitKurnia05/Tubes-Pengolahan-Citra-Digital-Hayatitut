# Deteksi Kondisi Daun Melati (Sehat, Kering, Menguning)

Tugas Besar Pengolahan Citra Digital (IF25-40410), Program Studi Teknik Informatika, Institut Teknologi Sumatera, 2026.

Topik 7: Deteksi kondisi daun tanaman hias atau sayuran.

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

## Tim

| No | NIM | Nama | Peran |
|---|---|---|---|
| 1 | | | Project Lead |
| 2 | | | Data Engineer & Annotator |
| 3 | | | Computer Vision Engineer |
| 4 | | | Evaluation & QA Analyst |
| 5 | | | Application Engineer |

Dosen pengampu: Andika Setiawan, S.Kom., M.Cs. (Kelas RA) / Mohamad Idris, S.Si., M.Sc. (Kelas RB)

## Sitasi dan Lisensi

Kode dalam repositori ini berlisensi MIT (lihat `LICENSE`). Dataset AJLCD-2025 berlisensi CC BY 4.0 dan dimiliki pembuatnya; mohon sitasi sumber aslinya bila menggunakan data.

R. Ayon et al., "Arabian jasmine leaf condition dataset (AJLCD-2025)," Mendeley Data, V1, 2025, doi: 10.17632/nwdzwk89zs.1.
