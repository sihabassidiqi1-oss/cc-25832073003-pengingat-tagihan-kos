# Manual Deployment M03

## Application Path
`/srv/cc-m03`

## Service
`cc-m03.service`

## Backend
`127.0.0.1:8000`

## Public Endpoint
`http://13.212.27.178/` (IP publik dinamis: berubah setiap instance di-start; terakhir diverifikasi 2026-10-26)

## Update Procedure

```bash
cd /srv/cc-m03
git pull
source .venv/bin/activate
python -m pip install -r requirements.txt
sudo systemctl restart cc-m03
sudo systemctl status cc-m03 --no-pager
curl -fsS http://127.0.0.1:8000/health
curl -fsS http://127.0.0.1/health
```

## Verification Result (2026-10-26)
- `cc-m03.service`: active (running), enabled.
- `ss -ltnp`: Gunicorn mendengarkan di 127.0.0.1:8000 (bukan 0.0.0.0).
- Caddy: validasi konfigurasi dan reload berhasil; `curl http://127.0.0.1/health` mengembalikan status "ok".
- Uji dari internet (laptop dan browser): HTTP 200 pada `/` dan `/health`; header `Via: 1.1 Caddy`.
