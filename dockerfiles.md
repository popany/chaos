# centos with oracle instant client

    FROM centos
    RUN yum install -y gcc-c++ \
        boost-devel \
        make \
        cmake

    ADD oracle-instantclient11.2-basic-11.2.0.4.0-1.x86_64.rpm /opt/oracle-instantclient11.2-basic.rpm
    ADD oracle-instantclient11.2-devel-11.2.0.4.0-1.x86_64.rpm /opt/oracle-instantclient11.2-devel.rpm
    ADD oracle-instantclient11.2-odbc-11.2.0.4.0-1.x86_64.rpm /opt/oracle-instantclient11.2-odbc.rpm

    RUN yum install -y /opt/oracle-instantclient11.2-basic.rpm \
        /opt/oracle-instantclient11.2-devel.rpm \
        /opt/oracle-instantclient11.2-odbc.rpm && \
        echo /usr/lib/oracle/11.2/client64/lib > \
        /etc/ld.so.conf.d/oracle-instantclient.conf && \
        ldconfig

    CMD ["/bin/bash"]
