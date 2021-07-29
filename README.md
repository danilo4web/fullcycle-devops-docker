## DOCKER COMMANDS:

### Run image as a container:
```
docker run nginx
docker run -d -p 8081:80 nginx
docker run -d -p 8081:80 --name nginx nginx
docker exec -it nginx bash
```

### Mount: Bind
```
docker run -d --name nginx -p 8081:80 --mount type=bind,source=$(pwd)/docker/html/,target=/usr/share/nginx/html/ nginx
```

### Alias PHP8:
```
docker run --rm -v $(pwd):/app -w /app php php arquivo.php
alias php8='docker run --rm -v $(pwd):/app -w /app php php'
```

### Volumes:
```
docker volume ls
docker volume create myvolume
docker volume inspect myvolume
```

### Sharing volumes:
```
docker run --name nginx -d --mount type=volume,source=myvolume,target=/app nginx
docker run --name nginx2 -d --mount type=volume,source=myvolume,target=/app nginx
```

### Images:
```
docker Images
docker rmi ubuntu
```


### Builder images:
file: Dockerfile
```
FROM nginx:latest
  
RUN apt-get update && \
    apt-get install vim -y

COPY html/ /usr/share/nginx/html
```
docker build -t danilo4web/nginx-com-vim:latest . \
docker run -it danilo4web/nginx-com-vim:latest bash

### CMD:
```
FROM ubuntu:latest

CMD [ "echo", "Hello World" ]
```
docker build -t danilo4web/hello:latest . \
docker run --rm danilo4web/hello \
docker run --rm danilo4web/hello echo "Hi Danilo"


### ENTRYPOINT:
```
FROM ubuntu:latest

ENTRYPOINT [ "echo", "Hello " ]
CMD [ "World" ]
```
docker build -t danilo4web/hello-var:latest .  \
docker run --rm danilo4web/hello-var  \
docker run --rm danilo4web/hello-var "Danilo"

### Publish to Docker Hub
file: Dockerfile
```
FROM nginx:latest

COPY html/ /usr/share/nginx/html

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["nginx", "-g", "daemon off;"]
```

docker build -t danilo4web/nginx-fullcycle . \
docker run --rm -d -p 8081:80 danilo4web/nginx-fullcycle  \
docker push danilo4web/nginx-fullcycle