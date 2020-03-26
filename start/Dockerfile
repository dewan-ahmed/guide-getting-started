# FIrst stage

# our base build image
FROM maven:3.6-jdk-8 as builder

# copy the project files
COPY pom.xml .

# build all dependencies
RUN mvn -ntp dependency:go-offline

# copy your other files
COPY ./src ./src

# build for release
# RUN mvn -ntp liberty:install-server liberty:create package
# build the war file
RUN mvn package



# Second stage

FROM open-liberty

ARG VERSION=1.0
ARG REVISION=SNAPSHOT

LABEL \
  org.opencontainers.image.authors="Dewan Ahmed" \
  org.opencontainers.image.vendor="IBM" \
  org.opencontainers.image.url="local" \
  org.opencontainers.image.source="https://github.com/OpenLiberty/guide-getting-started" \
  org.opencontainers.image.version="$VERSION" \
  org.opencontainers.image.revision="$REVISION" \
  vendor="Open Liberty" \
  name="system" \
  version="$VERSION-$REVISION" \
  summary="The system microservice from the Getting Started guide" \
  description="This image contains the system microservice running with the Open Liberty runtime."

# Copy war file over from builder stage
ENV WAR_NAME=guide-getting-started.war

COPY --from=builder --chown=1001:0 src/main/liberty/config/ /config/
COPY --from=builder --chown=1001:0 target/$WAR_NAME /config/apps/

RUN configure.sh
