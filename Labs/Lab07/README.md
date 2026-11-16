<div align="center">

# 🐳 Lab 07: Docker Containers & Images

**Hands-on container engineering on Ubuntu Server**

![Docker](https://img.shields.io/badge/Docker-29.1.3-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Compose](https://img.shields.io/badge/Compose-v5.7.0-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![NGINX](https://img.shields.io/badge/NGINX-1.31.8-009639?style=for-the-badge&logo=nginx&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-10.10%20%7C%209.6-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-26.04%20LTS-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-Bash-FCC624?style=for-the-badge&logo=linux&logoColor=black)

</div>

---

## 👨‍💻 Author

**Kiran Siddique** · DevOps Track · `Labs/Lab07`

---

## 🎯 What This Lab Demonstrates

This lab covers the full container lifecycle, from installing Docker to building a custom image from a modified container. Every task was performed on a Linux server (Ubuntu in VMware), with access from both the VM console and a Windows host over SSH and the browser.

| # | Task | Outcome |
|:-:|------|---------|
| 1 | Docker & Compose setup | Verified Engine `29.1.3` and Compose `v5.7.0` |
| 2 | PostgreSQL containers | Ran `9.6` and `10.10` side by side, handled a missing-config error |
| 3 | Docker inspection & cleanup | Searched Docker Hub, managed container states, removed a container and its image |
| 4 | NGINX on multiple ports | Served two NGINX versions on `6001` and `6002`, verified via browser and logs |
| 5 | Container interaction & commit | Attached and detached safely, exec'd into a shell, tracked changes, saved `nginx:myimage` |

---

## 🛠️ Skills Applied

<table>
<tr>
<td width="50%" valign="top">

**🐧 Linux & Shell**
- Ubuntu server administration
- Bash commands and terminal workflow
- Checking network interfaces and IPs (`hostname -I`, `ip`)
- Managing user groups and permissions (`usermod`, `docker` group)

**🐳 Containerization**
- Docker Engine and Compose installation
- Running containers in detached and interactive modes
- Port publishing (`-p HOST:CONTAINER`)
- Container lifecycle: `run`, `ps`, `restart`, `stop`, `rm`
- Image management: `pull`, `images`, `rmi`, `commit`

</td>
<td width="50%" valign="top">

**🔍 Troubleshooting & Observability**
- Reading container logs and HTTP access logs
- Inspecting container state and configuration (`docker inspect`)
- Tracking filesystem changes (`docker diff`)
- Diagnosing startup failures (missing `POSTGRES_PASSWORD`)

**🌐 Services & Networking**
- NGINX web server deployment and verification
- PostgreSQL database deployment with version pinning
- Browser-based service validation across host and VM
- SSH access from Windows (PowerShell)

</td>
</tr>
</table>

---

## ⚡ Highlights

- 🔁 **Clean lifecycle management:** Stopped and removed a container, then untagged its image, handling a stopped container that blocked deletion.
- 🔀 **Side-by-side services:** Ran two NGINX versions and two PostgreSQL versions without port or name conflicts.
- 🧪 **Container forensics:** Used `docker diff` to confirm exactly which files were added inside a running container.
- 💾 **Custom image creation:** Turned a modified container into a reusable image with `docker commit`.

---

## 🧰 Tech Stack

`Docker` · `Docker Compose` · `NGINX` · `PostgreSQL` · `Ubuntu` · `Bash` · `Git` · `GitHub` · `VMware Workstation` · `SSH`

---

## 📂 Repository Structure

```
Labs/Lab07/
├── README.md
└── screenshots/          # 17 proof-of-work screenshots
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

## 🔑 Key Takeaways

> **Images are templates, containers are instances.** Changes inside a container stay there until you commit them into a new image.

> **Attach ≠ Exec.** `docker attach` joins the main process, while `docker exec` starts a new process such as a shell.

> **Detach safely with `Ctrl+P` `Ctrl+Q`** so the container keeps running.

> **Running vs. all:** `docker ps` shows active containers, `docker ps -a` shows everything, including stopped ones.

---

<div align="center">

**✅ Lab Status: Completed**

*Built with curiosity, tested with patience.* 🚀

</div>
