ARG BUILD_FROM
FROM $BUILD_FROM
#FROM %%BASE_IMAGE%%
#FROM homeassistant/armhf-base:latest
#armhf:FROM bestlibre/armhf-debian-base:stretch
#aarch64:FROM bestlibre/aarch64-debian-base:stretch
#amd64:FROM debian:stretch
#i386:FROM i386/debian:stretch
#MAINTAINER Franck Nijhof <frenck@geekchimp.com>

# Add env
ENV LANG="C.UTF-8"
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# Copy run file
COPY files/run.sh /

# Setup base
RUN apk add --no-cache jq git python make g++ libffi-dev openssl-dev dbus \
      avahi-compat-libdns_sd avahi-dev avahi-tools avahi-ui-tools \
      nodejs-current yarn && \
    mkdir -p /var/run/dbus && \
    mkdir -p /var/run/avahi-daemon && \
    chown messagebus:messagebus /var/run/dbus && \
    chown avahi:avahi /var/run/avahi-daemon && \
    yarn global add npm && \
    yarn cache clean && \
    apk del yarn && \
    npm set unsafe-perm true && \
    npm -g install \
      node-gyp \
      homebridge \
      homebridge-homeassistant \
      homebridge-broadlink-rm \
      homebridge-mi-air-purifier miio \
      homebridge-xiaomi-mi-robot-vacuum \
      homebridge-camera-ffmpeg \
      homebridge-mi-aqara && \
    chmod a+x /run.sh

# Copy configuration files
COPY files/avahi-daemon.conf /etc/avahi/
COPY files/homebridge-config.json /root/

CMD [ "/run.sh" ]
