ARG FROM_IMAGE="openjdk:8-jre-alpine"
FROM $FROM_IMAGE

RUN rm -rf /etc/localtime
RUN ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

ARG APP_NAME="sentinel-dashboard"
ARG CURR_TIME="00000000000000"
ENV CURR_TIME=$CURR_TIME
ENV APP_NAME=$APP_NAME
ENV JAVA_OPTS=""

RUN mkdir -p /poazy/app/${APP_NAME}
RUN mkdir -p /poazy/logs/${APP_NAME}

ADD ${APP_NAME}.jar /poazy/app/${APP_NAME}/${APP_NAME}.jar

WORKDIR /poazy/app/${APP_NAME}

VOLUME /poazy/app/${APP_NAME}
VOLUME /poazy/logs/${APP_NAME}

RUN touch $APP_NAME-v$CURR_TIME
ENV APP_VERSION=$CURR_TIME

EXPOSE 8080

ENTRYPOINT java -Djava.security.egd=file:/dev/./urandom ${JAVA_OPTS} -jar /poazy/app/${APP_NAME}/${APP_NAME}.jar