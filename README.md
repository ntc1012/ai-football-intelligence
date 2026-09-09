# AI Football Intelligence Platform

Personal production-grade platform for football video analysis: detect players and the ball, track identities across frames, reconstruct pitch geometry, and turn raw match footage into tactical insight.

## Vision

Build a private intelligence layer over football matches. Input is video (and later event data). Output is structured understanding: who is where, how the team is organized, how the ball moves, and what that means tactically. The long-term product is a web workspace for reviewing matches with AI-generated analysis, not a one-off notebook.

## Core features (target)

- Player and referee detection in broadcast or tactical camera footage
- Multi-object tracking (identities across frames)
- Ball detection and trajectory estimation
- Pitch registration and homography (video coordinates → field coordinates)
- Team, formation, and spatial analytics
- Tactical summaries and match-review views
- Job-based processing pipeline (upload → workers → stored results)
- Web UI for matches, clips, overlays, and reports

## High-level architecture

```
Video / data ingest
        │
        ▼
   workers (async jobs)
        │
        ▼
   ai/*  (detection → tracking → ball → pitch → analytics → tactical)
        │
        ▼
   PostgreSQL + object storage     Redis (queues / cache)
        │
        ├── api (FastAPI)
        └── web (Next.js)
```

- **apps/web** — operator and analyst UI
- **apps/api** — HTTP API, auth, match metadata, job control
- **ai/** — computer vision and football intelligence modules
- **workers/** — background processing of videos and model inference
- **packages/** — shared libraries used by api, workers, and AI
- **infrastructure/** — deployment, networking, and environment definitions

## Initial technology stack

| Layer | Technology |
| --- | --- |
| Language | Python |
| Computer vision | OpenCV |
| Deep learning | PyTorch, YOLO |
| Tracking | ByteTrack or BoT-SORT |
| API | FastAPI |
| Frontend | Next.js |
| Database | PostgreSQL |
| Queue / cache | Redis |
| Runtime | Docker |

Secrets and local config live in `.env` (see `.env.example`). Do not commit real keys.

## Development roadmap

1. **Foundation** — this repository layout, local Postgres/Redis via Docker, environment conventions
2. **Ingest** — video upload, storage, and job records
3. **Perception** — detection, tracking, ball, and pitch modules with evaluation on annotated clips
4. **Intelligence** — analytics and tactical layers on top of tracks and pitch coordinates
5. **Product surface** — FastAPI + Next.js match review, overlays, and reports
6. **Hardening** — tests, model versioning, observability, and production deployment

Application code, model training, and dependency installation are intentionally not started in this commit.

## Local infrastructure (later)

When you are ready to run services:

```bash
cp .env.example .env
docker compose up -d
```
