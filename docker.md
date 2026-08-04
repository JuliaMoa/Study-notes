
## Docker – grunder

- **Image** = mall (blueprint) för en container.
- **Container** = körande instans av en image.
- **Dockerfile** = beskriver hur en image byggs.
- **Docker Compose** = hanterar flera containers via `compose.yaml`.

---

# Vanliga Docker-kommandon

## Containers

```bash
docker run <image>
docker run -it <image>
docker run -d <image> 
docker run --name <container> <image>

docker ps
docker stop <container>
docker start <container>

docker exec -it <container> bash

docker logs <container>
docker logs -f <container>

docker container rm <container>
docker container prune
```

## Images

```bash
docker pull <image>

docker images

docker build -t my-image .

docker image rm <image>

docker history <image>

docker tag <old> <new>

docker image push <image>
```

---

# Dockerfile

```dockerfile
FROM ubuntu

WORKDIR /app

COPY . .

RUN apt-get update

ENV PORT=8080

CMD ["python3","app.py"]
```

| Instruktion | Beskrivning |
|---|---|
| FROM | Bas-image |
| COPY | Kopierar filer |
| RUN | Kör kommando vid build |
| WORKDIR | Byter arbetskatalog |
| USER | Kör som annan användare |
| ARG | Build-variabel |
| ENV | Miljövariabel |
| CMD | Standardkommando |
| ENTRYPOINT | Huvudkommando |

---

# Docker Compose

```bash
docker compose up -d
docker compose up --build
docker compose run 
docker compose down
docker exec -it/bin/bash
docker inspect
docker ps
docker volume ls
docker compose ls
docker image ls -a
docker image prune
docker container prune

docker compose logs
docker compose logs <service>
docker compose top
```

## compose.yaml

```yaml
services:
  app:
    image: nginx
    ports:
      - "8080:80"

volumes:

networks:
```

Vanliga sektioner

- services
- networks
- volumes
- configs
- secrets

## depends_on

- `service_started`
- `service_completed_successfully`
- `service_healthy`

---

# Volumes

Bind mount

```bash
docker run -v <host>:<container> <image>
```

Docker volume

```bash
docker volume create data
docker volume inspect data
docker volume rm data
```

---

# Nätverk

```bash
docker run --network <network>

docker run -p 8080:80 nginx
```

Port mapping:

```
Host:Container
8080:80
```

---

# Bra att komma ihåg

- Image → Container
- Dockerfile bygger images.
- `docker run` skapar containers.
- `docker compose` hanterar flera containers.
- Volumes sparar data permanent.
- Bind mounts delar filer mellan host och container.
- Kör inte containers som root om det inte behövs.

---

# Teori (kort)

## Virtualisering
- VM kör eget operativsystem.
- Containrar delar hostens kernel och startar därför snabbare.

## Kubernetes
- Orkestrerar många containers över flera servrar.
- Passar större system än Docker Compose.

## Säkerhet
- Använd images från pålitliga källor.
- Installera bara det som behövs.
- Uppdatera mjukvara.
- Undvik att köra som root.
