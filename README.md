# MallOps Suite

**Digitalisasi operasional gedung dalam satu sistem** - parkir berbasis sensor, meter listrik/air IoT, leasing & billing otomatis, procurement + e-Faktur, portal tenant, dan bot monitoring. Dibangun end-to-end di atas **Odoo 19 Community** (open source, tanpa biaya lisensi): analisis kebutuhan -> desain (PRD + acceptance criteria) -> implementasi -> 67 test method hijau -> demo E2E live.

> Kode sumber bersifat privat (tersedia untuk review teknis saat proses klien).
> Halaman penjualan: **[biotama.cv/mallops](https://biotama.cv/mallops)**

## Modul

| Modul | Isi |
|---|---|
| `mall_leasing` | Unit & tenant, kontrak sewa (LC/2026/xxxx), eskalasi tahunan, rent-free, slot parkir per kontrak (Office Edition) |
| Billing & e-Faktur | Tagihan otomatis + denda + reminder, PDF report, **e-Faktur XML bentuk DJP** (NSFP 16 digit, NPWP, per baris) |
| `api_hub` | REST API 25 endpoint (`/api/v1/*`, portal, gate), API key + scope + rate limit, webhook HMAC |
| `payment_doku` | DOKU VA/QRIS: webhook, auto-reconcile, expiry |
| IoT Meter | Sketsa ESP32 + gateway push `/utility-readings` (kWh & air per unit tenant) |
| Portal & Bot | Portal tenant Astro+React (skeleton loader, mobile-first) + bot Telegram ops |
| `mall_operations` | Purchase request -> approval -> **PO otomatis per vendor**; e-Faktur |
| `mall_parking` | Parkir ala operator Indonesia: tarif progresif (grace/cap/event), tiket idempotent, **5 mode masuk** (tiket/QR/member/**e-money bank**/LPR), bayar tunai/QRIS/e-money auto-debit, validasi tenant, member bulanan, shift kasir + variance, **gate registry + heartbeat**, **sensor lahan (bay/loop) -> papan SISA real-time**, retensi foto UU PDP |

## Arsitektur (ringkas)

```
[Perangkat lapangan]          [Core Odoo 19]                 [Klien]
ESP32 gate/sensor/meter  -->  REST API (scope per device) --> Portal tenant (Astro)
Kamera LPR (HTTP push)   -->  mall_parking / IoT meter  -->  Bot Telegram
Kasir & back office      -->  Billing / e-Faktur / PR-PO -->  Laporan & dashboard
```

Prinsip: gate harus tetap bekerja saat internet mati (offline mode), Odoo sebagai
pencatat kejadian; satu token per device (roadmap F9); data milik gedung.

## Kualitas

- **67 test method hijau** di database fresh (0 gagal) - lima modul
- Demo E2E terbukti: LPR masuk -> tiket -> QRIS -> gate buka; tap e-money; sensor -> papan SISA
- Issue log 32 pelajaran teknis (disiplin dokumentasi per fase F0-F8)
- PRD per fitur dengan acceptance criteria sebelum eksekusi

## Screenshot

Lihat folder `screenshots/` (dashboard Odoo, portal tenant, bot).

## Kontak

Agustinus Biotamalo Lumbantoruan - [biotama.cv](https://biotama.cv) - fxwisdom1@gmail.com
