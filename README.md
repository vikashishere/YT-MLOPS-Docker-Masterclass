# YT-MLOPS-Docker-Masterclass

Here are the notes covering Docker concepts discussed up to the current timestamp (32:38):

### **Why Docker?**
* **Problem:** Applications often run correctly on a developer's machine but fail in testing or production environments due to different dependencies, operating systems, or versions ("It works on my machine").
* **Solution:** Docker encapsulates the application code, its runtime, system tools, libraries, and dependencies into a portable "package" (container), ensuring consistent behavior across any system (13:22 - 14:35).

### **Key Docker Benefits**
* **Portability:** Apps run identically on a laptop, physical server, or cloud server (19:05).
* **Isolation:** Multiple applications can run on the same server without interfering with each other's dependencies (19:25).
* **Scalability:** Enables spinning up or removing containers dynamically based on traffic loads to save resources (21:58).

### **Docker vs. Virtual Machines (VMs)**
* **VMs:** Require a full guest operating system for each application, which is heavy and slow (27:13).
* **Docker:** Lighter and faster as it uses containerization, sharing the host OS kernel instead of virtualizing the entire OS (31:54).

### **Docker Engine**
* The **Docker Engine** is the core software that manages Docker objects such as containers, images, and networks (1:27:37).
* It consists of three main layers:
    1. **Docker Daemon:** Listens for API requests and manages Docker objects (1:27:55).
    2. **REST API:** The communication layer that allows the user or tools to interact with the Docker Daemon (1:28:06).
    3. **Docker CLI:** The tool (command-line interface) used by the developer to send commands like `docker build` or `docker run` (1:28:19).
 



<img width="777" height="292" alt="image" src="https://github.com/user-attachments/assets/1bea943f-dde6-467b-85d9-8ec05c0b0f01" />



<img width="1160" height="745" alt="image" src="https://github.com/user-attachments/assets/ea73d54b-a44f-4b58-aeb9-913be85dc289" />



<img width="1162" height="652" alt="image" src="https://github.com/user-attachments/assets/8a962228-aa14-40d8-a6c3-3e4a08e3a466" />










<img width="1069" height="754" alt="image" src="https://github.com/user-attachments/assets/4fd08636-8afe-4941-8e97-a16037934a59" />
<img width="923" height="515" alt="image" src="https://github.com/user-attachments/assets/ac637116-596b-4e44-b74f-0b2a37b2a58a" />

<img width="1343" height="736" alt="image" src="https://github.com/user-attachments/assets/c926e1b1-0684-4ada-8c47-ffed494a5190" />


- **NOTE**
-

WORKDIR /app: Automatically creates and sets /app as the execution context for all subsequent commands.


COPY . .: First . represents local host project directory; second . represents /app inside the container.

The 5000 port showen in picture is of image port that we map to our pc or cloud port
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

```bash id="olfsjp"
# Install Docker Desktop
# Provides Docker Engine and a graphical interface for managing containers

# Sign in to Docker Hub
# Authenticate with your Docker Hub account
docker login

# Verify Docker installation
# Check that Docker is installed and working correctly
docker --version

# Test Docker setup
# Download and run a sample container to confirm Docker is working
docker run hello-world

# Build a Docker image
# Create an image from the Dockerfile in the current directory
docker build -t <local-image-name> .

# Run a container
# Start a container from a local image and expose a port->host port is set in docker file (EXPOSE)
docker run -p <host-port>:<container-port> <local-image-name>

# Tag an image
# Create a Docker Hub tag for the local image
docker tag <local-image-name> <dockerhub-username>/<repository-name>: insert a tag name like(latest)

# Push an image
# Upload the tagged image to Docker Hub
docker push <dockerhub-username>/<repository-name>:latest

# Pull an image
# Download an image from Docker Hub
docker pull <dockerhub-username>/<repository-name>:latest

# Run a pulled image
# Start a container from the downloaded image
docker run -p <host-port>:<container-port> <dockerhub-username>/<repository-name>:latest
```

