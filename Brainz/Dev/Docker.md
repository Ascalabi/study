# Docker
Docker is a container management service. The idea is for developers to easily develop applications, shipt them into containers which can be deployed anywhere

## Features of Docker
- Docker has the ability to reduce the size of development by providing a smaller footprint of the operating system via containers.

- With containers, it becomes easier for teams across different units, such as development, QA and Operations to work seamlessly across applications.

- You can deploy Docker containers anywhere, on any physical and virtual machines and even on the cloud.

- Since Docker containers are pretty lightweight, they are very easily scalable.


## Commands
run docker sql server idk man:
>docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=yourStrong(!)Password" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-CU13-ubuntu-20.04

Check running images
>docker ps

Check images
>docker images

Related to: [[Informacijski Sistemi]]
