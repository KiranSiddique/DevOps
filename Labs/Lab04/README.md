<div align="center">

# Lab 04: Simple Python Database Connections

![Course](https://img.shields.io/badge/Course-DevOps%20CS--649-blue)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Python](https://img.shields.io/badge/Python-3.14-3776AB?logo=python&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-29.1.3-2496ED?logo=docker&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8.4-4479A1?logo=mysql&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-7.0-47A248?logo=mongodb&logoColor=white)

**Student:** Kiran Siddique &nbsp;|&nbsp; **Registration No.:** 2023-BCS-052 &nbsp;|&nbsp; **GitHub:** [@KiranSiddique](https://github.com/KiranSiddique)

</div>

---

## Table of Contents

- [Overview](#overview)
- [Learning Objectives](#learning-objectives)
- [Environment](#environment)
- [Task Progress](#task-progress)
- [Task Details](#task-details)
- [Exam Questions](#exam-questions)
- [Known Issues and Changes](#known-issues-and-changes)
- [Repository Structure](#repository-structure)
- [Key Takeaways](#key-takeaways)

---

## Overview

This lab connects Python to **MySQL** and **MongoDB** running in Docker. The same `db_connect.py` file was edited through four stages:

| Stage | Source of connection settings | Task |
|---|---|---|
| 1 | Values written in code (MySQL) | Task 4 |
| 2 | Values written in code (MongoDB) | Task 5 |
| 3 | Environment variables via `os.getenv()` | Task 6 |
| 4 | INI file via `configparser` | Task 7 |

Each program only connects, sends a request, and prints which database answered.

---

## Learning Objectives

- Explain the username, password, endpoint and database name used for a connection.
- Connect to MySQL and MongoDB with a short Python program.
- Explain the settings embedded in a MongoDB URI.
- Use `os.getenv()` to run unchanged code against dev, test and simulated prod.
- Use `configparser` to read settings from an INI credentials file.
- Confirm the target by reading the environment value stored in the database.

---

## Environment

| Component | Version / Detail |
|---|---|
| Host | Ubuntu Server (hostname `ubuntu`, user `kiransiddique`) |
| Git | 2.53.0 |
| Docker Engine | 29.1.3 |
| Docker Compose | v5.6.1 |
| Python | 3.14 (virtual environment `.venv`) |
| MySQL | 8.4 |
| MongoDB | 7.0 (see [Known Issues](#known-issues-and-changes)) |
| Python packages | `pymongo 4.19.0`, `mysql-connector-python 9.7.0`, `dnspython 2.9.0` |

---

## Task Progress

| # | Task | Status | Evidence |
|:-:|---|:-:|---|
| 1 | Fork and clone the Databases repository | ✅ | `databases_fork.png`, `databases_clone.png` |
| 2 | Install Docker, Compose and Python | ✅ | `docker_installation.png` |
| 3 | Start the database services | ✅ | `database_services.png`, `database_ports.png` |
| 4 | MySQL with values in the code | ✅ | `mysql_hardcoded_connection.png` |
| 5 | MongoDB with values in the code | ✅ | `mongo_hardcoded_connection.png` |
| 6 | MongoDB with `os.getenv()` | ✅ | `mongo_env_dev.png`, `mongo_env_test.png`, `mongo_env_prod.png`, `Q1_missing_environment.png` |
| 7 | MongoDB with a credentials file | ✅ | `credential_files_created.png`, `credential_file_permissions.png`, `mongo_file_dev.png`, `mongo_file_test.png`, `mongo_file_prod.png` |
| Q1 | Missing environment variable | ✅ | `Q1_missing_environment.png` |
| Q2 | Which source the file version reads | ✅ | `Q2_file_source_selection.png` |
| Q3 | Wrong password in the file | ✅ | `Q3_wrong_file_password.png` |

---

## Task Details

<details>
<summary><b>Task 1: Fork and Clone the Databases Repository</b></summary>

Forked `WaqasSaleem97/Databases` into my own account, then cloned it over SSH.

```bash
cd ~
git clone git@github.com:KiranSiddique/Databases.git
cd Databases
ls
```

**Result:** `db_connect.py`, `compose.yaml`, `requirements.txt` and `db-init/` are present.

</details>

<details>
<summary><b>Task 2: Install Docker, Docker Compose and Python</b></summary>

Installed `docker.io`, the Compose plugin, and Python with a virtual environment. Verified all tools after re-login and confirmed the `docker` group membership.

```bash
git --version
docker --version
docker compose version
groups
```

**Result:** Git 2.53.0, Docker 29.1.3, Compose v5.6.1, and `docker` appears in `groups`.

</details>

<details>
<summary><b>Task 3: Start the Database Services</b></summary>

Started the four services with `docker compose up` and checked health and ports.

```bash
docker compose ps -a
docker compose port mysql 3306
docker compose port mongo-dev 27017
docker compose port mongo-test 27017
docker compose port mongo-prod 27017
```

**Result:**

| Service | Host endpoint |
|---|---|
| MySQL | 127.0.0.1:3307 |
| MongoDB dev | 127.0.0.1:27017 |
| MongoDB test | 127.0.0.1:27018 |
| MongoDB prod (simulated) | 127.0.0.1:27019 |

All four services reported `(healthy)`. The MongoDB containers were started one at a time because the VM has 1.6 GB of RAM.

</details>

<details>
<summary><b>Task 4: MySQL with Values in the Code</b></summary>

Connected to MySQL with host, port, user, password and database written directly in the code.

```bash
python db_connect.py
```

**Result:**

```
Connected to MySQL: school_mysql
Server: 127.0.0.1 3307
Environment: dev
```

</details>

<details>
<summary><b>Task 5: MongoDB with Values in the Code</b></summary>

Built the URI by joining username, password, endpoint and database name, then sent a `ping` command to verify the connection.

```bash
python db_connect.py
```

**Result:**

```
Connected to MongoDB: school_dev
Server: ('127.0.0.1', 27017)
Environment: dev
```

</details>

<details>
<summary><b>Task 6: MongoDB with <code>os.getenv()</code></b></summary>

The same code read the four values from environment variables. Each profile was run in the same shell after exporting its values. The terminal was cleared before capturing screenshots so no secrets appeared.

| Profile | Database | Port | Environment |
|---|---|---|---|
| dev | `school_dev` | 27017 | dev |
| test | `school_test` | 27018 | test |
| prod (simulated) | `school_prod` | 27019 | prod |

</details>

<details>
<summary><b>Task 7: MongoDB with a Credentials File</b></summary>

Created `credentials.ini` from the example file, restricted it to the owner with `chmod 600`, and switched `db_connect.py` to read the section selected by `APP_ENV`.

```bash
cp -n credentials.ini.example credentials.ini
chmod 600 credentials.ini
stat -c '%a %n' credentials.ini
export APP_ENV=dev   # then test, then prod
python db_connect.py
```

**Result:** `600 credentials.ini`. Dev, test and prod all connected to the matching database and port.

`credentials.ini` is listed in `.gitignore` and was not committed.

</details>

<details>
<summary><b>Q1, Q2, Q3: Exam Questions</b></summary>

**Q1 (Missing environment variable):** With `MONGO_DATABASE` unset, `os.getenv` returns `None`. The `if not ...` check becomes true, so `SystemExit` stops the program before it contacts MongoDB.

**Q2 (Which source the file version reads):** With `APP_ENV` unset, `os.getenv("APP_ENV", "dev")` falls back to `dev`, so the `[dev]` section of `credentials.ini` is used. The shell's `MONGO_*` values are ignored by the file version.

**Q3 (Wrong password in the file):** With the right endpoint but `password = wrong`, MongoDB rejected the login with `Authentication failed` (code 18). The endpoint only locates the server; the password is checked against the user.

</details>

---

## Known Issues and Changes

> **MongoDB image changed from `8.0` to `7.0`** (approved by the instructor)

- **Problem:** On a host running Linux kernel `7.0.0-38-generic`, the `mongo:8.0` containers exited immediately with `MongoDB cannot start: Linux kernel versions 6.19 and newer has a known incompatibility with this version of MongoDB` (SERVER-121912).
- **Tests:** `mongo:8.0` failed with the same error, and `mongo:7.0` started and stayed healthy.
- **Change:** The three MongoDB `image:` lines in `compose.yaml` were changed from `mongo:8.0` to `mongo:7.0`. No other compose settings were changed.
- **Memory:** The VM has 1.6 GB of RAM. Containers were started one at a time to avoid out-of-memory kills (exit code 137).

---

## Repository Structure

```text
DevOps/
└── Labs/
    └── Lab04/
        ├── README.md
        ├── Lab4_Solution.pdf
        └── screenshots/
            ├── databases_fork.png
            ├── databases_clone.png
            ├── docker_installation.png
            ├── database_services.png
            ├── database_ports.png
            ├── mysql_hardcoded_connection.png
            ├── mongo_hardcoded_connection.png
            ├── mongo_env_dev.png
            ├── mongo_env_test.png
            ├── mongo_env_prod.png
            ├── credential_files_created.png
            ├── credential_file_permissions.png
            ├── mongo_file_dev.png
            ├── mongo_file_test.png
            ├── mongo_file_prod.png
            ├── Q1_missing_environment.png
            ├── Q2_file_source_selection.png
            └── Q3_wrong_file_password.png
```

**Fork:** [KiranSiddique/Databases](https://github.com/KiranSiddique/Databases)

---

## Key Takeaways

1. The connection block stays the same. Only the source of its settings changes.
2. `os.getenv()` lets one program run against several targets without edits.
3. An INI file keeps settings out of the code, and `interpolation=None` treats values literally.
4. A correct endpoint is not enough. Credentials must match too.
5. Always confirm the target by reading the stored environment value, not just the connection message.

---

<div align="center">

Lab 04 completed on Ubuntu Server · Submitted to `DevOps/Labs/Lab04`

</div>
