## Compose sample application

## Node.js application with Nginx proxy and Redis database

Project structure:
```
.
âââ README.md
âââ compose.yaml
âââ nginx
âÂ Â  âââ Dockerfile
âÂ Â  âââ nginx.conf
âââ web
    âââ Dockerfile
    âââ package.json
    âââ server.js

2 directories, 7 files


```
[_compose.yaml_](compose.yaml)
```
redis:
    image: 'redislabs/redismod'
    ports:
      - '6379:6379'
  web1:
    restart: on-failure
    build: ./web
    hostname: web1
    ports:
      - '81:5000'
  web2:
    restart: on-failure
    build: ./web
    hostname: web2
    ports:
      - '82:5000'
  nginx:
    build: ./nginx
    ports:
    - '80:80'
    depends_on:
    - web1
    - web2
```
The compose file defines an application with four services `redis`, `nginx`, `web1` and `web2`.
When deploying the application, docker compose maps port 80 of the nginx service container to port 80 of the host as specified in the file.


> â¹ï¸ **_INFO_**  
> Redis runs on port 6379 by default. Make sure port 6379 on the host is not being used by another container, otherwise the port should be changed.

## Deploy with docker compose

```
$ docker compose up -d
[+] Running 24/24
 â ¿ redis Pulled                                                                                                                                                                                                                      ...
   â ¿ 565225d89260 Pull complete                                                                                                                                                                                                      
[+] Building 2.4s (22/25)
 => [nginx-nodejs-redis_nginx internal] load build definition from Dockerfile                                                                                                                                                         ...
[+] Running 5/5
 â ¿ Network nginx-nodejs-redis_default    Created                                                                                                                                                                                      
 â ¿ Container nginx-nodejs-redis-web2-1   Started                                                                                                                                                                                      
 â ¿ Container nginx-nodejs-redis-redis-1  Started                                                                                                                                                                                      
 â ¿ Container nginx-nodejs-redis-web1-1   Started                                                                                                                                                                                      
 â ¿ Container nginx-nodejs-redis-nginx-1  Started
```


## Expected result

Listing containers must show three containers running and the port mapping as below:


```
docker-compose ps
```

## Testing the app

After the application starts, navigate to `http://localhost:80` in your web browser or run:

```
curl localhost:80
curl localhost:80
web1: Total number of visits is: 1
```

```
curl localhost:80
web1: Total number of visits is: 2
```
```
$ curl localhost:80
web2: Total number of visits is: 3
```



## Stop and remove the containers

```
$ docker compose down
```



## Skills Developed

This project enhanced my understanding of software architecture and best practices.