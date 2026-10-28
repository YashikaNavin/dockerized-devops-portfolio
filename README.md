# Dockerized DevOps Portfolio Website

## Overview

A static HTML portfolio website served by Nginx inside a Docker container. This hands-on project demonstrates Docker image building, container networking, non-root execution, Linux capability restrictions, and read-only filesystem hardening.

## Technologies Used

- Linux
- Docker
- Dockerfile
- Nginx
- HTML and CSS
- Shell commands

## Project Structure

    docker-web-project/
    ├── Dockerfile
    ├── index.html
    └── README.md

## Features

- Custom HTML and CSS website served by Nginx.
- Custom Docker image tagged as `devops-portfolio:v2`.
- Nginx configured to listen on container port 8080.
- Non-root execution using the `nginx` user.
- Linux capabilities dropped using `--cap-drop=ALL`.
- Read-only root filesystem with writable temporary filesystems for Nginx runtime requirements.

## Build the Image

    docker build -t devops-portfolio:v2 .

## Run the Standard Container

    docker run -d \
      --name devops-portfolio-standard \
      -p 8086:8080 \
      devops-portfolio:v2

Open http://localhost:8086 in your browser.

## Run the Hardened Container

    docker run -d \
      --name devops-portfolio-hardened \
      --cap-drop=ALL \
      --read-only \
      --tmpfs /tmp:rw,noexec,nosuid,size=16m \
      --tmpfs /var/cache/nginx:rw,noexec,nosuid,size=16m,uid=101,gid=101,mode=755 \
      --tmpfs /run:rw,noexec,nosuid,size=4m,uid=101,gid=101,mode=755 \
      -p 8088:8080 \
      devops-portfolio:v2

Open http://localhost:8088 in your browser.

## Validation and Testing

- Verified HTTP status 200 for the website.
- Confirmed Nginx master and worker processes run as the non-root `nginx` user.
- Verified Docker reports all Linux capabilities dropped for the hardened container.
- Confirmed the hardened container uses a read-only root filesystem.
- Verified writing to the root filesystem fails as expected.
- Confirmed Nginx starts with writable temporary filesystems mounted at the required paths.

## Key Learnings

- Docker image building, tagging, and layer caching.
- Container port publishing.
- Running containers as non-root users.
- Restricting Linux capabilities.
- Read-only root filesystems and temporary mounts.
- Troubleshooting Nginx startup and filesystem permissions.

## Project Type

Hands-on learning project built to practise Docker containerization and security hardening.
