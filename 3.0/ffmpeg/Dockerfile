ARG SRS_VERSION=3.0.52

FROM debian:buster

ARG SRS_VERSION
ENV SRS_VERSION=${SRS_VERSION}
ENV SRS_COMMIT=16050008172832d8dbe847940457e74e4f5669e4
ENV SRS_CONFIGURE_ARGS=--with-ffmpeg

RUN set -ex; \
    apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends wget ca-certificates sudo python net-tools apt-utils libfile-spec-native-perl; \
    cd /tmp; \
    wget https://github.com/ossrs/srs/archive/${SRS_COMMIT}.tar.gz; \
    tar zxf ${SRS_COMMIT}.tar.gz; \
    cd srs-${SRS_COMMIT}/trunk; \
    ./configure --prefix=/srs ${SRS_CONFIGURE_ARGS}; \
    make; \
    make install; \
    cp -HR objs/ffmpeg /srs/objs/; \
    rm -rf /tmp/*; \
    apt-get clean;
COPY ./srs.conf /srs/conf/docker.conf

FROM debian:buster-slim
ARG SRS_VERSION
ENV SRS_VERSION=${SRS_VERSION}
EXPOSE 1935 1985 8080
COPY --from=build /srs /srs
WORKDIR /srs
CMD ["./objs/srs", "-c", "./conf/docker.conf"]
LABEL Name="SRS_BUILD"
LABEL Version="0.0.1"