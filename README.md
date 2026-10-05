# Chessreps PGN Builder

Paket ini dibuat untuk membangun database PGN Chessreps dari sumber gratis/resmi.

## Sumber
1. TWIC: https://theweekinchess.com/twic
2. Lichess Broadcast Database: https://database.lichess.org/broadcast/

TWIC menyediakan PGN mingguan. Lichess menyediakan official broadcast games dalam `.pgn.zst`.

## Cara paling mudah di Windows

### 1. Install Python
https://www.python.org/downloads/

Saat instalasi centang **Add Python to PATH**.

### 2. Install dependency
Buka PowerShell/CMD di folder ini:

```bash
pip install python-chess requests beautifulsoup4
```

### 3. Download TWIC otomatis
Contoh:

```bash
python chessreps_builder.py --twic 1664 1663 1662 1661 1660
```

Untuk rentang:

```bash
python chessreps_builder.py --twic-range 1600 1664
```

### 4. Download Lichess Broadcast
Contoh 2026:

```bash
python chessreps_builder.py --broadcast 2026-01 2026-02 2026-03 2026-04 2026-05 2026-06 2026-07 2026-08 2026-09
```

Atau rentang:

```bash
python chessreps_builder.py --broadcast-range 2025-01 2026-09
```

Script otomatis mengunduh `.pgn.zst` dan mendekompressnya jika `zstandard` tersedia.

Install zstandard:

```bash
pip install zstandard
```

### 5. Gabungkan dan deduplicate

```bash
python chessreps_builder.py --merge
```

Output:

- `output/master_games.pgn`
- `output/master_games_clean.pgn`

### 6. Buat database GM

Default:

```bash
python chessreps_builder.py --gm
```

Output:

- `output/gm_games.pgn`

Default filter: salah satu pemain memiliki Elo >= 2500.

Untuk lebih ketat:

```bash
python chessreps_builder.py --gm --min-elo 2600
```

## Rekomendasi untuk Chessreps

Jangan memasukkan miliaran game Lichess standard ke Chessreps. Gunakan:
- TWIC untuk game turnamen/profesional.
- Lichess Broadcast untuk official broadcast.
- Filter GM/top-level.
- Deduplicate sebelum membuat opening repertoire.

## Catatan lisensi

Lichess menyatakan broadcast games dirilis di bawah CC BY-SA 4.0.
TWIC menyatakan majalahnya gratis untuk penggunaan personal dan semua hak dilindungi. Untuk aplikasi publik/komersial, cek ketentuan masing-masing sumber dan hubungi TWIC jika diperlukan.

## Struktur folder

```text
chessreps_pgn_builder/
├── chessreps_builder.py
├── requirements.txt
├── README.md
├── raw/
│   ├── twic/
│   └── broadcast/
└── output/
```

## Mengatur ukuran database

Builder ini sengaja berupa skrip, bukan ZIP berisi jutaan PGN. Ukuran database ditentukan sendiri:

```bash
python chessreps_builder.py --merge --gm --min-elo 2600 --since 2015 --max-games 50000
```

- `--min-elo` : Elo minimal salah satu pemain
- `--since` : hanya partai dari tahun tertentu
- `--max-games` : batas jumlah partai pada `gm_games.pgn`

File broadcast `.pgn.zst` cukup besar dan hasil dekompresinya jauh lebih besar, jadi mulai dari ukuran kecil.

## Statistik offline (tanpa PGN mentah)

```bash
python chessreps_builder.py --stats --embed index.html
```

- `output/stats.json` : jumlah partai, hasil Putih/Remis/Hitam, dan langkah teratas per posisi
- `output/index_offline.html` : salinan Si BIDAK dengan statistik tertanam di dalam file

`stats.json` juga bisa dimuat lewat tombol "Muat file PGN" di aplikasi.
