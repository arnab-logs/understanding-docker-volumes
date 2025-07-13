
---

````markdown
# Docker Volumes – Quick Notes & Commands

A handy reference for learning and practicing Docker Volumes.

---

## Volume Basics

### Create a Volume
```bash
docker volume create demo-volume
```
Creates a Docker-managed logical volume.

---

### List All Volumes
```bash
docker volume ls
```
Shows all volumes created on your system.

---

### Inspect a Volume
```bash
docker volume inspect demo-volume
```
Displays volume details like mount location, creation time, driver, etc.

---

## Running Containers with Volumes

### Mount Volume Using `--mount`
```bash
docker run -d \
  --name web-server \
  --mount source=demo-volume,target=/app \
  nginx:latest
```
Mounts `demo-volume` to `/app` inside the container.  
Use `--mount` for clarity and readability.

---

### Mount Volume Using `-v` (Shorthand)
```bash
docker run -v demo-volume:/app nginx
```
Quick version of volume mounting.  
Useful for small tasks, but less readable in team environments.

---

## Inspect Container Volume Usage

### View Container Mount Info
```bash
docker inspect web-server
```
Look for the `Mounts` section to confirm volume usage.

---

## Volume Deletion Protection

### Try Deleting an In-Use Volume
```bash
docker volume rm demo-volume
```
Returns error if volume is still attached to a container.

---

## Safe Deletion Workflow

1. **Stop the container**
```bash
docker stop web-server
```

2. **Remove the container**
```bash
docker rm web-server
```

3. **Now delete the volume**
```bash
docker volume rm demo-volume
```

---

## Remove All Unused Volumes
```bash
docker volume prune
```
Deletes all dangling volumes not used by any containers.

---

## Summary of Use Cases

- Use **Volumes** for:
  - Persisting logs, user uploads, config files
  - Sharing data between containers
  - Backups and recovery
  - External or cloud-based storage

- Use **Bind Mounts** for:
  - Accessing host files directly
  - Live code editing during development
  - Specific path-based sharing needs

---

## Volume Storage Path (Advanced)
Docker stores volume data at:
```
/var/lib/docker/volumes/<volume-name>/_data
```
You can `cd` into it if needed (on Linux/macOS) — but usually, Docker manages it for you.

---

## Best Practices

- Prefer **volumes** over bind mounts for portability and management
- Use `--mount` for readable, maintainable commands
- Avoid writing important data inside the container filesystem

---


````