**Placeholders**

```bash id="xv2r8n"
<local-image-name>   # Name of the image on your machine
<dockerhub-username> # Your Docker Hub username
<repository-name>    # Repository name on Docker Hub
<host-port>          # Port on your computer
<container-port>     # Port exposed by the container
```

**Workflow**

```bash id="g7k3dm"
# Build → Run → Tag → Push → Pull → Run

docker build -t <local-image-name> .
docker run -p <host-port>:<container-port> <local-image-name>

docker tag <local-image-name> <dockerhub-username>/<repository-name>:latest
docker push <dockerhub-username>/<repository-name>:latest

docker pull <dockerhub-username>/<repository-name>:latest
docker run -p <host-port>:<container-port> <dockerhub-username>/<repository-name>:latest
```





<details>
<summary>Click to expand</summary>

```bash
```bash id="r4g8nq"
# docker login
# Prerequisites:
# - Docker Desktop installed and running
# - Docker Hub account created
docker login


# docker --version
# Prerequisites:
# - Docker CLI installed
docker --version


# docker run hello-world
# Prerequisites:
# - Docker Engine running
# - Internet connection (if image is not already downloaded)
docker run hello-world


# docker build -t <local-image-name> .
# Prerequisites:
# - Dockerfile exists in the current directory
# - Application source code is present
# - Required files are not excluded by .dockerignore
docker build -t <local-image-name> .


# docker run -p <host-port>:<container-port> <local-image-name>
# Prerequisites:
# - Image already built locally
# - Application inside the container listens on <container-port>
# - <host-port> is available on the host machine
docker run -p <host-port>:<container-port> <local-image-name>


# docker tag <local-image-name> <dockerhub-username>/<repository-name>:latest
# Prerequisites:
# - Local image exists
# - Docker Hub repository name chosen
docker tag <local-image-name> <dockerhub-username>/<repository-name>:latest


# docker push <dockerhub-username>/<repository-name>:latest
# Prerequisites:
# - Logged in to Docker Hub
# - Image tagged with Docker Hub repository name
# - Permission to push to the repository
docker push <dockerhub-username>/<repository-name>:latest


# docker pull <dockerhub-username>/<repository-name>:latest
# Prerequisites:
# - Internet connection
# - Image exists in Docker Hub repository
# - Access permission (for private repositories)
docker pull <dockerhub-username>/<repository-name>:latest


# docker run -p <host-port>:<container-port> <dockerhub-username>/<repository-name>:latest
# Prerequisites:
# - Image pulled successfully (or accessible from Docker Hub)
# - Application listens on <container-port>
# - <host-port> is available on the host machine
docker run -p <host-port>:<container-port> <dockerhub-username>/<repository-name>:latest
```

### Quick Readiness Checklist

```bash id="8b3mzx"
✓ Docker Desktop installed
✓ Docker Engine running
✓ Docker Hub account created
✓ Dockerfile available
✓ Application source code available
✓ Local image built
✓ Docker Hub repository chosen
✓ Internet connection available
✓ Required ports identified and available
```

### Dependency Flow

```bash id="m7w2kd"
Docker Desktop
    ↓
docker login
    ↓
Dockerfile + Source Code
    ↓
docker build
    ↓
Local Image
    ↓
docker run

Local Image
    ↓
docker tag
    ↓
docker push
    ↓
Docker Hub Repository
    ↓
docker pull
    ↓
docker run
```

...


</details>


<details><summary> some extras update it lster</summary>
Here is the updated, comprehensive set of notes covering all the Docker concepts, commands, strict syntax rules, and troubleshooting solutions discovered during our session. You can copy and append this directly to the end of your master notes file.

---

### Master Docker Reference (Session Additions)

#### 1. Core Docker Architecture & Mechanics

* **`WORKDIR` Auto-Creation:** If the path specified in `WORKDIR` (e.g., `/app`) does not exist inside the image, Docker creates it automatically. All subsequent commands (`COPY`, `RUN`, `CMD`) run relative to this path.
* **`COPY . .` Syntax Explained:**
* **First Dot (`.`):** Local host directory (build context on your machine).
* **Second Dot (`.`):** Target directory inside the container (`WORKDIR`).


* **Layer Caching Optimization:** Splitting file copying prevents unnecessary reinstallations during builds:
```dockerfile
# Copies requirements first so dependencies are cached
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code second
COPY . .

