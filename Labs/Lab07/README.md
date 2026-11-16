# Lab 07: Docker Containers and Images

![Docker](https://img.shields.io/badge/Docker-Containers-2496ED?logo=docker&logoColor=white)
![NGINX](https://img.shields.io/badge/NGINX-Web%20Server-009639?logo=nginx&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Database-4169E1?logo=postgresql&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-Server-E95420?logo=ubuntu&logoColor=white)

> **Course:** DevOps · **Submission path:** `Labs/Lab07/` · **Environment:** Ubuntu Server (VMware)

This lab introduces Docker fundamentals on an Ubuntu server. It covers installing Docker and Docker Compose, running PostgreSQL and NGINX containers, managing containers and images, and creating a custom image from a modified container.

---

## Table of Contents

- [Objectives](#objectives)
- [Environment](#environment)
- [Tasks Overview](#tasks-overview)
- [Key Commands](#key-commands)
- [Screenshot Evidence](#screenshot-evidence)
- [Repository Structure](#repository-structure)
- [Key Learnings](#key-learnings)

---

## Objectives

By the end of this lab, I was able to:

- Install and verify Docker Engine and the Docker Compose plug-in.
- Run PostgreSQL 9.6 and 10.10 containers in the background and identify them with `docker ps`.
- Inspect the Docker installation, search Docker Hub, and manage running and stopped containers.
- Run two NGINX containers on different host ports and read their logs.
- Attach to a container's main process, open a secondary shell, track filesystem changes, and save a new image with `docker commit`.

---

## Environment

| Item | Details |
|------|---------|
| Operating System | Ubuntu 26.04.1 LTS |
| Virtualization | VMware Workstation |
| Docker Engine | 29.1.3 |
| Docker Compose | v5.7.0 |
| Host Access | Browser on Windows host, terminal via VM console and PowerShell SSH |

---

## Tasks Overview

| Task | Description | Status |
|------|-------------|:------:|
| 1 | Install Docker and Docker Compose, verify versions | ✅ |
| 2 | Run PostgreSQL 9.6 (missing-password check) and PostgreSQL 10.10 | ✅ |
| 3 | Inspect Docker, search Docker Hub, manage and clean up containers and images | ✅ |
| 4 | Run two NGINX containers on ports 6001 and 6002, verify pages and logs | ✅ |
| 5 | Attach and detach, exec into a container, inspect changes, commit `nginx:myimage` | ✅ |

---

## Key Commands

```bash
# Task 1: Versions
docker --version
docker compose version

# Task 2: PostgreSQL
docker run -d -e POSTGRES_PASSWORD=password postgres:9.6
docker run -d -e POSTGRES_PASSWORD=password postgres:10.10

# Task 3: Inspect and cleanup
docker info
docker search ubuntu
docker run -d ubuntu
docker ps -a
docker rm <container>
docker rmi postgres:9.6

# Task 4: NGINX on separate ports
docker run -d -it -p 6001:80 --name nginx-latest nginx
docker run -d -p 6002:80 --name nginx-older nginx:1.30.6
docker logs nginx-latest

# Task 5: Work inside a container and save an image
docker attach nginx-latest          # detach with Ctrl+P, Ctrl+Q
docker exec -it nginx-latest /bin/sh
docker diff nginx-latest
docker inspect nginx-latest
docker commit nginx-latest nginx:myimage
```

---

## Screenshot Evidence

All screenshots are stored in [`screenshots/`](./screenshots/).

### Task 1: Installation

| File | Shows |
|------|-------|
| `docker_installation.png` | `docker --version` and `docker compose version` output |

### Task 2: PostgreSQL

| File | Shows |
|------|-------|
| `postgres_missing_password.png` | Missing `POSTGRES_PASSWORD` error on `docker run postgres:9.6` |
| `postgres_96_running.png` | `docker ps` with the running `postgres:9.6` container |
| `postgres_both_versions.png` | `docker ps` with `postgres:9.6` and `postgres:10.10` running |

### Task 3: Docker Inspection and Cleanup

| File | Shows |
|------|-------|
| `docker_info.png` | `docker info` server details |
| `docker_search_ubuntu.png` | `docker search ubuntu` with the official image |
| `ubuntu_container_status.png` | Ubuntu container exited, visible in `docker ps -a` |
| `postgres_96_cleanup.png` | Restart, stop, remove, and image untag of `postgres:9.6` |

### Task 4: NGINX Containers

| File | Shows |
|------|-------|
| `nginx_containers_started.png` | Both `docker run` commands and returned container IDs |
| `nginx_latest_page.png` | NGINX welcome page on port 6001 |
| `nginx_older_page.png` | NGINX welcome page on port 6002 |
| `nginx_latest_logs.png` | `docker logs nginx-latest` with the browser request |

### Task 5: Container Interaction and Image Commit

| File | Shows |
|------|-------|
| `nginx_attach_detach.png` | Attached output and detach to the Ubuntu prompt |
| `nginx_exec_shell.png` | `docker exec` session and commands run inside the container |
| `nginx_diff.png` | `docker diff` showing `newfile.html` added |
| `nginx_inspect.png` | `docker inspect` showing the container in `running` state |
| `nginx_commit.png` | `docker commit` creating `nginx:myimage` |

---

## Repository Structure

```
Labs/Lab07/
├── README.md
└── screenshots/
    ├── docker_installation.png
    ├── postgres_missing_password.png
    ├── postgres_96_running.png
    ├── postgres_both_versions.png
    ├── docker_info.png
    ├── docker_search_ubuntu.png
    ├── ubuntu_container_status.png
    ├── postgres_96_cleanup.png
    ├── nginx_containers_started.png
    ├── nginx_latest_page.png
    ├── nginx_older_page.png
    ├── nginx_latest_logs.png
    ├── nginx_attach_detach.png
    ├── nginx_exec_shell.png
    ├── nginx_diff.png
    ├── nginx_inspect.png
    └── nginx_commit.png
```

---

## Key Learnings

- **Images vs. containers:** An image is a read-only template. A container is a running (or stopped) instance of that image.
- **Required configuration:** The official PostgreSQL image refuses to start without `POSTGRES_PASSWORD`.
- **Running vs. all containers:** `docker ps` lists only running containers. `docker ps -a` also shows stopped ones.
- **Stopped containers block image removal:** A container that references an image must be removed before `docker rmi` can delete it.
- **Port publishing:** `-p HOST:CONTAINER` maps a host port to a container port, so two NGINX containers can run side by side.
- **Attach vs. exec:** `docker attach` connects to the main process. `docker exec` starts a new process, such as a shell.
- **Detaching safely:** `Ctrl+P` then `Ctrl+Q` leaves a container running after you detach.
- **Filesystem tracking:** `docker diff` shows files added (`A`), changed (`C`), or deleted (`D`) compared with the image.
- **Committing changes:** `docker commit` saves a container's current filesystem as a new image.

---

**Author:** Kiran Siddique
**Lab Status:** Completed
