Here is the complete **Docker Compose Guide** as saved in your [`notes.md`](file:///wsl.localhost/Ubuntu/home/dell/voice-agent/notes.md):

---

# 🐳 Complete End-to-End Docker Compose Guide

## 1. What is Docker Compose & Why Use It?

### The Problem: Manual `docker run`
When building modern applications with multiple components (e.g., FastAPI backend, React frontend, MongoDB/PostgreSQL, Redis), starting containers individually requires long, tedious commands:

```bash
# Starting Backend manually
docker run -d --name backend -p 8000:8000 --env-file ./backend/.env -v ./backend:/app my-backend-image

# Starting Frontend manually
docker run -d --name frontend -p 3000:80 my-frontend-image
```

Doing this manually every time is error-prone and hard to maintain.

### The Solution: Docker Compose
**Docker Compose** is an orchestration tool that allows you to define, configure, and run multi-container applications using a single YAML configuration file (`docker-compose.yml`).

With Docker Compose, your entire application stack starts with **one single command**:
```bash
docker compose up --build
```

---

## 2. Anatomy & Core Concepts of `docker-compose.yml`

Every standard `docker-compose.yml` file consists of 3 primary top-level sections:

```yaml
services:  # 1. Defines all application containers/microservices
  backend: ...
  frontend: ...

volumes:   # 2. Defines persistent data stores (DB data, caches)
  db_data: ...

networks:  # 3. Defines virtual networks connecting services
  app_network: ...
```

---

### Detailed Key Breakdown

| Key | Purpose | Example / Details |
| :--- | :--- | :--- |
| **`build`** | Builds a Docker image from a local `Dockerfile`. | `context: ./backend`, `dockerfile: Dockerfile` |
| **`image`** | Downloads a prebuilt image from Docker Hub instead of building locally. | `image: mongo:latest` or `image: redis:alpine` |
| **`container_name`** | Assigns a custom human-readable container name instead of a random auto-generated string. | `container_name: rag-voice-agent-backend` |
| **`ports`** | Maps host machine ports to internal container ports (HOST:CONTAINER format). | `- '8000:8000'` (Access at `http://localhost:8000`) |
| **`env_file`** | Loads environment variables from an external file. | `- ./backend/.env` |
| **`environment`** | Directly sets environment variables inside the container. | `- PYTHONUNBUFFERED=1` |
| **`volumes`** | Syncs local folders into the container (Bind Mounts) OR saves data permanently (Named Volumes). | `- ./backend:/app` (Live code reloading) |
| **`depends_on`** | Defines startup sequence dependencies between services. | Frontend waits until Backend is healthy before launching. |
| **`healthcheck`** | Periodically checks if an app inside the container is responsive. | Tests `http://localhost:8000/health` every 30s. |
| **`restart`** | Container auto-restart policy. | `unless-stopped`, `always`, or `on-failure`. |
| **`networks`** | Connects containers to a shared DNS-enabled virtual network. | Allows Frontend to reach Backend using container name. |

---

## 3. How Networking & Communication Work

In Docker Compose, containers connected to the same custom network can talk to each other **by using their service name as the domain name**.

```text
┌────────────────────────────────────────────────────────┐
│               Host Machine (Your Computer)              │
│  Frontend Browser Port: 3000   │  Backend Port: 8000  │
└───────────────────────────┬────────────────────────────┘
                            │
┌───────────────────────────▼────────────────────────────┐
│          Docker Network (rag-voice-agent-network)      │
│                                                        │
│   ┌─────────────────────┐    HTTP / WS    ┌──────────┐ │
│   │  frontend container │ ──────────────► │ backend  │ │
│   └─────────────────────┘   (DNS lookup)  └──────────┘ │
└────────────────────────────────────────────────────────┘
```

- Inside the Docker network, the frontend communicates with the backend via `http://backend:8000` (not `localhost`).
- From your host machine's browser, you access them via `http://localhost:3000` and `http://localhost:8000`.

---

## 4. Complete Command Reference Cheat Sheet

### Lifecycle Commands

| Action | Command | Explanation |
| :--- | :--- | :--- |
| **Build & Start** | `docker compose up --build` | Rebuilds Dockerfiles if changed and starts all containers in foreground. |
| **Start in Background** | `docker compose up -d` | Runs containers in detached (background) mode. |
| **Stop Services** | `docker compose down` | Stops and removes containers, networks, and default volumes. |
| **Stop & Delete Volumes** | `docker compose down -v` | Destroys containers AND named data volumes (cleans database state). |
| **Start / Stop without rebuild** | `docker compose start` / `docker compose stop` | Pauses/resumes containers without destroying them. |

---

### Inspection & Debugging Commands

| Action | Command | Explanation |
| :--- | :--- | :--- |
| **View Live Logs** | `docker compose logs -f` | Streams real-time stdout/stderr logs from all services. |
| **View Service Logs** | `docker compose logs -f backend` | Streams logs for a specific service (`backend`). |
| **List Running Containers** | `docker compose ps` | Shows running containers, status, and port mappings. |
| **Execute Command in Container** | `docker compose exec backend bash` | Opens an interactive bash terminal session inside a running container. |
| **Check Process Top** | `docker compose top` | Displays running processes inside each container. |

---

## 5. Universal Best Practices & Rules Across ALL Projects

1. **Do I need to recreate `docker-compose.yml` every time?**
   - **No.** You write it once per project repository. Git tracks it, and developers pull and run `docker compose up`.
2. **Never Commit Secrets (`.env` files)**:
   - Always keep `.env` in `.gitignore`. Commit `.env.example` as a template for team members.
3. **Bind Mounts vs Volumes**:
   - **Bind Mounts** (`./backend:/app`): Use during development so changes on your host machine instantly reload inside the container.
   - **Named Volumes** (`db_data:/var/lib/postgresql/data`): Use for databases so data persists even when containers are deleted.
  




Here is the breakdown of **Disk Space**, **RAM Usage**, and **How to clear it completely whenever you want**:

---

### 1. 💾 Disk Space Usage (Storage)

| Component | Space Taken | Details |
| :--- | :--- | :--- |
| **Backend Image** | **~1.5 GB – 2.5 GB** | Contains Python runtime + AI/Audio packages (`tokenizers`, `resampy`, `nltk`, `pydantic`). |
| **Frontend Image** | **~30 MB – 50 MB** | Extremely lightweight (just static React files served by Nginx). |
| **Docker Build Cache** | **~2 GB – 4 GB** | Intermediate layers Docker saves to speed up future builds. |
| **TOTAL DISK SPACE** | **~3.5 GB – 6 GB** | *(Can be cleared anytime)* |

---

### 2. ⚡ RAM Usage (Memory while running)

| Component | RAM Taken | Details |
| :--- | :--- | :--- |
| **Backend Container** | **~300 MB – 600 MB** | Uses memory when processing FastAPI routes and audio streams. |
| **Frontend Container** | **~15 MB – 30 MB** | Uses almost zero memory. |
| **TOTAL RAM USAGE** | **~400 MB – 700 MB** | *(RAM is freed instantly when you stop Docker)* |

---

### 3. 🧹 YES! You Can Clear It Completely Later

Here are the **3 cleanup commands** you can run anytime:

#### A. Free up RAM immediately (Stop running containers)
```bash
docker compose down
```
*(This instantly stops the containers and releases all ~500 MB RAM back to your system).*

---

#### B. Free up ~2 to 4 GB of Disk Space (Clear build cache)
```bash
docker builder prune -f
```
*(Removes temporary build files while keeping your working images).*

---

#### C. Complete Deep Clean (Wipe ALL unused images, containers & caches)
If you ever want to get **100% of your disk space back**:
```bash
docker system prune -a --volumes -f
```
*(This deletes all downloaded/built Docker images and caches, freeing up all ~5+ GB of storage).*
4. **Port Conflicts**:
   - If port `8000` is already in use by another app on your machine, change the **Host** port in `docker-compose.yml`:
     `- '8081:8000'` (maps machine port `8081` to container port `8000`).
