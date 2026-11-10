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
- [ ] Backend API
- [ ] Frontend
- [ ] Docker setup
- [ ] CI with GitHub Actions
- [ ] Security scanning with Trivy
- [ ] Push images to Docker Hub
- [ ] Deploy to AWS EC2
- [ ] Production improvements (Terraform, OIDC, rollback, non-root containers)
- [ ] Final documentation

## Author

Dipto — [GitHub](https://github.com/diptoverse)
