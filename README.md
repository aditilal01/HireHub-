# HireHub

**AI-Powered Job Portal & Recruitment Platform**

HireHub is a full-stack job portal that connects candidates and recruiters. It combines a React + TypeScript frontend with a Spring Boot REST API, PostgreSQL persistence, JWT-based authentication, resume management, and optional AI-powered resume analysis and job matching.

## Repository structure

```text
HireHub/
├── backend/                 # Spring Boot 3.2.5 / Java 17 REST API
│   ├── src/main/
│   ├── src/test/
│   ├── Dockerfile
│   ├── pom.xml
│   └── README.md
├── frontend/                # React 18 / TypeScript / Vite application
│   ├── src/
│   ├── Dockerfile
│   ├── package.json
│   └── README.md
├── .github/workflows/       # Backend and frontend CI
├── .env.example             # Shared local environment template
└── docker-compose.yml       # PostgreSQL + backend + frontend
```

## Main capabilities

- Candidate and recruiter registration and login with JWT authentication.
- Recruiter job creation, editing, deactivation, and applicant management.
- Candidate job discovery, filtering, applications, and saved jobs.
- Candidate profiles with optional recruiter visibility.
- Candidate and recruiter dashboards.
- Resume upload and storage.
- Optional AI resume analysis and job matching using an OpenAI API key configured on the backend.
- API documentation through Swagger / OpenAPI.

AI-generated analysis and match scores are advisory and should not be treated as hiring decisions.

## Technology stack

| Layer | Technologies |
|---|---|
| Frontend | React 18, TypeScript, Vite, Tailwind CSS |
| UI and data | React Router, Axios, React Hook Form, Zod, Recharts, Lucide |
| Backend | Java 17, Spring Boot 3.2.5, Spring Security, Spring Data JPA |
| Database | PostgreSQL |
| Authentication | JWT |
| Resume processing | Apache Tika |
| AI integration | OpenAI API (optional) |
| API docs | Swagger / OpenAPI |
| Deployment | Docker, Docker Compose, GitHub Actions |

## Run with Docker Compose

### Prerequisites

- Docker Engine or Docker Desktop
- Docker Compose v2

### 1. Configure environment

From the repository root:

```bash
cp .env.example .env
```

Edit `.env` and set a strong database password and a random JWT secret. The OpenAI key is optional and should only be stored in this backend-side environment file. Never commit `.env` or expose the key in frontend code.

### 2. Build and start the stack

```bash
docker compose up --build -d
```

### 3. Open the applications

- Frontend: http://localhost:5173
- Backend API: http://localhost:8080
- Swagger UI: http://localhost:8080/swagger-ui/index.html

### 4. View logs and stop

```bash
docker compose ps
docker compose logs -f backend
docker compose down
```

To remove the database and uploaded-resume volumes as well, use `docker compose down -v`. This permanently deletes that local persisted data.

## Run locally without Docker

### Backend

Requirements: Java 17 and Maven.

```bash
cd backend
```

Configure PostgreSQL and the required environment variables (see `backend/.env.example`; Spring Boot does not automatically load that example file). Then run:

```bash
mvn spring-boot:run -s settings.xml
```

The API is available at http://localhost:8080.

### Frontend

Requirements: Node.js 20 or later and npm.

```bash
cd frontend
npm ci
```

Create a local `.env` file from `.env.example` if your API is not running at the default `http://localhost:8080`, then start:

```bash
npm run dev
```

Vite serves the frontend at http://localhost:5173 by default.

## Environment variables

The root `.env.example` is used by Docker Compose.

| Variable | Purpose |
|---|---|
| `DB_USERNAME`, `DB_PASSWORD` | PostgreSQL credentials |
| `JWT_SECRET` | Signing secret for JWTs; use a long random value |
| `JWT_EXPIRATION_MS` | Token lifetime in milliseconds |
| `SERVER_PORT` | Host port for the backend |
| `CORS_ORIGINS` | Comma-separated allowed frontend origins |
| `OPENAI_API_KEY` | Optional backend-only key for AI features |
| `OPENAI_MODEL` | OpenAI model name; defaults to `gpt-4o-mini` |

For backend-specific configuration, see [backend/README.md](backend/README.md). For frontend setup and UI details, see [frontend/README.md](frontend/README.md).

## CI

GitHub Actions contains separate workflows:

- **Backend CI** runs the Maven test suite when backend files change.
- **Frontend CI** installs locked npm dependencies and runs the production TypeScript/Vite build when frontend files change.

## Security and privacy notes

- Keep credentials, JWT secrets, and API keys out of source control.
- Configure production CORS origins explicitly.
- Use HTTPS and managed secrets in production.
- Review resume retention, access control, and user consent policies before deploying with real candidate data.
- Treat AI output as assistive information, not an automated employment decision.

## Current project status

This repository consolidates the existing backend and frontend codebases. The original repositories remain available as backups:

- [Original backend](https://github.com/aditilal01/job-portal-backend)
- [Original frontend](https://github.com/aditilal01/job-portal-frontend)

The monorepo migration itself does not imply that every end-to-end workflow has been runtime-tested. See the component READMEs and CI results for build and test details.
