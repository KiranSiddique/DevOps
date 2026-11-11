# 🗄️ Lab 06: Set Up Nexus Repository and Configure User Access

> **Course:** DevOps (CS-649 Cloud DevOps) · **Instructor:** Sir Waqas Saleem · **Section:** A

---

## 👤 Student

| | |
|---|---|
| **Name** | Kiran Siddique |
| **Registration No.** | 2023-BCS-052 |
| **GitHub** | [@KiranSiddique](https://github.com/KiranSiddique) |
| **Submission Path** | `DevOps/Labs/Lab06` |
| **Submitted On** | 11 November 2026 |

---

## 🧭 Overview

Nexus Repository runs in Docker on the Ubuntu server. In this lab I set up the admin account, created a restricted user with a Maven snapshots role, published a Gradle JAR and a Maven JAR to `maven-snapshots`, queried the REST API, created a file blob store and a Docker hosted repository, and finally ran a cleanup policy followed by blob store compaction.

---

## 🖥️ Environment

| Component | Version / Detail |
|---|---|
| Server | Ubuntu Server 26.04.1 LTS (user `kiransiddique`, host `ubuntu`) |
| Docker Engine / Compose | 29.1.3 / v5.7.0 |
| Git | 2.53.0 |
| Nexus Repository | `sonatype/nexus3:latest`, Community Edition 3.98.1-01 |
| Nexus ports | `8006` (web UI and REST API), `8007` (Docker connector) |
| Maven | 3.9.12 |
| Gradle | 7.0 (Wrapper), run with JDK 16.0.2 |

---

## 🗂️ Submission Structure

```
DevOps/
└── Labs/
    └── Lab06/
        ├── README.md             ← this file
        ├── Lab6_Solution.pdf     ← solution report with captions
        └── screenshots/          ← 28 required PNG screenshots
```

---

## ✅ Progress

| # | Task | Screenshots | Status |
|:-:|---|:-:|:-:|
| 1 | Run Nexus Repository and Complete the Initial Setup | 1 | ✅ |
| 2 | Create a Role and User and Verify Access | 1 | ✅ |
| 3 | Build and Publish the Gradle Application to Nexus | 4 | ✅ |
| 4 | Publish the Maven Application to Nexus | 5 | ✅ |
| 5 | Inspect Nexus Repositories and Components Using the REST API | 4 | ✅ |
| 6 | Create a Nexus Blob Store and Docker Hosted Repository | 4 | ✅ |
| 7 | Create and Run a Maven Snapshot Cleanup Policy | 9 | ✅ |
| | **Total** | **28** | **28 / 28** |

---

## 📸 Screenshot Checklist

<details>
<summary><b>Task 1: Nexus Repository setup</b> (1)</summary>

- [x] `nexus_container_running.png`

</details>

<details>
<summary><b>Task 2: Role and user</b> (1)</summary>

- [x] `nexus_user_access.png`

</details>

<details>
<summary><b>Task 3: Gradle publish</b> (4)</summary>

- [x] `gradle_nexus_configuration.png`
- [x] `gradle_nexus_build.png`
- [x] `gradle_nexus_publish.png`
- [x] `nexus_gradle_artifact.png`

</details>

<details>
<summary><b>Task 4: Maven publish</b> (5)</summary>

- [x] `maven_nexus_project.png`
- [x] `maven_distribution_management.png`
- [x] `maven_package_nexus.png`
- [x] `maven_deploy_nexus.png`
- [x] `nexus_maven_artifact.png`

</details>

<details>
<summary><b>Task 5: REST API</b> (4)</summary>

- [x] `nexus_repositories_user.png`
- [x] `nexus_repositories_admin.png`
- [x] `nexus_components_list.png`
- [x] `nexus_component_details.png`

</details>

<details>
<summary><b>Task 6: Blob store and Docker hosted repository</b> (4)</summary>

- [x] `nexus_default_blob_content.png`
- [x] `nexus_mystore_created.png`
- [x] `nexus_blob_directories.png`
- [x] `nexus_docker_hosted_repository.png`

</details>

<details>
<summary><b>Task 7: Cleanup policy and compaction</b> (9)</summary>

- [x] `nexus_cleanup_policy_created.png`
- [x] `nexus_cleanup_policy_attached.png`
- [x] `nexus_cleanup_task_waiting.png`
- [x] `nexus_cleanup_task_completed.png`
- [x] `nexus_maven_snapshots_empty.png`
- [x] `nexus_blob_store_before_compaction.png`
- [x] `nexus_compact_task_configured.png`
- [x] `nexus_compact_task_completed.png`
- [x] `nexus_blob_store_after_compaction.png`

</details>

---

## 🛠️ What Was Done

<details>
<summary><b>Task 1:</b> Run Nexus in Docker and finish setup</summary>

Nexus runs as a container named `nexus` with the persistent named volume `nexus-data`. Port `8006` serves the web UI and REST API, and port `8007` is published for the Docker connector used in Task 6.

```bash
docker run -d --name nexus --restart unless-stopped \
  -p 8006:8081 -p 8007:8007 -v nexus-data:/nexus-data \
  sonatype/nexus3:latest
docker start nexus            # used once the container already existed
docker logs -f nexus
```

The setup wizard was completed, the generated admin password was replaced, and anonymous access was disabled. An unauthenticated request to `/service/rest/v1/repositories` returns `401 Unauthorized`.

</details>

<details>
<summary><b>Task 2:</b> Role and user</summary>

- **Role:** `nx-java`, with the privilege `nx-repository-view-maven2-maven-snapshots-*`
- **User:** `nexus`, Active, with the `nx-java` role granted

```bash
curl -i -u nexus http://<server>:8006/service/rest/v1/status
# HTTP/1.1 200 OK
```

The password was typed at curl's prompt and never placed on the command line.

</details>

<details>
<summary><b>Task 3:</b> Gradle publish to Nexus</summary>

The `java-app` project from Lab 5 was used. Gradle 7.0 does not run on Java 21, so the project and Gradle JVM were set to JDK 16.0.2.

- `build.gradle`: added `maven-publish`, a publication for `build/libs/my-app-$version.jar`, and the `maven-snapshots` repository over HTTP with `allowInsecureProtocol = true`.
- `settings.gradle`: `rootProject.name = 'my-app'`.
- `gradle.properties`: holds `repoUser` and `repoPassword`, created with mode `600`. Its contents are not recorded here.

```bash
./gradlew --version            # JVM: 16.0.2
./gradlew clean build          # BUILD SUCCESSFUL
./gradlew clean build publish  # BUILD SUCCESSFUL
```

Result: `com/example/my-app/1.0-SNAPSHOT/my-app-1.0-20261111.092824-1.jar`, with POM, checksums and metadata.

</details>

<details>
<summary><b>Task 4:</b> Maven deploy to Nexus</summary>

- `pom.xml`: added `<distributionManagement>` with the snapshot repository ID `nexus-snapshots`, right after `<version>`.
- `~/.m2/settings.xml`: holds the matching `nexus-snapshots` server with the `nexus` user's credentials, created with mode `600`.

```bash
mvn package     # BUILD SUCCESS, creates target/java-maven-app-1.1.0-SNAPSHOT.jar
mvn deploy      # BUILD SUCCESS, uploads to nexus-snapshots
```

Result: `com/example/java-maven-app/1.1.0-SNAPSHOT/java-maven-app-1.1.0-20261111.094359-1.jar`, with POM and metadata.

</details>

<details>
<summary><b>Task 5:</b> REST API queries</summary>

Each request used `-u nexus` or `-u admin`, so curl asked for the password. I compared the repository list for the restricted user with the admin's list, then listed the components in `maven-snapshots` and fetched one component by ID.

```bash
curl -u nexus -X GET "…/service/rest/v1/repositories"
curl -u admin -X GET "…/service/rest/v1/repositories"
curl -u nexus -X GET "…/service/rest/v1/components?repository=maven-snapshots"
curl -u nexus -X GET "…/service/rest/v1/components/<component-id>"
```

</details>

<details>
<summary><b>Task 6:</b> Blob store and Docker hosted repository</summary>

- Inspected the default blob store inside the container without editing it. `blobs/default/content` contains `2026`, `directpath` and `tmp`.
- Created the File blob store **`mystore`** at `/nexus-data/blobs/mystore`. Its directory contains `content` and `metadata.properties`.
- Created the Docker hosted repository **`docker-hosted`**: Online, HTTP connector `8007`, blob store `mystore`, strict content type validation enabled.

```bash
sudo docker exec -it nexus /bin/sh
cd /opt/sonatype/sonatype-work/nexus3/blobs
ls
ls default/content
ls mystore
exit
```

</details>

<details>
<summary><b>Task 7:</b> Cleanup policy, manual run, and compaction</summary>

1. Created the policy **`maven`** (format `maven2`, Pre-Releases/Snapshots, asset matcher `.*`), previewed it against `maven-snapshots`, and saved it.
2. Attached the policy to `maven-snapshots`.
3. Checked the Cleanup service (Waiting, next run 6:00 AM), then used **Run Now**. Last result: **OK**.
4. `maven-snapshots` then showed **No items in this repository**.
5. Recorded the default blob store values before compaction.
6. Created the task **`compact-store`** for `default` (0 days, Daily at 06:30) and ran it. Last result: **OK**.
7. Recorded the default blob store values after compaction.

> ⚠️ Cleanup removes snapshot components from the repository. I ran it only after all Gradle and Maven publish screenshots were saved.

</details>

---

## 🧩 Challenges and Fixes

| Problem | Fix |
|---|---|
| `docker pull sonatype/nexus3` stayed at `Pulling fs layer` | DNS resolved fine. I forced IPv4 with `curl -4`, then ran the pull in a retry loop (with `nohup` and a log file). It completed on the next retry. |
| `TLS handshake timeout` and `connection reset` during pulls | Retries and the log file kept progress; completed layers were not downloaded again. |
| `docker run` failed with *name already in use* | The `nexus` container already existed, so I used `docker start nexus`. |
| `permission denied ... docker.sock` | Ran the Docker commands with `sudo`. |
| Memory was tight (about 1 GB free, swap in use) | Raised the VM memory to 6 GB before starting Nexus. |
| Password prompt looked like a hang | Typed the password at curl's prompt. It was never placed on the command line. |
| Credential files | `gradle.properties` and `~/.m2/settings.xml` were created with mode `600`, and their contents are not shown. |

---

## 🏁 Summary

I ran Nexus Repository in Docker with persistent storage and completed its setup. I disabled anonymous access, created the `nx-java` role and the `nexus` user, and checked the user through the Status API. I published a Gradle snapshot and a Maven snapshot, and verified both in the browser and through the REST API. I created the `mystore` blob store and the `docker-hosted` repository on port 8007. Finally I ran the cleanup policy, emptied `maven-snapshots`, and compacted the default blob store.
