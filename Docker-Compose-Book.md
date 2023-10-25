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


JSON
- CloudWatch (https://aws.amazon.com/cloudwatch/), 
- StackDriver (https://cloud.google.com/products/operations)
- ELK Stack (https://www.elastic.co/elasticstack/)

