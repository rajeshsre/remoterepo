**Docker volumes**

**Bind-mount**
#docker run -d --name bind-mount-cont --mount type=bind,source=/root/dest,target=/app nginx

#docker exec -it 3b8ecc18ccae /bin/bash

**Bind volume mount**
docker run -d --name bind-volume-cont -v /root/dest:/app1 nginx

**Volume**
root@docker:~# docker volume ls
DRIVER    VOLUME NAME
root@docker:~# docker volume create html-volume
html-volume
root@docker:~# cd /var/lib/docker/
root@docker:/var/lib/docker# ls
buildkit  containers  engine-id  image  network  overlay2  plugins  runtimes  swarm  tmp  volumes

root@docker:/var/lib/docker# cd volumes/
root@docker:/var/lib/docker/volumes# ls
backingFsBlockDev  html-volume  metadata.db
root@docker:/var/lib/docker/volumes# cd html-volume/
root@docker:/var/lib/docker/volumes/html-volume# ls
_data
root@docker:/var/lib/docker/volumes/html-volume# cd _data/
root@docker:/var/lib/docker/volumes/html-volume/_data# ls
root@docker:/var/lib/docker/volumes/html-volume/_data# 

root@docker:~# docker container run -d --name nginx-container --mount type=volume,source=html-volume,target=/usr/share/nginx/html nginx
a3c119e467541eb3061b057221e007ef4575940c08d0dccec41acb8a17655ce9

root@docker:~# docker ps -a
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                  PORTS     NAMES
a3c119e46754   nginx         "/docker-entrypoint.…"   9 seconds ago    Up 9 seconds            80/tcp    nginx-container
f7cd09589b0a   nginx         "/docker-entrypoint.…"   12 minutes ago   Up 12 minutes           80/tcp    bind-volume-cont1
5e6a469fac47   nginx         "/docker-entrypoint.…"   16 minutes ago   Up 16 minutes           80/tcp    bind-volume-cont
3b8ecc18ccae   nginx         "/docker-entrypoint.…"   27 minutes ago   Up 27 minutes           80/tcp    bind-mount-cont
f4abd457b1c5   nginx         "/docker-entrypoint.…"   2 days ago       Exited (0) 2 days ago             n2
75a726c4c587   httpd         "httpd-foreground"       2 days ago       Exited (0) 2 days ago             h2
1f88a1fb35c0   httpd         "httpd-foreground"       2 days ago       Exited (0) 2 days ago             h1
9df36a9dc323   ubuntu        "/bin/bash"              2 days ago       Exited (0) 2 days ago             kind_ardinghelli
10c64f9160f0   ubuntu        "/bin/bash"              2 days ago       Exited (0) 2 days ago             funny_tesla
11aebe34abd1   hello-world   "/hello"                 5 days ago       Exited (0) 5 days ago             flamboyant_stonebraker

root@docker:~# ls /var/lib/docker/volumes/
backingFsBlockDev  html-volume  metadata.db

root@docker:~# ls /var/lib/docker/volumes/html-volume/
_data
root@docker:~# ls /var/lib/docker/volumes/html-volume/_data/
50x.html  index.html
root@docker:~# 

root@docker:/var/lib/docker/volumes/html-volume/_data# 
root@docker:/var/lib/docker/volumes/html-volume/_data# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
a3c119e46754   nginx     "/docker-entrypoint.…"   2 minutes ago    Up 2 minutes    80/tcp    nginx-container
f7cd09589b0a   nginx     "/docker-entrypoint.…"   14 minutes ago   Up 14 minutes   80/tcp    bind-volume-cont1
5e6a469fac47   nginx     "/docker-entrypoint.…"   19 minutes ago   Up 19 minutes   80/tcp    bind-volume-cont
3b8ecc18ccae   nginx     "/docker-entrypoint.…"   30 minutes ago   Up 30 minutes   80/tcp    bind-mount-cont
root@docker:/var/lib/docker/volumes/html-volume/_data# 

root@docker:/var/lib/docker/volumes/html-volume/_data# docker rm -f nginx-container
nginx-container
root@docker:/var/lib/docker/volumes/html-volume/_data# 
root@docker:/var/lib/docker/volumes/html-volume/_data# cd
root@docker:~# 

root@docker:~# ls /var/lib/docker/volumes/html-volume/_data/
50x.html  index.html
root@docker:~# 

root@docker:~# docker container run -d --name nginx-container -P --mount type=volume,source=html-volume,target=/usr/share/nginx/html nginx
b9b9f15def354f7e7b22e2845ec4987e5c2735c84afd1b52184cbd63df237e61
root@docker:~# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                     NAMES
b9b9f15def35   nginx     "/docker-entrypoint.…"   7 seconds ago    Up 7 seconds    0.0.0.0:32768->80/tcp, :::32768->80/tcp   nginx-container
f7cd09589b0a   nginx     "/docker-entrypoint.…"   17 minutes ago   Up 17 minutes   80/tcp                                    bind-volume-cont1
5e6a469fac47   nginx     "/docker-entrypoint.…"   21 minutes ago   Up 21 minutes   80/tcp                                    bind-volume-cont
3b8ecc18ccae   nginx     "/docker-entrypoint.…"   32 minutes ago   Up 32 minutes   80/tcp                                    bind-mount-cont
root@docker:~# 

root@docker:~# ls /var/lib/docker/volumes/html-volume/_data/
50x.html  index.html
root@docker:~# cd /var/lib/docker/volumes/html-volume/_data/
root@docker:/var/lib/docker/volumes/html-volume/_data# ls -l
total 8
-rw-r--r-- 1 root root 497 May 28 13:22 50x.html
-rw-r--r-- 1 root root 615 May 28 13:22 index.html
root@docker:/var/lib/docker/volumes/html-volume/_data# rm -rf index.html 
root@docker:/var/lib/docker/volumes/html-volume/_data# vi index.html
root@docker:/var/lib/docker/volumes/html-volume/_data# 
