FROM balenalib/armv7hf-alpine:3.10-build
LABEL io.balena.device-type="beaglebone-green-wifi"

RUN apk add --update \
		less \
		nano \
		net-tools \
		ifupdown \
		usbutils \
		gnupg \
	&& rm -rf /var/cache/apk/*