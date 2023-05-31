# docker build -t "nifi-example" ./
# docker run -p 8080:8080 nifi-example

from apache/nifi:latest

ADD --chown=nifi:nifi ./reporting-task-nar/target/*.nar lib/

RUN sed -i -e 's/#java\.arg\.debug/java\.arg\.debug/g' conf/bootstrap.conf
