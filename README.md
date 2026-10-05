# Si BIDAK

Repository ini memecah `Si_BIDAK_v6.html` monolitik menjadi komponen yang lebih mudah dikelola di GitHub, sambil mempertahankan struktur runtime aplikasi sedekat mungkin dengan versi satu-file.

## Struktur

```text
.
├── index.html                  # shell UI / markup utama
├── css/                        # 11 blok CSS, urutan cascade dipertahankan
├── js/
│   ├── vendor/chess.js         # chess.js yang semula inline
│   ├── data/                   # konstanta data besar BOOK/P/PC
│   ├── app-main.js             # logika aplikasi utama
│   ├── features/               # AVF, persona, FX, mate, intro, training, trail
│   └── loader.js               # bootstrap loader untuk data + engine + source
├── data/
│   ├── meta/                   # metadata JSON
│   └── books/                  # database / statistik gzip
├── assets/stockfish/           # Stockfish.js + Stockfish WASM
├── tools/                      # PGN Builder ZIP + source Python
└── legacy/Si_BIDAK_v6.html     # sumber satu-file asli
```

## Menjalankan

Karena `js/loader.js` memakai `fetch()` untuk mengambil komponen, jalankan lewat HTTP server, bukan dengan membuka `file://` secara langsung:

```bash
python -m http.server 8000
```

Kemudian buka `http://localhost:8000/`. Struktur ini juga menggunakan path relatif sehingga cocok untuk GitHub Pages.

## Prinsip pemisahan

Source utama tidak di-refactor ke framework atau bundler. CSS dipisah mengikuti blok aslinya, script fitur dipisah mengikuti urutan aslinya, sedangkan payload besar dikeluarkan dari HTML menjadi file data/binary. `loader.js` mengembalikan resource tersebut ke DOM pada runtime karena sebagian logika lama memang mencari elemen `<script>` berdasarkan `id`.

Fitur ekspor database/offline tetap diarahkan menjadi satu HTML self-contained: resource yang sudah dimuat di-inline kembali sebelum file diunduh.

## Lisensi / atribusi

Lihat `THIRD-PARTY-NOTICES.md`. Notice pihak ketiga dari bundle asli dipertahankan, termasuk chess.js, Stockfish, opening data, bidak cburnett, dan sumber database PGN.
