# MindleStore — Where Did the Profit Go?

> Investigasi kenapa penjualan MindleStore sehat tapi labanya stagnan. Hasilnya: satu kebijakan cap diskon 30% bisa naikkan profit **+43,6%** tanpa menyentuh 89% penjualan yang sehat.

## Business Problem

MindleStore beroperasi di 4 region & 531 kota dengan penjualan yang terus jalan, tapi laba mandek dan sebagian produk laris justru merugi. Pertanyaan intinya: **di mana margin bocor, dan tuas mana yang paling besar dampaknya untuk memperbaikinya?** Dikerjakan untuk Data Insight Challenge 2026.

## Dataset

- **Sumber:** MindleStore (Superstore retail), CSV single-table
- **Volume:** 9.994 transaksi · 37.873 unit terjual
- **Dimensi:** 4 region · 531 kota · 3 segmen pelanggan · 3 kategori / 17 sub-kategori
- **Metrik:** sales, quantity, discount, profit

## Arsitektur & Pendekatan

Medallion architecture di SQL Server (T-SQL):

- **Bronze** — raw ingestion via `BULK INSERT`, semua kolom `VARCHAR` (no assumption soal tipe data).
- **Silver** — cleansing + enrichment: casting via `TRY_CAST`, flag `is_loss` / `is_duplicate`, kolom turunan `discount_band`, `profit_margin`, `unit_price`. QC flag disimpan, baris tidak dibuang.
- **Gold** — ~12 analytical view siap-BI (KPI summary, profit by category/sub-category/region/state, discount band & sensitivity, segment performance & mix).

Pemisahan layer ini bikin logika cleansing terisolasi dari logika analitik, jadi setiap view Gold bisa dilacak balik ke transformasi Silver yang jelas.

## Tools & Tech

`SQL Server` · `T-SQL` · `Medallion (Bronze–Silver–Gold)` · `Power BI Desktop` · `Custom Visual (HTML Content)`

## Key Findings

- **18,7% transaksi dijual rugi** — masalahnya bukan volume, tapi cara penjualan dihasilkan.
- **Titik kritis diskon ada di 30%.** Band 0% margin +29,5%; di atas 20% setiap band rugi, band 41–60% anjlok ke −40,7%.
- **Diskon menghancurkan margin secara tak merata:** Appliances jatuh dari +29,7% ke **−255,1%** saat diskon ≥30%; Binders paling berbahaya (volume diskon-tinggi terbanyak, 613 txn).
- **Ketimpangan wilayah dari kebijakan diskon:** Central mendiskon **2,2× lebih dalam** dari West (24,0% vs 10,9%) dan jadi region paling buntung.
- **49,6% profit datang dari 3 produk Technology** (Copiers, Phones, Accessories).
- **Simulasi cap diskon ≤30%:** profit $286 Rb → **$411 Rb (+43,6%)**, margin 12,5% → 20,2%, dengan hanya membuang 11,3% penjualan yang tidak sehat.

## Struktur Repo
mindlestore-profit-analysis/
├── README.md
├── raw_data/                # dataset mentah (MindleStore.csv)
├── Analyst/                 # SQL Medallion: Bronze → Silver → Gold
├── Visualisasi/             # dashboard Power BI (.pbix / export PDF)
└── Presentation Deck/       # storyline deck (PDF)

## 🛡️ License

Project ini dilisensikan di bawah [MIT License](LICENSE). Bebas dipakai, dimodifikasi, dan dibagikan dengan atribusi yang sesuai.

## 🌟 About Me

Hai! Aku **Ghifarry Alnaffriza**, mahasiswa Pembangunan Ekonomi Kewilayahan di Sekolah Vokasi UGM yang lagi ngebangun jalan sebagai **data/business analyst**. Aku suka ngulik data buat nemuin cerita di balik angka kayak project MindleStore ini.

Yuk connect! Boleh reach out lewat platform berikut:

[![LinkedIn](https://img.shields.io/badge/LINKEDIN-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/ghifarryalnaffriza)
[![GitHub](https://img.shields.io/badge/GITHUB-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/ghifarryalnaffriza)
[![Email](https://img.shields.io/badge/EMAIL-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:ghifaralns@gmail.com)
