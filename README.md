# Devops with Docker

## Part 1

### 1.1
```
docker ps -a
CONTAINER ID        IMAGE       COMMAND                     CREATED             STATUS                      PORTS           NAMES
252ae131fce3        nginx       "/docker-entrypoint..."     20 seconds ago      Up 18 seconds               80/tcp          vibrant_shannon
d7cc527f7d5f        nginx       "/docker-entrypoint..."     21 seconds ago      Exited (0) 7 seconds ago                    infallible_robinson
9da8b776be12        nginx       "/docker-entrypoint..."     23 seconds ago      Exited (0) 11 seconds ago                   zen_banzai
```
### 1.2
```
docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
```
### 1.3
```
Give me the password: basics
You found the correct password. Secret message is:
"This is the secret message"
```
### 1.4
```
#tail -f ./logs.txt
Wed, 28 Oct 2020 20:11:54 GMT
Wed, 28 Oct 2020 20:11:57 GMT
Wed, 28 Oct 2020 20:12:00 GMT
Wed, 28 Oct 2020 20:12:03 GMT
Secret message is:
"Docker is easy"
```
### 1.5
```
sh -c 'echo "Input website:"; read website; echo "Searching.."; sleep 1; curl -L http://$website;'
```
### 1.6
```
FROM devopsdockeruh/overwrite_cmd_exercise

CMD ["-c"]
```
### 1.7
Commands:
```
docker build -t curler .
docker run -it curler
```
Dockerfile:
```
FROM ubuntu:16.04

WORKDIR /usr/local/bin
RUN apt-get update && apt install -y curl
COPY script.sh .
RUN chmod a+rx script.sh

ENTRYPOINT ["bash", "script.sh"]
```
Script.sh:
```
#!/bin/sh
echo "Input website:"; read website; echo "Searching.."; sleep 1; curl -L http://$website;
```
### 1.8
Command:
```
docker run -v ${PWD}/logs.txt:/usr/app/logs.txt devopsdockeruh/first_volume_exercise
```
Logs.txt:
```
Mon, 02 Nov 2020 23:08:25 GMT
Mon, 02 Nov 2020 23:08:28 GMT
Mon, 02 Nov 2020 23:08:31 GMT
Mon, 02 Nov 2020 23:08:34 GMT
Secret message is:
"Volume bind mount is easy"
Mon, 02 Nov 2020 23:08:40 GMT
Mon, 02 Nov 2020 23:08:43 GMT
Mon, 02 Nov 2020 23:08:46 GMT
Mon, 02 Nov 2020 23:08:49 GMT
Secret message is:
"Volume bind mount is easy"
Mon, 02 Nov 2020 23:08:55 GMT
Mon, 02 Nov 2020 23:08:58 GMT
Mon, 02 Nov 2020 23:09:01 GMT
```
### 1.9
Command:
```
docker run -p 80:80 devopsdockeruh/ports_exercise
```
```
```
```
```
```
```
```
```
```
