#
# Bitlbee (w/ Facebook and OTR) + Alpine Linux
#

FROM alpine:edge

RUN echo "@testing http://nl.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories && \
    apk --update add ca-certificates bitlbee@testing bitlbee-facebook@testing bitlbee-otr@testing && \
    adduser -h /var/lib/bitlbee -s /sbin/nologin -DH bitlbee && \
    chown -R bitlbee:bitlbee /var/lib/bitlbee && \
    touch /var/run/bitlbee.pid && \
    chown bitlbee:bitlbee /var/run/bitlbee.pid && \
    rm -rf /var/cache/apk/*

USER bitlbee
VOLUME ["/var/lib/bitlbee"]
EXPOSE 6667/tcp

ENTRYPOINT ["bitlbee", "-F", "-n"]
