FROM openjdk:11-jre-slim
ENV DOCKERIZE_VERSION v0.7.0
ENV JAVA_OPTS="-Djava.awt.headless=true -Xmx256m -Xms256m -XX:+UseConcMarkSweepGC -Dlog4j2.formatMsgNoLookups=true"
ENV MYSQL_HOST="living-atlas-mysql"
ENV SERVER_PORT="8080"
RUN apt-get update \
    && apt-get install -y wget \
    && wget -O - https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz | tar xzf - -C /usr/local/bin \
    && apt-get autoremove -yqq --purge wget && rm -rf /var/lib/apt/lists/*
COPY ./build/libs/collectory-*.war app.war
ENTRYPOINT dockerize -wait tcp://$MYSQL_HOST:3306 -timeout 120s sh -c "java $JAVA_OPTS -jar app.war"
EXPOSE $SERVER_PORT