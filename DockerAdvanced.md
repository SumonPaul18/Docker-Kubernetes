# Docker Container Advanced Configuration

SSH into Docker Container 

Refference:https://www.linkedin.com/pulse/ssh-docker-container-centos-yashwanth-medisetti

<b>Error:</b>

Failed to get D-Bus connection: Operation not permitted

<b>Solution:</b>

Run the container in privileged mode.

docker run -dit --name (container_name) --privileged centos /usr/sbin/init

    docker run -d --name centos7 --privileged=true centos:7 /usr/sbin/init
####
    docker exec -it centos7 /bin/bash

Now working systemctl command on the container.

how to create a Docker container with SSH access. 

Refference:https://www.techrepublic.com/article/deploy-docker-container-ssh-access/

Create a Dockerfile

    nano Dockerfile

In that file, paste the following:

    FROM ubuntu:20.04
    RUN apt-get update && apt-get install -y openssh-server
    RUN mkdir /var/run/sshd
    RUN echo 'root:PASSWORD' | chpasswd
    RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
    RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
    EXPOSE 22
    CMD ["/usr/sbin/sshd", "-D"]

Build the dockerfile

    sudo docker build -t sshd_ubuntu .

Run the container

    docker run -d -P --name test_sshd sshd_ubuntu

How to locate the IP address of the running container

    docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' test_sshd

How to SSH into the running container

    ssh root@IP

#

