# TOC
- [meanings](#meaning-of-bullseye-buster-slim-on-docker-image)
- [curl in dockerfile](#curl-in-dockerfile)
- [docker compose](#docker-compose)
- [TIPS](#tips)
- [Trouble Shooting](#trouble-shooting)

# meaning of `bullseye`, `buster`, `slim` on docker image
In debian image, there are some code name by each version.  
So a tag on a docker image sometimes use the code name.

|Code Name|Version|読み方|
| ---- | ---- | ---- |
|bullseye|v11|ブルズアイ|
|buster|v10|バスター|
|stretch|v9|ストレッチ|
|jessie|v8|ジェシー|

[https://prograshi.com/platform/docker/docker-image-tags-difference/](https://prograshi.com/platform/docker/docker-image-tags-difference/)

# curl in dockerfile
A result of curl command can be cached.

# docker compose
Technically, `docker compose` and `docker-compose` are different.
## start by docker compose
```
$ sudo docker compose up -d
```
## stop a container and remove the image
```
$ sudo docker compose stop hogeContainer
$ sudo docker compose rm hogeContaner
$ sudo docker rmi hogeImage
```

##  Balus
This command stops containers and delete the images.  
But once it deletes images, the caches still exists so you can quickly rebuild it.
```
$ docker compose down --rmi all --volumes --remove-orphans
```

## ps
```
$ sudo docker compose ps --format json nodejs
```
## copy fine in a container to host machine.
```
$ docker compose cp foo-container:/usr/share/foo/foo.yml ./foo.yml
```
sudo -> the file right is for super user. so after that, you have to add rights to normal user.

## docker-compose.yml
### `tty` and `stdin_open`
This setteing is for using `exec -it`.
```
tty: true
stdin_open: true
```
[https://qiita.com/yuki_0920/items/dc3f32667d004979cc5a](https://qiita.com/yuki_0920/items/dc3f32667d004979cc5a)

# TIPS
## how to run several programs on "docker start"
create `startup.sh` and call several shell or programs in the shell.  
But if you use `startup.sh`, be careful about user right.
### startup.sh 
```
#!/bin/bash

for script in `ls -1 /root/start-scripts/`
do
    /root/start-scripts/$script 
done
```

## nodejs
1. npm init @ empty container using this `entrypoint at docker compose up -d`
```
ENTRYPOINT ["/bin/sh", "-c", "while :; do sleep 10; done"]
```

## chomod is import when you share file between host and container.

## caring about the timing of sharing volume
caution : especially, using dockerfile and docker-compose at the same time   
(image setting) -> RUN -> docker-compose.yml -> CMD

### e.g. cannot get the intended result of curl command
dockerfile
```
WORKDIR /usr/share
RUN apk add curl
RUN mkdir ELK_VERSIONN
RUN curl -L -O "https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-${ELK_VERSIONN}-linux-x86_64.tar.gz"
#RUN tar -xzvf filebeat-${ELK_VERSION}-linux-x86_64.tar.gz
```
But this ${} varable is evaluated as none like following.
```
http://${hoge} -> empty
```
This is caused by the timing around environmental variables.  

# Trouble Shooting
## suddenly error at starting
reboot your PC then may be soleved.  

## Static IP bug
A setting for static IP did not work.  
This is caused by the version of docker compose.  
There is a diffference between `docker compose` and `docker-compose` aroud setting static IP.  
bad
```
"com.docker.compose.version": "1.0-alpha"
```
good
```
"com.docker.compose.version": "1.29.0"
```

## Error failed to solve with frontend dockerfile.v0 when attempting to build with docker compose build 
### error msg
This suddenly happened after update docker.
```
$ docker compose up -d
...
...
 => ERROR [internal] load metadata for docker.elastic.co/elasticsearch/elasticsearch:7.10.2                                                                                             0.5s
------
 > [internal] load metadata for docker.elastic.co/elasticsearch/elasticsearch:7.10.2:
------
failed to solve: rpc error: code = Unknown desc = failed to solve with frontend dockerfile.v0: failed to create LLB definition: rpc error: code = Unknown desc = error getting credentials - err: exit status 1, out: ``

$ 
```
### cause
There is no access ID/PW for docker hub.
But if you try `docker logout` and `docker login` again,  
That does not solve this problem.  
Because probably, keystore system on Mac book blocks the change.  
So at first you have to unlock it and delete already existed config.  
  
### solution
It is solved by removing "credsStore" from ~/.docker/config.json
Before:
```
{
  "auths": {},
  "credsStore": "desktop"
}
```
After:
```
{
  "auths": {}
}
```
or
```
$ rm ~/.docker/config.json
```

### c.f.
https://github.com/docker/compose/issues/9560