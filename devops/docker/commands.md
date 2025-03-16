Here is a simple summary of all the Docker commands we've discussed, presented clearly and simply:

## Docker Command Cheat Sheet

### 1. **Build Docker image**
```bash
docker build -t image-name .
```

### 2. **Run Docker container**
```bash
docker run -p hostPort:containerPort image-name
```
Example:
```bash
docker run -p 3000:3000 my-node-app
```

### 3. **List running containers**
```bash
docker ps
```

### 4. **List all containers (including stopped)**
```bash
docker ps -a
```

### 5. **Stop a running container**
```bash
docker stop container-id
```

### 6. **Start a stopped container**
```bash
docker start container-id
```

### 7. **Restart a container**
```bash
docker restart container-id
```

### 8. **Remove a container**
```bash
docker rm container-id
```

### 9. **Remove all stopped containers**
```bash
docker container prune
```

### 10. **List Docker images**
```bash
docker images
```

### 11. **Remove Docker image**
```bash
docker rmi image-id
```

### 12. **Remove unused images**
```bash
docker image prune
```

### 13. **Pull an image from Docker Hub**
```bash
docker pull image-name
```
Example:
```bash
docker pull node:20-alpine
```

### 14. **Log into Docker Hub (docker registry)**
```bash
docker login
```

### 15. **Push your Docker image to Docker Hub**
```bash
docker push username/image-name
```

### 16. **See container logs**
```bash
docker logs container-id
```

### 17. **Execute command inside a running container**
```bash
docker exec -it container-id sh
```

Example with Bash:
```bash
docker exec -it container-id bash
```

### 18. **Inspect details of a container/image**
```bash
docker inspect container-id/image-id
```

### 19. **Copy files from container to host**
```bash
docker cp container-id:/path/in/container /path/on/host
```

### 20. **Copy files from host to container**
```bash
docker cp /path/on/host container-id:/path/in/container
```