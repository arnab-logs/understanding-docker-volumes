# Docker Volumes – Quick Notes & Commands

A handy reference for learning and practicing Docker Volumes with real-world examples.

---

## Volume Basics

### Create a Volume
```bash
docker volume create demo-volume
```
Creates a Docker-managed logical volume.

### List All Volumes
```bash
docker volume ls
```
Shows all volumes created on your system.

### Inspect a Volume
```bash
docker volume inspect demo-volume
```
Displays volume details like mount location, driver, and lifecycle metadata.

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

### Mount Volume Using `-v` (Shorthand)
```bash
docker run -v demo-volume:/app nginx
```
Quick shorthand for volume mounting. Less readable in scripts, but works well for local usage.

---

## Inspect Volume Usage in a Container

### View Container Mount Info
```bash
docker inspect web-server
```
Check the `Mounts` section to confirm volume usage and mount path inside the container.

---

## Volume Deletion Protection

### Try Deleting an In-Use Volume
```bash
docker volume rm demo-volume
```
Returns an error if the volume is still attached to a running or stopped container.

---

## Safe Deletion Workflow

1. Stop the container
   ```bash
   docker stop web-server
   ```

2. Remove the container
   ```bash
   docker rm web-server
   ```

3. Now delete the volume
   ```bash
   docker volume rm demo-volume
   ```

---

## Clean Up Dangling Volumes

### Remove All Unused Volumes
```bash
docker volume prune
```
Deletes all dangling volumes that are not currently in use.

---

## Summary of Use Cases

### Use Volumes when you need:
- Data persistence
- Shared storage between containers
- Backup and migration
- Cloud or remote volume support

### Use Bind Mounts when:
- You need to access specific host files or directories
- You want live editing during development

---

## Where Are Volumes Stored?

On most systems, Docker stores volumes in:

```
/var/lib/docker/volumes/<volume-name>/_data
```

This is managed by Docker, but can be inspected manually if needed.

---

## Best Practices

- Prefer Docker Volumes for long-term storage and production use
- Use `--mount` syntax for better readability and control
- Avoid storing important data inside the container filesystem (it will be lost)

---

This notes file is part of my Docker learning journey where I explored how volumes help preserve container data, share files between containers, and support real-world application needs.
