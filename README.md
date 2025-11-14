 # Docker Architecture 

##  What is Docker?

Docker is an open-source platform that allows developers to **build, package, and run applications** inside lightweight, portable units called **containers**.

Each container includes everything the application needs:

* Application code
* Runtime
* Libraries
* System tools
* Configuration files

Because everything is packaged together, the application runs **the same on any system** — whether it’s a developer’s laptop, a testing machine, a production server, or the cloud.

---

 ##  Docker Architecture Overview

Docker uses a **client–server model**, which means:

* The **Docker Client (CLI)** sends commands
* The **Docker Daemon (Server)** performs the actual work

The architecture is made up of:

1. Docker CLI (Client)
2. Docker Daemon (Server)
3. Docker Hub (Registry)
4. Docker Images
5. Docker Containers

Let’s understand each one clearly 

---

 ##  1. Docker CLI (Client)

The Docker CLI is the tool you use to interact with Docker — the commands you type (`docker run`, `docker ps`, etc.) are all given through the CLI.

It **does not perform tasks itself**.
Instead, it sends your instructions to the Docker Daemon.

 You can think of the Docker CLI as a **remote control** — you press buttons, but the actual machine (daemon) does the work.

---

 ##  2. Docker Daemon (Server)

The Docker Daemon is the **engine of Docker** — a background service that:

* Builds containers
* Runs containers
* Downloads and manages images
* Manages networks, volumes, storage
* Processes all API requests

Every time you use Docker, the daemon is the one executing your tasks.

 Think of the Docker Daemon as a **factory worker** who receives orders from the CLI and produces containers.

---

 ##  3. Docker Hub (Registry)

Docker Hub is Docker’s **cloud-based image registry**.
It acts as a large public library of images for:

* Ubuntu
* Nginx
* MySQL
* Redis
* Node.js
* etc.

It allows you to:

* **Pull** images (download)
* **Push** images (upload)

Companies can also run **private registries** for internal purposes.

 Docker Hub is basically an **App Store for Docker Images**.

---

 ##  4. Docker Images

A Docker Image is the **blueprint** used to create containers.

An image contains:

* Application code
* Runtime (e.g., Java, Python, Node)
* Dependencies
* OS-level tools and libraries

Images are:

* Read-only
* Built in layers
* Lightweight
* Version-controlled

 Think of an image as a **recipe**, and a container as a **dish prepared using that recipe**.

---

 ##  5. Docker Containers

A Docker Container is a **running instance** of an image.

It is:

* Isolated
* Lightweight
* Fast
* Portable

The container shares the host’s OS kernel, which makes it extremely efficient, but it has its own:

* File system
* Network
* Process space

You can run multiple containers from the same image — deleting a container never deletes the image.

 A container is like a **mini virtual computer** running your application independently.

---

 ##  How All Components Work Together

Here’s the complete workflow of Docker:

1. You run a command using the **Docker CLI**.
2. The **Docker Daemon** receives the request.
3. If required, the daemon **pulls the image** from Docker Hub.
4. The daemon uses the image to **create a container**.
5. The container starts running your application in an **isolated environment**.

➡ This entire process ensures your app runs **consistently and reliably anywhere**.


### **Update & Install**

```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
docker --version
```

### **Fix Permissions**

```bash
sudo chmod 666 /var/lib/docker.sock
```

<img width="1466" src="https://github.com/user-attachments/assets/86163e17-f776-4a18-8e1f-6f181362bb37" />

---

 ## 2. Pulling & Running Containers

### **Pull an Image**

```bash
docker pull <image>
```

### **Run a Container (Detached Mode)**

```bash
docker run -dt <image>
```

### **Example**

```bash
docker run -dt ubuntu
```

### **Check Available Images**

```bash
docker images
```

### **Check Running Containers**

```bash
docker ps
```

### **Check All Containers**

```bash
docker ps -a
```

<img width="1456" src="https://github.com/user-attachments/assets/3113f17c-5cdd-4c60-a33c-f08c14c66261" />

---

 ## **3. Enter a Running Container**

### **Enter a Running Detached Container**

```bash
docker exec -it <container-id> /bin/bash
```

<img width="1469" src="https://github.com/user-attachments/assets/00a2ec1b-54d8-489a-8586-2be5f0762db1" />

### **Run in Front-End / Interactive Mode**

```bash
docker run -it <image>
```

 *Note: Exiting this mode stops the container.*

<img width="1468" src="https://github.com/user-attachments/assets/353e68ab-695a-4d92-9174-e5715765a0e7" />

