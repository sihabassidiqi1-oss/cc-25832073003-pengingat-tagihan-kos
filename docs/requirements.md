# Requirements

## Problem Statement
[ASUMSI, perlu verifikasi baseline 14 hari] Pencatatan keuangan harian dan tugas/deadline kuliah penyusun masih tersebar dan tidak konsisten, sehingga pengeluaran sulit dilacak dan deadline berisiko terlewat. Belum ada satu sumber data pribadi yang tahan lama, dapat diakses dari berbagai perangkat, dan dapat diekspor.

## Target Users
- Pengguna utama: Sihab Assidiqi (pemilik, 1 pengguna aktif).
- Pengguna tambahan opsional (orang terpercaya) hanya setelah gate privasi lulus.

## Functional Requirements
- FR-01 Pengguna terautentikasi dapat menambah transaksi (jenis, nominal, kategori, tanggal, catatan) dalam <=30 detik. [Must; direncanakan M03/M05; belum diimplementasikan pada baseline M03]
- FR-02 Sistem menyimpan data transaksi secara persisten di SQLite dan tetap utuh setelah restart/reboot. [Must; direncanakan M05; belum diimplementasikan]
- FR-03 Sistem menyediakan GET /health untuk status proses. [Must; terimplementasi dan teruji pada M03]
- FR-04 Pengguna terautentikasi dapat membaca, mengubah, menghapus transaksi dan tugas, serta mengekspor CSV; akses anonim ditolak. [Should; direncanakan setelah M05; belum diimplementasikan]
- FR-05 Input tidak valid (nominal kosong/negatif, tanggal salah) ditolak dengan pesan jelas tanpa membocorkan detail internal. [Should; direncanakan setelah M05; belum diimplementasikan]

## Non-Functional Requirements
- NFR-01 backend loopback-only
- NFR-02 application managed by systemd
- NFR-03 public request through reverse proxy
- NFR-04 no secret in repository

Catatan pemetaan: NFR-01 s.d. NFR-04 di atas adalah kebutuhan deployment M03 dari modul. Pada Studi Kelayakan, "backend loopback-only" dan "no secret in repository" termasuk NFR-01 (Security), sedangkan "managed by systemd" mendukung NFR-03 (Reliability, restart otomatis). "Public request through reverse proxy" menjadi dasar HTTPS pada M04.

## Constraints
- 1 vCPU
- 1 GB RAM
- 20 GB disk
- Ubuntu Server 24.04
- public IPv4

Nilai terverifikasi (FAKTA, diukur 2026-10-26):
- vCPU: 1 (`lscpu`: CPU(s)=1, 1 thread per core, 1 core per socket).
- RAM: 909 MiB terbaca sistem (`free -h`), nominal 1 GB.
- Disk: volume EBS gp3 20 GB; sistem file `/` 19G, terpakai 3.8G (`df -h /`).
- OS: Ubuntu 24.04.5 LTS (`/etc/os-release`).
- Public IPv4: dinamis; 13.212.27.178 saat verifikasi, berubah setiap instance di-start.

## Acceptance Criteria M03
- [x] first deployment accessible (diuji dari internet, 2026-10-26: HTTP 200 pada http://13.212.27.178/)
- [x] health endpoint works (diuji dari internet, 2026-10-26: GET /health mengembalikan status "ok")
