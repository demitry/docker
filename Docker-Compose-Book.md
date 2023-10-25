# Docker-Compose Book

## Ch 1

```bash
docker version
docker compose version

docker run --rm -p 8080:80 --name nginx-compose nginx

curl 127.0.0.1:8080

docker run --rm -p 8080:80 --name nginx-compose -v $(pwd) static-site:/usr/share/nginx/html nginx
docker run --rm -p 8080:80 --name nginx-compose -v ${pwd} static-site:/usr/share/nginx/html nginx

```

 https://stackoverflow.com/posts/51379726
- In powershell you should use `${pwd}` instead of `$(pwd)`
- THIS is what caused me to bang my head into the wall

```
services:
  nginx:
    image: nginx
    ports:
      - 8080:80
```

docker compose up

```
services:
  nginx:
    image: nginx
    ports:
      - 8080:80
    volumes:
    - ./static-site:/usr/share/nginx/html
```

docker compose up

JSON
- CloudWatch (https://aws.amazon.com/cloudwatch/), 
- StackDriver (https://cloud.google.com/products/operations)
- ELK Stack (https://www.elastic.co/elasticstack/)


```
docker ps
docker exec -it ch1-nginx-1 cat /etc/nginx/nginx.conf
docker exec -it ch1-nginx-1 cat /etc/nginx/nginx.conf > origImageConfig.conf
docker cp chapter1-nginx-1: /etc/nginx/nginx.conf nginx.conf
```
