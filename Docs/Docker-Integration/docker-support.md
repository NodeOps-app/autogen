---
topic: Docker image import
---

## Image requirements

- Public
- Arch --platform linux/amd64 
> This constraint ensures imported Docker images are built for the host machine architecture.


## Prerequisites

- Docker image fitting the requirements
- [AutoGen account](https://autogen.nodeops.network/)

## Deploy a Docker image

<!-- ![nginx example](../../Static/Gifs/docker-boilerplate.gif) app has changed, this is now out of date, missing the environment vars -->

1. Logged into the [AutoGen app](https://autogen.nodeops.network/), click **+ New Project**. 
2. Click on **Deploy Docker Container**.
3. Populate:
	- Image path
	- Port
	- (Optional) project name
4. Click on **Deploy Project**.
5. Click **Visit Project** to access the app on its public endpoint.

> Lightweight images will be available immediately.
> View the logs to verify larger deployments before hitting that unique endpoint.



