# ADR-0001: Initial VPS Deployment Architecture

## Status
Accepted

## Context
VPS 1 vCPU, RAM 1 GB, disk 20 GB, Ubuntu Server 24.04.

Diimplementasikan pada AWS EC2 t3.micro (CPU options CoreCount=1, ThreadsPerCore=1); lihat `docs/architecture.md`.

## Decision
- Flask
- Gunicorn 1 worker
- systemd
- Caddy
- UFW
- GitHub

## Alternatives
1. Node.js + PM2
2. NGINX + Gunicorn
3. Docker
4. PHP-FPM

## Rationale
- Flask: framework ringan yang cukup untuk aplikasi pengelola tagihan kos dan mudah dikembangkan menjadi CRUD dengan SQLite pada M05.
- Gunicorn 1 worker, 2 thread: menjaga headroom CPU dan RAM (1 vCPU, 909 MiB terbaca) untuk SSH, systemd, Caddy, dan operasi paket. Pengukuran 2026-10-26: layanan memakai sekitar 29.6 MB RAM.
- systemd: aplikasi hidup otomatis saat boot (enabled) dan dijalankan ulang bila gagal (Restart=on-failure).
- Caddy: reverse proxy dengan konfigurasi singkat dan siap HTTPS otomatis ketika domain tersedia pada M04.
- UFW: hanya SSH dan HTTP yang terbuka; backend Gunicorn hanya mendengarkan di 127.0.0.1:8000.
- GitHub: repository sebagai sumber kebenaran; server mengambil kode lewat deploy key tanpa write access.
- Alternatif tidak dipilih untuk M03: Node.js + PM2 dan PHP-FPM karena proyek memakai Python/Flask; NGINX + Gunicorn setara secara fungsi tetapi Caddy dipilih karena konfigurasi lebih ringkas dan HTTPS otomatis pada M04; Docker berada di luar lingkup M03 dan dikerjakan pada M06.

## Consequences
### Positive
- Footprint kecil (Caddy sekitar 9 MB, layanan aplikasi sekitar 30 MB) sehingga muat di 1 GB RAM.
- Backend tidak terpapar ke internet; satu pintu masuk publik melalui Caddy.
- Deployment dapat diulang dari dokumen `docs/deployment.md`.

### Negative
- HTTP saja sampai M04, sehingga lalu lintas belum terenkripsi.
- Satu server tanpa redundansi; satu worker dan dua thread membatasi konkurensi.
- Deployment masih manual (git pull dan restart service).
- Gunicorn mencatat error non-fatal pada control server karena `ProtectHome=true`; fungsi aplikasi tidak terpengaruh.

## Review Trigger
Review pada M06/M07.
