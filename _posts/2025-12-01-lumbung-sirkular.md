---
layout: post
title: "Lumbung Sirkular: Platform Digital Ekonomi Sirkular"
description: "Aplikasi marketplace limbah dan manajemen daur ulang berbasis peta interaktif untuk menghubungkan penghasil limbah dengan pendaur ulang."
date: 2025-12-20 10:00:00 +0800
categories: [Web Development, Green Tech]
tags: [React, FastAPI, Python, Leaflet, Tailwind CSS]
image:
  path: assets/images/projects/lumbung-sirkular/home2.png
  alt: Lumbung Sirkular Thumbnail
---

## Ringkasan Proyek

Lumbung Sirkular adalah platform inovatif yang bertujuan untuk mendukung ekosistem ekonomi sirkular di Indonesia. Aplikasi ini berfungsi sebagai jembatan digital yang menghubungkan Penghasil Limbah (Producer) dengan Pendaur Ulang (Recycler).

Proyek ini dibangun untuk mengatasi masalah rantai pasok sampah yang tidak efisien dengan menyediakan fitur pelacakan lokasi, transaksi jual-beli limbah, dan sertifikasi dampak lingkungan secara digital.

## Teknologi yang Digunakan

Proyek ini menggunakan stack teknologi modern dengan fokus pada performa dan skalabilitas.

- Frontend: React (Vite), React-Leaflet, Tailwind CSS
- PDF generation (client): jsPDF & html2canvas
- Backend: Python (FastAPI), SQLAlchemy
- Auth: JWT
- Database: PostgreSQL / SQLite

## Fitur Unggulan

1. Marketplace Limbah & Booking System — pengguna dapat mengunggah jenis limbah dengan foto dan deskripsi; pendaur ulang dapat melakukan booking.
2. Peta Interaktif (Geolocation) — visualisasi titik lokasi limbah dan pendaur ulang untuk memudahkan logistik.
3. Impact Certificate Generator — setelah transaksi selesai, sistem meng-generate sertifikat dampak lingkungan (PDF).
4. Multi-Dashboard — `Producer Dashboard` untuk pelacakan penjualan; `Recycler Dashboard` untuk manajemen permintaan dan stok.

## Struktur Arsitektur (singkat)

```
backend/
├─ app/
│  ├─ routes/    # auth, transactions, wastes, upload
│  ├─ models.py
│  ├─ schemas.py
│  └─ auth.py
└─ uploads/

frontend/
└─ src/
   ├─ components/
   │  ├─ map/       # MapViewer, MapPicker
   │  ├─ impact/    # Certificate generator
   │  └─ waste/     # Listing & booking
   ├─ context/
   ├─ pages/
   └─ services/
```

## Tantangan & Solusi

- Tantangan: Visualisasi data geospasial dan pilihan titik jemput yang akurat.
- Solusi: Menggunakan `react-leaflet` dengan komponen `MapPicker` yang menangkap koordinat saat peta diklik dan menyimpannya ke backend.

Contoh implementasi `MapPicker` (React + react-leaflet):

```js
const MapPicker = ({ onLocationSelect }) => {
  useMapEvents({
    click(e) {
      const { lat, lng } = e.latlng;
      setMarker({ lat, lng });
      onLocationSelect({ lat, lng });
    },
  });

  return marker ? <Marker position={[marker.lat, marker.lng]} /> : null;
};
```

## Galeri Antarmuka

| Marketplace Limbah | Peta Persebaran |
|---|---|
| ![](assets/images/projects/lumbung-sirkular/marketplace.png) | ![](/assets/images/projects/lumbung-sirkular/map.jpg) |

| Dashboard Recycler | Sertifikat Digital |
|---|---|
| ![](/assets/images/projects/lumbung-sirkular/dashboard.jpg) | ![](/assets/images/projects/lumbung-sirkular/certificate.jpg) |

## Tautan

- Repository: ([Lumbung Sirkular GitHub](https://github.com/Rezka08/lumbung-sirkular_trio-ayam-jantan.git))

---
Jika Anda ingin, saya bisa: memperbaiki path gambar konkret, menambahkan alt text, atau membangun situs lokal untuk melihat hasilnya.
 Lihat di GitHub