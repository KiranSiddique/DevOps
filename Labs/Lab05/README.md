# 🧪 Lab 05: Build, Test, and Package Applications with IntelliJ IDEA and Docker

> **Course:** DevOps (CS-649 Cloud DevOps) · **Instructor:** Sir Waqas Saleem · **Section:** A

---

## 👤 Student

| | |
|---|---|
| **Name** | Kiran Siddique |
| **Registration No.** | 2023-BCS-052 |
| **GitHub** | [@KiranSiddique](https://github.com/KiranSiddique) |
| **Submission Path** | `DevOps/Labs/Lab05` |
| **Submitted On** | 10 November 2026 |

---

## 🖥️ Lab Environment

| Component | Version / Detail |
|---|---|
| OS | Ubuntu Server 26.04.1 LTS (user `kiransiddique`, host `ubuntu`) |
| IDE | IntelliJ IDEA 2026.2.3 (browser desktop, port 3001) |
| Docker Engine | 29.1.3 |
| Docker Compose | v5.7.0 |
| Git | 2.53.0 |
| Maven | 3.9.12 (system Java 21.0.12.1) |
| Gradle | 7.0 via Gradle Wrapper, Gradle JVM set to JDK 16.0.2 |

---

## 🗂️ Repository Structure

```
DevOps/
└── Labs/
    └── Lab05/
        ├── README.md               ← this file
        ├── Lab5_Solution.pdf       ← solution report with captions and answers
        └── screenshots/            ← 37 required PNG screenshots
```

---

## ✅ Progress at a Glance

| # | Task | Screenshots | Status |
|:-:|---|:-:|:-:|
| 1 | Clone and Launch IntelliJ IDEA | 3 | ✅ Done |
| 2 | Build and Run the Maven Project | 10 | ✅ Done |
| 3 | Build and Run the Gradle Project | 8 | ⏳ 1 pending |
| 4 | Install and Run the Node.js Project | 7 | ✅ Done |
| 5 | Build the React and Node.js API Artifact | 4 | ✅ Done |
| 6 | Build a Docker Image for the Node.js Project | 5 | ✅ Done |
| | **Total** | **37** | **36 / 37** |

---

## 📸 Screenshot Checklist

<details>
<summary><b>Task 1: Clone and Launch IntelliJ IDEA</b> (3)</summary>

- [x] `intellij_repo_clone.png`
- [x] `intellij_running.png`
- [x] `intellij_desktop.png`

</details>

<details>
<summary><b>Task 2: Build and Run the Maven Project</b> (10)</summary>

- [x] `maven_project_clone.png`
- [x] `maven_installed.png`
- [x] `maven_package.png`
- [x] `maven_elasticsearch_dependency.png`
- [x] `maven_repackage.png`
- [x] `maven_test.png`
- [x] `maven_jar.png`
- [x] `maven_running.png`
- [x] `maven_stopped.png`
- [x] `maven_project_closed.png`

</details>

<details>
<summary><b>Task 3: Build and Run the Gradle Project</b> (8)</summary>

- [x] `gradle_project_clone.png`
- [x] `gradle_build.png`
- [ ] `gradle_elasticsearch_dependency.png` ← **pending**
- [x] `gradle_rebuild.png`
- [x] `gradle_jar.png`
- [x] `gradle_running.png`
- [x] `gradle_stopped.png`
- [x] `gradle_project_closed.png`

</details>

<details>
<summary><b>Task 4: Install and Run the Node.js Project</b> (7)</summary>

- [x] `node_project_clone.png`
- [x] `nodejs_installed.png`
- [x] `npm_install.png`
- [x] `node_running.png`
- [x] `node_browser.png`
- [x] `npm_test.png`
- [x] `npm_pack.png`

</details>

<details>
<summary><b>Task 5: Build the React and Node.js API Artifact</b> (4)</summary>

- [x] `react_node_project_clone.png`
- [x] `react_api_dependencies.png`
- [x] `react_api_build.png`
- [x] `react_api_bundle.png`

</details>

<details>
<summary><b>Task 6: Build a Docker Image for the Node.js Project</b> (5)</summary>

- [x] `docker_installed.png`
- [x] `dockerfile_exists.png`
- [x] `docker_images_before.png`
- [x] `docker_build.png`
- [x] `docker_images_after.png`

</details>

---

## 🛠️ Key Commands

**Task 2: Maven**
```bash
sudo apt install -y maven
mvn package                 # BUILD SUCCESS
mvn test
java -jar target/java-maven-app-1.1.0-SNAPSHOT.jar
```

**Task 3: Gradle (JDK 16)**
```bash
./gradlew --version         # JVM: 16.0.2
./gradlew build             # BUILD SUCCESSFUL
java -jar build/libs/java-app-1.0-SNAPSHOT.jar
```

**Task 4: Node.js**
```bash
cd app
npm install
npm start                   # app listening on port 4000!
npm test
npm pack                    # bootcamp-node-project-1.0.0.tgz
```

**Task 5: React + API**
```bash
cd api
npm install
npm run build               # creates server.bundle.js
```

**Task 6: Docker**
```bash
sudo docker build -t lab5:1.0 .
sudo docker images          # lab5 · 1.0
```

---

## 🧩 Challenges and Fixes

| Problem | Fix |
|---|---|
| `COPY --chmod` failed, BuildKit missing | `sudo apt install -y docker-buildx`, then rerun `setup.sh` |
| `TLS handshake timeout` to `ghcr.io` | Retried the build after the network recovered |
| IntelliJ desktop went black (WebSocket disconnect) | `docker compose restart intellij` |
| Maven `PluginResolutionException` | `mvn -U package` (forces fresh downloads) |
| Gradle 7.0 cannot run on Java 21 | Project SDK and Gradle JVM set to JDK 16.0.2 |
| Port 3000 already in use (`EADDRINUSE`) | Changed `app.listen` to port 4000 |

---

## 🧠 Exam Evaluation Questions

<details>
<summary><b>Q1.</b> Which pom.xml values create the JAR name, and what does <code>package</code> do?</summary>

The name is `artifactId-version.jar`, so `java-maven-app` and `1.1.0-SNAPSHOT`. The `package` phase compiles the code, runs tests, and creates the JAR. The Spring Boot `repackage` goal then makes it executable with its dependencies inside.

</details>

<details>
<summary><b>Q2.</b> Why does <code>./gradlew build</code> work without a system Gradle, and which JDK settings matter?</summary>

The wrapper reads `gradle-wrapper.properties`, downloads the exact Gradle version, and uses it. If Java is unsupported, check the Project SDK, language level, Gradle JVM, and `JAVA_HOME`. `./gradlew --version` shows the JVM actually used.

</details>

<details>
<summary><b>Q3.</b> Why does <code>EADDRINUSE</code> happen on port 3000, and why does port 4000 work?</summary>

Port 3000 is already used inside the desktop container, so Node cannot bind to it. Port 4000 is free, and the browser in the same container reaches it at `http://localhost:4000`.

</details>

---

## 🏁 Summary

I launched the IntelliJ IDEA environment in Docker and cloned four projects inside IntelliJ. I added Elasticsearch 7.0.0 to both the Maven and Gradle builds, ran the Maven test phase and both generated JARs, and set the Gradle project to JDK 16.0.2. I also ran the Node.js app on port 4000, ran its tests and `npm pack`, built the React API bundle with webpack, and built the Docker image `lab5:1.0`.
