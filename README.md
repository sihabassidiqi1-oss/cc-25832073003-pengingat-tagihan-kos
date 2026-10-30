# CatatKu Cloud

Cloud Computing — PTI 2802  
Pertemuan 3 — Project Inception

## Author
Sihab Assidiqi / 25832073003 / Kelas: PTI 2A 2025

## Problem
[ASUMSI, perlu verifikasi baseline 14 hari] Pencatatan keuangan harian dan tugas/deadline kuliah masih tersebar dan tidak konsisten, sehingga pengeluaran sulit dilacak dan deadline berisiko terlewat. Belum ada satu sumber data pribadi yang tahan lama, dapat diakses dari berbagai perangkat, dan dapat diekspor.

## Target Users
Pemilik proyek (satu pengguna aktif); pengguna tambahan opsional hanya setelah gate privasi lulus.

## Features M03
- baseline application
- health endpoint
- VPS deployment

## Architecture

```mermaid
flowchart LR
    U[User] --> C[Caddy]
    C --> G[Gunicorn]
    G --> F[Flask]
```

## Infrastructure
- Ubuntu Server 24.04
- 1 vCPU
- 1 GB RAM
- 20 GB disk
- Caddy
- Gunicorn
- Host: AWS EC2 t3.micro (ap-southeast-1), 1 vCPU melalui CPU options

## Public Endpoint
`http://13.212.27.178/` (IP dinamis; terakhir diverifikasi 2026-10-26)

## Health Check
`GET /health`

## Deployment
See `docs/deployment.md`.

## Security
- key-only SSH
- root SSH disabled
- UFW enabled
- backend loopback-only

## Current Limitations
- HTTP only
- no persistent database
- no container
- manual deployment

## Roadmap
M04 DNS/HTTPS, M05 Data, M06 Container, M07 IaC, M09 CI/CD