```



---

#### 2. Networking & Execution Context

* **Background Long-Polling Services:** Applications using outbound connections (e.g., Telegram bots using `start_polling`) do **not** require port declarations (`EXPOSE`) or host port mappings (`-p`).
* **Web Applications & APIs:** Require explicit port mapping (`-p <host_port>:<container_port>`) to forward incoming network traffic from your machine to the containerized application (e.g., Flask, FastAPI).

---

#### 3. Image & Storage Lifecycle Management

* **Dangling Images (`<none>`):** Rebuilding an image with an existing tag leaves the old image untagged in storage. Over time, these consume significant disk space.
* **Version Tagging Strategy:** Tag images with explicit version numbers alongside `latest` to maintain rollback points:
```cmd
docker build -t shivansh1729/telegram_bot:v1.0.0 .

```


* **Cleanup Commands:**
```cmd
# Remove specific image
docker rmi <image_id_or_name>

# Remove untagged (dangling) images
docker image prune

# Remove all unused containers, networks, and images (system wipe)
docker system prune -a

```



---

#### 4. Environment Variables (`.env`) Strictness

* **Strict Formatting Rules:** Docker's `--env-file` parser fails on trailing or leading spaces around variable names or the `=` sign.
* **Incorrect:** `TELEGRAM_BOT_TOKEN = "123456"` or `BASE_URL =...`
* **Correct:** `TELEGRAM_BOT_TOKEN=123456`


* **Python Runtime Validation:** Frameworks like `aiogram` validate credentials on startup. Passing empty, missing, or space-corrupted tokens throws an unhandled runtime error (`aiogram.utils.token.TokenValidationError: Token is invalid!`).

---

#### 5. Project Update Workflow

1. **Stop & Remove Active Container:** Step 1.
```cmd
docker stop telegram_bot_container
docker rm telegram_bot_container

```


2. **Rebuild Local Image:** Step 2.
```cmd
docker build -t telegram_bot .

```


3. **Run Updated Container:** Step 3.
```cmd
docker run -d --name telegram_bot_container --env-file .env telegram_bot

```


4. **Verify Logs:** Step 4.
```cmd
docker logs -f telegram_bot_container

```


* **Single-Command Execution Shortcut:**
```cmd
docker rm -f telegram_bot_container && docker build -t telegram_bot . && docker run -d --name telegram_bot_container --env-file .env telegram_bot

```



---

#### 6. Troubleshooting Common Errors

* **Error:** `error during connect: Head ".../_ping": open //./pipe/dockerDesktopLinuxEngine: system cannot find the file specified.`
* **Fix:** Open **Docker Desktop** from Windows and wait until the whale icon status changes to "Docker Desktop is running".


* **Error:** `push access denied, repository does not exist or may require authorization: server message: insufficient_scope`
* **Fix:** Re-tag the local image with your Docker Hub username namespace before pushing (`docker tag local_name username/repository_name:latest`).


* **Error:** `docker: invalid env file (.env): variable 'KEY ' contains whitespaces`
* **Fix:** Strip out trailing whitespaces and remove spaces surrounding `=` inside your `.env` file.


* **Error:** `failed to do request ... write tcp ... use of closed network connection`
* **Fix 1:** Re-run `docker push` directly. Docker automatically saves completed layers and resumes only the missing chunks.
* **Fix 2:** Limit concurrent layer uploads in Docker Desktop Settings under **Docker Engine**:
```json
{
  "max-concurrent-uploads": 1
}

```
</details>
