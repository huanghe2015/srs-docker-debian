ARG SRS_VERSION=2.0.263
FROM debian:buster AS build
ARG SRS_VERSION
ENV SRS_VERSION=${SRS_VERSION}
ENV SRS_COMMIT=6e6e996bbaee73473f2ace76b7cb45a065368e7c
ENV SRS_CONFIGURE_ARGS=
RUN set -ex; \
    apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends wget ca-certificates sudo python net-tools apt-utils; \
    cd /tmp; \
    wget https://github.com/ossrs/srs/archive/${SRS_COMMIT}.tar.gz; \
    tar zxf ${SRS_COMMIT}.tar.gz; \
    cd srs-${SRS_COMMIT}/trunk; \
    ./configure --prefix=/srs ${SRS_CONFIGURE_ARGS}; \
    make; \
    make install; \
    rm -rf /tmp/*; \
    apt-get clean;
COPY ./srs.conf /srs/conf/docker.conf
# Real Content
FROM debian:buster-slim AS dist
ARG SRS_VERSION
ENV SRS_VERSION=${SRS_VERSION}
EXPOSE 1935 1985 8080
COPY --from=build /srs /srs
WORKDIR /srs
CMD ["./objs/srs", "-c", "./conf/docker.conf"]
LABEL Name="SRS_BUILD"
LABEL Version="0.0.2"