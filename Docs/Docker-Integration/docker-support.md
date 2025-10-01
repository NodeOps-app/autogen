---
topic: Docker image import
---


## Image requirements

- Public
- Arch --platform linux/amd64 
> This constraint ensures imported Docker images are built for the host machine architecture.

<!-- 
Testing
image name: jbthechamp/ghost-blog 
  -p 8080:2368 \
  ghost:latest
👉 uses SQLite by default (so no external DB needed).

Admin panel normally available at http://localhost:8080/ghost, but endpoint should be provided -->

## Deploy a Docker image

<details>
  <summary>Walk the steps</summary>

1. Logged into the app, click on **New Project**.
2. Click on **Deploy Docker Container**.
3. Populate:
	- Image path
	- Port
	- (Optional) project name
4. Click on **Deploy Project**.

</details>