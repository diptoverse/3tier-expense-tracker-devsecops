# 3-Tier Expense Tracker with a DevSecOps Pipeline

A 3-tier expense tracker built with React, Node.js and MongoDB, shipped to AWS
through a fully automated CI/CD pipeline. Every change is tested, security-scanned
and deployed without manual steps.

The app is kept simple on purpose. The focus is the delivery pipeline: automated
quality checks, security gates that block vulnerable code, immutable Docker images
and zero-touch deployment to the cloud.

**Status:** in active development. See the roadmap below for progress.

## Architecture

The app has three tiers, each running in its own container:

```
Browser → React + Nginx → Express API → MongoDB
          (public)        (private)     (private)
```

Only the frontend is reachable from the internet. The API and the database
stay on a private Docker network.

## How the pipeline works

Every push to `main` goes through these steps:

1. Lint and unit tests
2. Security scan of the code and dependencies (Trivy)
3. Build Docker images
4. Security scan of the images
5. Push the images to Docker Hub
6. Deploy to AWS EC2 and run a health check

If any step fails, the pipeline stops and nothing gets deployed.

## Tech stack

- **Frontend:** React, Vite, Nginx
- **Backend:** Node.js, Express
- **Database:** MongoDB
- **DevOps:** Docker, Docker Compose, GitHub Actions, Trivy, Docker Hub, AWS EC2
- **Planned:** Terraform, GitHub OIDC

## Roadmap

- [x] Project setup
- [x] Backend API
- [ ] Frontend
- [ ] Docker setup
- [ ] CI with GitHub Actions
- [ ] Security scanning with Trivy
- [ ] Push images to Docker Hub
- [ ] Deploy to AWS EC2
- [ ] Production improvements (Terraform, OIDC, rollback, non-root containers)
- [ ] Final documentation

## Run the backend locally

Requires Node.js 22 and Docker.

```bash
docker run -d --name expense-mongo-dev -p 127.0.0.1:27017:27017 -v expense-mongo-dev-data:/data/db mongo:7
cd backend
npm install
npm run dev
```

The API runs on `http://localhost:5000`.

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/health` | Service and database status |
| GET | `/api/expenses` | List expenses (filters: `search`, `category`, `from`, `to`) |
| GET | `/api/expenses/summary` | Totals and category breakdown |
| POST | `/api/expenses` | Create an expense |
| PUT | `/api/expenses/:id` | Update an expense |
| DELETE | `/api/expenses/:id` | Delete an expense |
| POST | `/api/expenses/seed` | Load demo data into an empty database |

Run lint and tests:

```bash
npm run lint
npm test
```

> On macOS, port 5000 is used by AirPlay Receiver. Turn it off in System Settings → General → AirDrop & Handoff, or run with `PORT=5001 npm run dev`.


## Author

Dipto — [GitHub](https://github.com/diptoverse)
