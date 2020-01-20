# cmd

## linux

### common

convert timestamps to date

    $ date -d @1577165662
    Tue Dec 24 13:34:22 CST 2019

    $ date -d @1577165662 '+%Y-%m-%d %H:%M:%S'
    2019-12-24 13:34:22

    $ date -d '2019-12-24 13:34:22 CST' +%s
    1577165662

### centos

check centos veison

    $ rpm -q centos-release
    centos-release-8.0-0.1905.0.9.el8.x86_64

or

    $ cat /etc/centos-release
    CentOS Linux release 8.0.1905 (Core)

### ubuntu

check boost version

    $ dpkg -s libboost-dev | grep 'Version'

## windows

### cmd

Changes the active console code page

    chcp [<NNN>]

### PowerShell

Gets ODBC DSNs

    Get-OdbcDsn

## docker

### Start a container with a bind mount

    $ docker run -d \
      -it \
      --name devtest \
      --mount type=bind,source="$(pwd)"/target,target=/app \
      nginx:latest

## Elasticsearch

### [Get index API](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-get-index.html)

    curl -X GET host_name:9200/_all|jq

### [Create index API](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html)

    curl -X PUT host_name:9200/index_name

### [Put mapping API](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-put-mapping.html)

### [Get mapping API](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-get-mapping.html)

### [Request Body Search](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-request-body.html)
