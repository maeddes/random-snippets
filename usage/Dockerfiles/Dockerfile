FROM gradle:6.7.0-jdk8 AS BUILDER

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash - \
    && apt-get install -y nodejs \
    && gradle -v \
    && npm --version

FROM BUILDER AS GRADLE

COPY ./ ./

RUN gradle build -PskipITs=true -Partifactory_user=mhaeussler@pp7.onmicrosoft.com -Partifactory_password=AKCp8hyPw3qM4tKDqMvYVVQJYyG2PgLY8DA1EvGuPBNAFf3inQB3rR6QWAMNURSvTS8FzK7Rj

FROM openjdk:8-jdk-alpine

VOLUME /config
VOLUME /logs
VOLUME /tmp
ARG JAR_FILE 
COPY --from=GRADLE ${JAR_FILE} webleasco.jar
RUN chmod 755 webleasco.jar
COPY webleasco-server/src/main/shell/* /
COPY webleasco-server/src/main/config/* /
EXPOSE 8080
EXPOSE 4000
ENV PROFILES ''
ENV DEBUG_PORT ''
ENV HEAP_SIZE 512M
ENV LEASCO_TTY_PREFIX ''
ENV LEASCO_CHARSET ''
ENV LOG_DIR /logs
ENV LOG_FILE /logs/webleasco.log
ENV JSON_LOG_FILE /logs/webleasco.json
ENV STAGE dev
ENV FILE_LOG_PATTERN='%d %c{15} %level %mdc{User} %mdc{TraceId:--} %mdc{SessionId:--} %mdc{LeascoBackend:--} %m%n'
ENV STDOUT_LOG_PATTERN='%d %c{15} %level %mdc{User} %mdc{TraceId:--} %mdc{SessionId:--} %mdc{LeascoBackend:--} %m%n'
ENV JSON_LOG_PATTERN='%d %c %level %mdc{User:--} %mdc{LeascoUser:--} %mdc{TraceId:--} %mdc{SessionId:--} %mdc{LeascoBackend:--} %mdc{LeascoTty:--} %m%n'
ENTRYPOINT ./webleasco.sh --server.port=8080
