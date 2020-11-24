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
### 1.10
Dockerfile:
```
FROM ubuntu:16.04

WORKDIR /usr/app
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install curl -y
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt install -y nodejs
COPY frontend-example-docker/ .
RUN npm install
RUN npm install -g serve
RUN npm run build

EXPOSE 5000

CMD ["serve", "-s", "-l", "5000", "dist"]
```
Commands:
```
docker build -t frontend .
docker run -p 5000:5000 frontend
```
### 1.11
Dockerfile:
```
FROM ubuntu:latest

WORKDIR /backend
RUN touch logs.txt
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install curl -y
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt install -y nodejs
RUN node -v
COPY backend-example-docker-master/ .
RUN npm install 

EXPOSE 8000

CMD ["npm", "start"]
```
Commands:
```
docker build -t backend .
docker run -p 8000:8000 -v ${PWD}/logs.txt:/backend/logs.txt backend
```
logs.txt:
```
11/9/2020, 7:15:21 PM: Connection received in root
11/9/2020, 7:16:33 PM: Connection received in root
```
### 1.12
Frontend Dockerfile:
```
FROM ubuntu:latest

WORKDIR /usr/app
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install curl -y
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt install -y nodejs
COPY frontend-example-docker-master/ .
ENV API_URL=http://localhost:8000
RUN npm install -g serve
RUN npm run build

CMD ["serve", "-s", "-l", "5000", "dist"]

EXPOSE 5000
```
Backend Dockerfile:
```
FROM ubuntu:latest

WORKDIR /backend
RUN touch logs.txt
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install curl -y
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt install -y nodejs
RUN node -v
COPY backend-example-docker-master/ .
RUN npm install 

EXPOSE 8000

ENV FRONT_URL=http://localhost:5000
CMD ["npm", "start"]
```
Commands:
```
docker build -t frontend .
docker run -p 5000:5000 frontend
docker build -t backend .
docker run -p 8000:8000 backend
```
### 1.13
Dockerfile:
```
FROM openjdk:8

WORKDIR /usr/app/
COPY spring-example-project-master/ .
RUN ./mvnw package
EXPOSE 8080

CMD ["java", "-jar", "./target/docker-example-1.1.3.jar"]
```
Commands:
```
docker build -t javaspring
docker run -p 8080:8080 javaspring
```
### 1.14
Dockerfile:
```
FROM ruby:2.6.0

WORKDIR /app

COPY rails-example-project-master/ .
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt-get install -y nodejs
RUN gem install bundler
RUN bundle install
ENV SECRET_KEY_BASE secret


RUN rails db:migrate RAILS_ENV=production
RUN rake assets:precompile
CMD ["rails", "s", "-e", "production"]

EXPOSE 3000
```
Got error:#12 1.804 ArgumentError: Missing `secret_key_base` for 'production' environment, set this string with `rails credentials:edit` so I added the SECRET_KEY_BASE

Commands:
```
docker build -t rails .
docker run -p 3000:3000 rails
```
### 1.15
Dockerhub: https://hub.docker.com/repository/docker/kivinenv/hello
Dockerfile:
```
FROM ubuntu:16.04

WORKDIR /usr/app
RUN apt-get update && apt-get install -y python3
COPY hello.py .

ENTRYPOINT ["python3", "./hello.py"]
```
### 1.16
Heroku link: https://devopswithdockerhello.herokuapp.com/

### 2.1
docker-compose file:
```
version: '3.5' 

services: 
    first_volume_exercise:  
      image: devopsdockeruh/first_volume_exercise
      volumes: 
      - ./logs.txt:/usr/app/logs.txt 
```
### 2.2
docker-compose file:
```
version: '3.5'  

services: 
    whoami: 
      image: devopsdockeruh/ports_exercise
      ports: 
        - 80:80 
```
```
```