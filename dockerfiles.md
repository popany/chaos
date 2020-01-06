# centos with oracle instant client
Excemple A

    FROM centos
    RUN yum install -y gcc-c++ \
        boost-devel \
        make \
        cmake \
        libaio \
        unixODBC \
        unixODBC-devel

    ADD oracle-instantclient11.2-basic-11.2.0.4.0-1.x86_64.rpm /opt/oracle-instantclient11.2-basic.rpm
    ADD oracle-instantclient11.2-devel-11.2.0.4.0-1.x86_64.rpm /opt/oracle-instantclient11.2-devel.rpm
    ADD oracle-instantclient11.2-odbc-11.2.0.4.0-1.x86_64.rpm /opt/oracle-instantclient11.2-odbc.rpm

    RUN yum install -y /opt/oracle-instantclient11.2-basic.rpm \
        /opt/oracle-instantclient11.2-devel.rpm \
        /opt/oracle-instantclient11.2-odbc.rpm && \
        echo /usr/lib/oracle/11.2/client64/lib > /etc/ld.so.conf.d/oracle-instantclient.conf && \
        ldconfig && \
        cd /usr/share/oracle/11.2/client64 && \
        ./odbc_update_ini.sh

    ENV ORACLE_INCLUDE=/usr/include/oracle/11.2/client64
    ENV ORACLE_LIB=/usr/lib/oracle/11.2/client64/lib

    CMD ["/bin/bash"]

Excemple B

    FROM centos6.10:0
    RUN yum install -y gcc-c++ \
        make \
        unixODBC \
        unixODBC-devel
    
    ADD oracle-instantclient11.2-basic-11.2.0.4.0-1.x86_64.rpm /opt/oracle-instantclient11.2-basic.rpm
    ADD oracle-instantclient11.2-devel-11.2.0.4.0-1.x86_64.rpm /opt/oracle-instantclient11.2-devel.rpm
    ADD oracle-instantclient11.2-odbc-11.2.0.4.0-1.x86_64.rpm /opt/oracle-instantclient11.2-odbc.rpm
    
    RUN yum install -y /opt/oracle-instantclient11.2-basic.rpm \
        /opt/oracle-instantclient11.2-devel.rpm \
        /opt/oracle-instantclient11.2-odbc.rpm && \
        echo /usr/lib/oracle/11.2/client64/lib > /etc/ld.so.conf.d/oracle-instantclient.conf && \
        ldconfig && \
        cd /usr/share/oracle/11.2/client64 && \
        ./odbc_update_ini.sh
    
    ENV ORACLE_INCLUDE=/usr/include/oracle/11.2/client64
    ENV ORACLE_LIB=/usr/lib/oracle/11.2/client64/lib
    
    ADD boost_1_66_0_build-lib.tar /
    ADD boost_1_66_0_build-include.tar /
    
    RUN echo /usr/local/lib > /etc/ld.so.conf.d/usr-local-lib.conf && \
        ldconfig
    
    ADD  cmake-3.16.2-Linux-x86_64.tar.gz /opt/
    ENV PATH=$PATH:/opt/cmake-3.16.2-Linux-x86_64/bin
    
    CMD ["/bin/bash"]
