# Evidence Notes M03

Evidence teks diambil pada 2026-10-30 dari server `cc-m03-25832073003`. Uji akses publik (browser dan curl dari laptop) dilakukan pada 2026-10-26.

## Isi folder
- `os.txt`: versi sistem operasi.
- `resources.txt`: CPU, memori, dan disk.
- `ufw.txt`: aturan firewall UFW.
- `ports.txt`: port yang mendengarkan (`ss -ltnp`).
- `service.txt`: status `cc-m03.service`.
- `health-internal.txt`: `GET /health` langsung ke Gunicorn (127.0.0.1:8000).
- `health-public.txt`: `GET /health` melalui Caddy (127.0.0.1:80).

Alamat IP publik server tidak dicantumkan pada file evidence teks.

## Lingkungan
- AWS EC2 t3.micro, region ap-southeast-1 (zona ap-southeast-1a).
- 1 vCPU dengan CPU options CoreCount=1 dan ThreadsPerCore=1.
- Disk EBS gp3 20 GB; swap 1 GB.
- Ubuntu Server 24.04.5 LTS.
- Kesetaraan EC2 dengan "VPS" pada modul belum dikonfirmasi dosen.

## Catatan teknis
- IP publik server bersifat dinamis dan berubah setiap instance di-start.
- Deploy key GitHub awalnya read-only; diganti read/write agar `git push` dari server dapat dilakukan. Key harus dihapus ketika server tidak digunakan lagi (modul bagian 91).
- Gunicorn mencatat `Control server error: [Errno 13]` saat berjalan di bawah systemd karena `ProtectHome=true`. Tidak berdampak pada aplikasi; unit file tidak diubah.

## Kasus troubleshooting: SSH timeout dan reset
- Masalah: koneksi SSH dari laptop ke server berkali-kali timeout atau `Connection reset`.
- Evidence: pengukuran berulang ke `checkip.amazonaws.com` menunjukkan IP publik laptop berganti beberapa alamat dalam satu sesi, dan pada satu waktu sebagian permintaan gagal (jaringan tidak stabil). Security Group membatasi port 22 hanya ke alamat IP klien.
- Hipotesis: IP klien yang dilihat server tidak ada di aturan Security Group, atau koneksi laptop sedang tidak stabil.
- Uji: mengukur IP laptop beberapa kali, membandingkannya dengan aturan Security Group, dan menguji ketersediaan port 22 ke tujuan lain.
- Perbaikan: menambahkan aturan /32 untuk IP yang terukur stabil, memakai jaringan yang stabil, dan menambahkan opsi `ServerAliveInterval` pada SSH.
- Verifikasi: login SSH berhasil dari jaringan dengan IP stabil.
- Catatan keterbatasan: pada satu jaringan seluler, SSH tetap timeout walaupun IP yang terukur sudah diizinkan. Penyebab pastinya belum diketahui dan dilanjutkan dari jaringan lain.
