# Docker - Starting Point

## Building Images
### 1. Build an image from a `Dockerfile`

```erlang
docker build -t image-name:tag .
```

*   `-t` -> defines the image name and tag (for example: `myapp:1.0`)
*   `.` -> indicates the directory where the `Dockerfile` is located
* * *
### 2. Create an image from a running container

```gherkin
docker commit <container_id> image-name:tag
```

* * *
### 3. List images

```plain
docker images
```

* * *
### 4. Remove an image

```plain
docker rmi image-name:tag
```

* * *
## Containers From Images
### 1. Run an interactive container

```plain
docker run -it image-name
```

### 2. Run in the background (detached)

```haskell
docker run -d --name my-container image-name
```

### 3. Map a port

```css
docker run -d -p 8080:80 image-name
```

👉 maps host port `8080` to container port `80`
* * *
## Management
### 1. View running containers

```powershell
docker ps
```

### 2. View all containers, including stopped ones

```css
docker ps -a
```

### 3. Stop a container

```gherkin
docker stop <container_id>
```

### 4. Remove a container

```gherkin
docker rm <container_id>
```

* * *
## Pushing and Pulling Images
### 1. Log in to Docker Hub

```plain
docker login
```

### 2. Push an image

```perl
docker push username/image-name:tag
```

### 3. Pull an image

```plain
docker pull image-name:tag
```

* * *
## Extra - Cleanup
### Remove unused images

```plain
docker image prune
```

### Remove everything unused (containers, volumes, networks, images)

```css
docker system prune -a
```
