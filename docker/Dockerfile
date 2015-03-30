FROM phusion/baseimage:0.9.16
MAINTAINER Ronald Rink <ronald.rink@d-fens.net>

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && \
    apt-get install -y curl ntp ntpdate tzdata && \
    curl -O -L https://packages.graylog2.org/releases/graylog2-omnibus/ubuntu/graylog_latest.deb && \
    dpkg -i graylog_latest.deb && \
    rm graylog_latest.deb && \
    sed -i "0,/^\s*$/s//\/opt\/graylog\/embedded\/share\/docker\/run_graylogctl\n/" /etc/rc.local && \
    sed -i "0,/^\s*$/s//tail\ \-F\ \/var\/log\/graylog\/server\/current\ \&\n/" /etc/rc.local && \
    apt-get clean && \
    rm -rf /tmp/* /var/tmp/*

# run graylog-server with debugger
# RUN sed -i 's/\(GRAYLOG_SERVER_JAVA_OPTS=".*\)"/\1 -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005"/' /opt/graylog/embedded/cookbooks/graylog/templates/default/sv-graylog-server-run.erb

VOLUME /var/opt/graylog/data
VOLUME /var/log/graylog
VOLUME /opt/graylog/plugin

# web interface
EXPOSE 9000
# gelf tcp
EXPOSE 12201
# gelf udp
EXPOSE 12201/udp
# rest api
EXPOSE 12900
# etcd
EXPOSE 4001
# syslog
EXPOSE 514
EXPOSE 514/udp

CMD ["/opt/graylog/embedded/share/docker/my_init"]
