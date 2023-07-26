FROM livingatlases/java-11-base:1.0.0

# Default values
ARG WAR_FILE=./build/libs/collectory-*.war
ARG APP_ARTIFACT=collectory
ARG USER=collectory

# Env variables
ENV WAR_FILE_ENV=${WAR_FILE}
ENV JAVA_OPTS="-Djava.awt.headless=true -Xmx256m -Xms256m -XX:+UseConcMarkSweepGC -Dlog4j2.formatMsgNoLookups=true"
ENV MYSQL_HOST="living-atlas-collectory-mysql"
ENV SERVER_PORT="8080"

# Directories and perms
RUN mkdir -p /data/$APP_ARTIFACT && \
    groupadd -r $USER -g 1000 && useradd -r -g $USER -u 1000 -m $USER && \
    chown -R $USER:$USER /data/$APP_ARTIFACT
WORKDIR /opt/atlas/$APP_ARTIFACT

# war
COPY $WAR_FILE_ENV app.war

EXPOSE $SERVER_PORT
# Lint with:
# docker run --rm -i hadolint/hadolint < Dockerfile
# hadolint ignore=DL3025
ENTRYPOINT dockerize -wait tcp://$MYSQL_HOST:3306 -timeout 120s sh -c "java $JAVA_OPTS -jar app.war"
