---
topic: Docker image import
---


## Image requirements

- Public
- Arch --platform linux/amd64 
> This constraint ensures imported Docker images are built for the host machine architecture.

<!-- 
Testing
image name: ghost-blog 
  -p 8080:2368 \
  ghost:latest
👉 uses SQLite by default (so no external DB needed).

Admin panel normally available at http://localhost:8080/ghost, but endpoint should be provided -->