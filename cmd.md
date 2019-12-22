# cmd

## linux

### centos

check centos veison

    $ rpm -q centos-release
    centos-release-8.0-0.1905.0.9.el8.x86_64

or

    $ cat /etc/centos-release
    CentOS Linux release 8.0.1905 (Core)

## ubuntu

check boost version

    $ dpkg -s libboost-dev | grep 'Version'

## docker

### Start a container with a bind mount

    $ docker run -d \
      -it \
      --name devtest \
      --mount type=bind,source="$(pwd)"/target,target=/app \
      nginx:latest
