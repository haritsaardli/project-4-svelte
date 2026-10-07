# 🌤️ Weather + AQI App

🔗 **Live Demo:** [project-4-svelte.haritsaardli.workers.dev](https://project-4-svelte.haritsaardli.workers.dev/)

Aplikasi cuaca berbasis web yang menampilkan kondisi cuaca terkini beserta indeks kualitas udara (AQI) berdasarkan lokasi pengguna atau kota yang dicari.

## Fitur

- Deteksi lokasi otomatis via GPS (Geolocation API)
- Pencarian cuaca berdasarkan nama kota
- Informasi cuaca lengkap: suhu, kondisi, kecepatan & arah angin, kelembaban, feels like, suhu maks/min
- **Indeks Kualitas Udara (AQI)** skala 1–5 dengan label status berwarna (Good, Fair, Moderate, Poor, Very Poor)
- Konsentrasi PM2.5 dalam µg/m³

## Teknologi

| Teknologi | Kegunaan |
|---|---|
| [Svelte](https://svelte.dev) | Framework UI utama |
| [Rollup](https://rollupjs.org) | Bundler |
| [Bootstrap 4](https://getbootstrap.com/docs/4.6/) | Styling & layout |
| [OpenWeatherMap API](https://openweathermap.org/api) | Data cuaca (`/data/2.5/weather`) |
| [OpenWeatherMap Air Pollution API](https://openweathermap.org/api/air-pollution) | Data AQI & PM2.5 (`/data/2.5/air_pollution`) |
| Browser Geolocation API | Deteksi koordinat pengguna |

## Cara Menjalankan

Install dependencies:

```bash
npm install
```

Jalankan development server:

```bash
npm run dev
```

Buka [localhost:5000](http://localhost:5000) di browser.

## Build Production

```bash
npm run build
npm run start
```

## Struktur Project

```
src/
├── main.js               # Entry point
├── App.svelte            # Root component
└── components/
    ├── Search.svelte     # Komponen utama (cuaca + AQI)
    └── Info.svelte       # Komponen info (tidak aktif)
```
