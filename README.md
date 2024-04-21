# ITI - Docker Lab Two🐋
## Task 1: Run a container using nginx image, and mount a directory from your host into the Docker container.
example: /home/samy/nginx:/home/nginx (bind mount)

#### Created Folder mys and copy index.html to this folder
```bash
docker run -d --publish 4444:80 -v C:\Users\amro7\Desktop\mys:/usr/share/nginx/html nginx
```

---
## Task 2: Create 2 docker network (net-1 & net-2)

### Steps
#### 1. Create 2 docker network (net-1 & net-2)
```bash
docker network create net1
docker network create net2
```

#### 2. Run 2 new containers using nginx:alpine image, and attach the net-1 to them.
```bash
docker run -d --name container1 --network net1 nginx:alpine
docker run -d --name container2 --network net1 nginx:alpine
```

#### 3. Run another 1 new containers using nginx:alpine image, and attach the net-2 to them.
```bash
docker run -d --name container3 --net net2 nginx:alpine
```

#### 4. Inspect the 3 containers to know their IPs and write them aside.
```bash
docker inspect -f '{{.NetworkSettings.Networks.net1.IPAddress}}' container1
172.18.0.2

docker inspect -f '{{.NetworkSettings.Networks.net1.IPAddress}}' container2
172.18.0.3

docker inspect -f '{{.NetworkSettings.Networks.net2.IPAddress}}' container3
172.19.0.2
```

#### 5. Enter a container in the net-1 network and try to ping a container in the net-2 network (What do you notice?)
```bash
docker exec -it container1 sh 
ping 172.19.0.2
ping fails, because not in same network.
```

#### 6. Enter a container in the net-1 network and try to ping the other container in the same network (What do you notice?)
```bash
docker exec -it net1 sh 
ping 172.18.0.3
ping done 
```

## Task 3: Explain the difference between Docker volumes and Bind Mount.
Docker volumes are managed by Docker, abstracted from the host filesystem, and optimized for performance.
They're suitable for sharing data among containers and persisting data across container lifecycles. Bind mounts directly link host directories to container directories, synchronizing data immediately.
They're suitable for development, giving containers access to host files and directories
