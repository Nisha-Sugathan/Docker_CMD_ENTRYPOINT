# Docker_CMD_ENTRYPOINT
The CMD and ENTRYPOINT specifies a command in Dockerfile , and that default program  will always be executed when the container starts


### Prerequisites
Need to install docker on your machine
Add your username to docker group

### Docker Installation

```
sudo yum install docker -y &> /dev/null
sudo systemctl restart docker.service
sudo systemctl enable docker.service
sudo usermod -a -G docker ec2-user
```
![docker_installation](https://github.com/Nisha-Sugathan/Docker-Bind_mounting/assets/134600837/ba7797c4-9a73-4ce6-b593-2befa5850e0d)

### Creating docker image with CMD
 cat Dockerfile 
```
FROM alpine:latest
CMD ["date"]
```
### Docker build
```
docker build -t first_image:cmd .
```

### Creating container with different programs

##### Default program
```
docker container run --name c1 first_image:cmd
```
##### sleep 5
```
 docker container run --name c2 first_image:cmd sleep 5
 docker container ls -a
CONTAINER ID   IMAGE         COMMAND     CREATED              STATUS                          PORTS     NAMES
4dac3aaf010a   myimage:cmd   "sleep 5"   17 seconds ago       Exited (0) 11 seconds ago                 c2
31ac3456d257   myimage:cmd   "date"      About a minute ago   Exited (0) About a minute ago             c1

```

![cms_ls](https://github.com/Nisha-Sugathan/Docker_CMD_ENTRYPOINT/assets/134600837/1aec3e0b-e6c5-48de-813e-be9338de87a6)


### Creating docker image with ENTRYPOINT
 cat Dockerfile 
 ```
FROM alpine:latest
ENTRYPOINT ["date"]

```
### Docker build 
```
docker build -t first_image:entry .
docker container run --name c3 first_image:entry 
```

### Error returning for other programs
```
docker container run --name c4 first_image:entry  sleep 5
```
The above command will return error because the sleep 5 will treat as argument of default program date
```
docker container run --name c5 first_image:entry  cat /etc/os-release
```
The above command will return error because the cat /etc/os-release would be passsed as argument of default program "date" given in the docker file.

![entry_cmd](https://github.com/Nisha-Sugathan/Docker_CMD_ENTRYPOINT/assets/134600837/02671cba-54dc-45a2-98a5-dac5230fda77)


### Returns output for date agruments
```
docker container run --name c6 first_image:entry +%F
2023-05-29
```

```
docker container run --name c7 first_image:entry +%D
```

![entry_success](https://github.com/Nisha-Sugathan/Docker_CMD_ENTRYPOINT/assets/134600837/04a51480-b210-4f25-a720-8f75617d05d4)

### Dockerfile using both CMD and ENTRY point
```
FROM alpine:latest
ENTRYPOINT ["date"]
CMD ["+%F"]
```
###create container for new dockerfile
```
docker build -t first_image:entry_cmd .

docker container run --name c9 first_image:entry_cmd

docker container run --name c10 first_image:entry_cmd +%D
```
