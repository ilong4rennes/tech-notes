## 2024-NodeBB — CMU Software Engineering Course

Open-source contribution project on the NodeBB forum platform. Each phase covers a different engineering practice.

| Name | Link | Summary |
| ---- | ---- | ------- |
| Codebase Setup | [[2024-NodeBB/01-Codebase-Setup]] | Fork, clone, local install (Redis), linting, testing, SonarCloud, GitHub workflow |
| Feature Implementation | [[2024-NodeBB/02-Feature-Implementation]] | NodeBB architecture overview; "Mark Resolved" feature; project structure breakdown |
| Continuous Deployment | [[2024-NodeBB/03-Continuous-Deployment]] | GitHub Actions workflows; Azure Web App deployment; YAML configuration |
| JSHint Integration | [[2024-NodeBB/03-JSHint-Integration]] | JSHint setup locally and in CI; `.jshintrc` config; npm script + Actions |
| LLM Integration | [[2024-NodeBB/04-LLM-Integration]] | Azure OpenAI GPT-4o-Mini; language detection and translation microservice (Flask) |

---

## 2025-CV-Model-Simulator — Computer Vision Web App

Full-stack web application for uploading and running computer vision models (YOLO/Ultralytics). Frontend in Next.js, backend in Flask, storage via LocalStack/S3.

| Name | Link | Summary |
| ---- | ---- | ------- |
| Design Doc | [[2025-CV-Model-Simulator/01-Design-Doc]] | User flows, component tree, API endpoints, database schema |
| Frontend | [[2025-CV-Model-Simulator/02-Frontend]] | Next.js + Redux; model upload, file size limits, run inference UI |
| Backend | [[2025-CV-Model-Simulator/03-Backend]] | Flask blueprints; REST API v1 specs for model management and inference |
| LocalStack | [[2025-CV-Model-Simulator/04-LocalStack]] | S3 integration; Docker deployment options; model caching strategy |
| Debug: Docker Networking | [[2025-CV-Model-Simulator/05-Debug-Docker-Networking]] | Flask → LocalStack connection refused; root cause: separate networks; fix via docker-compose |
| Deployment | [[2025-CV-Model-Simulator/06-Deployment]] | Infrastructure planning |

---

## Reference

Standalone technical notes not tied to a specific project.

| Name | Link | Summary |
| ---- | ---- | ------- |
| Syllabi Feature Analysis | [[Reference/Syllabi-Feature-Analysis]] | Full-stack deep dive: Express routes, Prisma ORM, React Query, batching (batshit), caching |
| Download Embedded Video | [[Reference/Guide-Download-Embedded-Video]] | DevTools Network tab; wget/curl; ffmpeg for HLS (.m3u8) stream stitching |