---

 ## **4. Inspect Containers**

### **Inspect Container Details**

Shows metadata, volumes, IP address, mounts, etc.

```bash
docker inspect <container-id>
```

<img width="1476" src="https://github.com/user-attachments/assets/445f4912-3688-4fbc-af5b-1255b801d08c" />

---

 ## **5. Stop, Kill & Start Containers**

### **Kill a Container (force stop)**

```bash
docker kill <id>
```

### **Start a Stopped Container**

```bash
docker start <id>
```

 *Detached mode preserves container data after restart.*

<img width="1483" src="https://github.com/user-attachments/assets/e3b8681d-078c-457a-aae3-bc50934ed591" />

---

 ## **6. Clean Images & Containers**

### **Delete Unused Images**

```bash
docker image prune -a
```

<img width="1472" src="https://github.com/user-attachments/assets/88e3d4c1-224a-4c87-b3e1-2ed4dd7861f0" />

### **Force Remove a Container**

```bash
docker rm -f <id>
```

<img width="1464" src="https://github.com/user-attachments/assets/50a78227-3778-4735-9d6e-eb225a58c7bc" />

---

 ## **7. Naming Containers**

### **Run Container with a Custom Name**

```bash
docker run -dt --name <name> <image>
```

<img width="1468" src="https://github.com/user-attachments/assets/80752dc1-9950-41ac-a46f-09fe73ce0b53" />

---

 ## **8. Docker Ports**

## **A. Port Forwarding (Random Port)**

Docker chooses a random available port.

```bash
docker run -dt --name <name> -P <image>
```

<img width="1473" src="https://github.com/user-attachments/assets/7fd45322-8250-44cf-b849-e5da0b517718" />
<img width="1674" src="https://github.com/user-attachments/assets/631e1592-9507-4219-9351-53b6c933ca3e" />
<img width="1518" src="https://github.com/user-attachments/assets/91aa4696-43e6-47b7-8fae-bff88f59bf27" />

---

## **B. Port Mapping (Custom Port)**

Map your system port → container port.

```bash
docker run -dt --name <name> -p hostPort:containerPort <image>
```

Example:

```bash
docker run -dt --name web -p 8080:80 nginx
```

<img width="1475" src="https://github.com/user-attachments/assets/fe6a1556-55e1-457d-8395-a862b0287d28" />
<img width="1654" src="https://github.com/user-attachments/assets/58c4bac8-9c79-4719-ac79-b94b2e01278d" />
<img width="1919" src="https://github.com/user-attachments/assets/01fa9a68-921e-4bd1-ab37-5beb24ea205c" />

---

 ## **9. Docker Volumes (Persistent Storage)**

Docker volumes store data even if containers are deleted.

---

## **A. Unnamed Volumes**

Created automatically by Docker.

```bash
docker run -dt --name <name> -P -v /app-data <image>
```

<img width="1245" src="https://github.com/user-attachments/assets/45e14ca2-015e-421d-ac64-3d4b9bf3cb5d" />

---

## **B. Named Volumes**

Docker manages them; easier for backups.

```bash
docker run -dt --name <name> -p 8090:80 -v vol-name:/app-data <image>
```

<img width="1582" src="https://github.com/user-attachments/assets/50e305ca-5623-4841-b519-ddfac26bc12a" />

---

## **C. Host Volumes (Recommended)**

Admin can directly access files on the host.

```bash
docker run -dt --name <name> -v /home/ubuntu/folder:/app-data <image>

```
<img width="1695" height="697" alt="Screenshot 2025-11-13 105912" src="https://github.com/user-attachments/assets/1f8777e0-0e6e-452f-a7b2-46315d7d4d63" />

---

 ## ** Docker Command Definitions**

| Command                 | Meaning                            |
| ----------------------- | ---------------------------------- |
| `docker pull`           | Downloads an image from Docker Hub |
| `docker run`            | Creates + starts a new container   |
| `-dt`                   | Runs in detached background mode   |
| `-it`                   | Runs in interactive terminal mode  |
| `docker ps`             | Shows running containers           |
| `docker ps -a`          | Shows all containers               |
| `docker exec -it`       | Enter a running container          |
| `docker stop`           | Gracefully stops a container       |
| `docker kill`           | Force stops a container            |
| `docker start`          | Starts a stopped container         |
| `docker rm -f`          | Deletes a container forcefully     |
| `docker image prune -a` | Deletes all unused images          |
| `-P`                    | Random port mapping                |
| `-p host:container`     | Custom port mapping                |
| `-v`                    | Mounts volumes                     |

---




