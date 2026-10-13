# DentCAD AI

Open-source-first dental crown CAD MVP: upload dental mesh scans, preview them in 3D, define a preparation margin, generate a **preliminary** crown proposal, inspect mesh diagnostics, and export STL.

> Research / technical-evaluation software only. Not validated for clinical use. Do not use its output for patient treatment or manufacture without independent professional review and validation.

## Stack
- FastAPI backend
- Trimesh + NumPy for mesh intake, measurements and STL output
- React + Vite + TypeScript + Three.js frontend
- No paid inference API. AI models are not silently mocked or claimed to run.
- Docker Compose for local deployment

## Quick start
Prerequisite: Docker + Docker Compose.

```bash
docker compose up --build
```
Open http://localhost:8080

### Local development
Backend:
```bash
cd backend
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000
```
Frontend:
```bash
cd frontend
npm install
npm run dev
```
Set `VITE_API_BASE=http://localhost:8000` in `.env.local` if needed.

## MVP workflow
1. Use **Load demo case** to open deterministic synthetic scan geometry, or upload STL/PLY/OBJ for upper, lower and preparation scans.
2. Rotate/pan/zoom in the interactive viewer; toggle scans independently.
3. Add margin points in the viewer. Points are currently approximate manual inputs; clinical-grade raycasting/margin tracing is a next milestone.
4. Generate the baseline crown proposal. This uses an explicit parametric geometry path, **not a trained AI crown model**.
5. Inspect the 3D result and export STL.

## Limitations
- Model inference is not represented as working until trained weights, licences, dependencies, interface and accuracy are validated.
- Demo geometry is synthetic, not anatomical and only validates the app's software flow.
- Geometry metrics are engineering diagnostics, not clinical acceptance criteria.
- STL has no universal unit metadata. Confirm input units; defaults to millimetres.
- Production hardening still needed: authentication, durable object storage, background jobs, privacy controls, logging, dependency/model/data licensing review and dental-professional validation.

## API
- `GET /health`
- `GET /api/demo-case`
- `POST /api/scan` multipart fields: `file`, `role`, `units`
- `POST /api/crown/generate` JSON: `preparation_scan_id`, `margin_points`, `parameters`
- `GET /api/artifacts/{artifact_id}/download`

## Tests
```bash
cd backend && pytest -q
```

## Licensing
Initial project code is Apache-2.0. No third-party weights or anatomical assets are redistributed. Audit each dependency, model, dataset and asset separately before commercialization.
