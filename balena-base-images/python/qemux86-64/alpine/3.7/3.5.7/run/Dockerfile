# AUTOGENERATED FILE
FROM balenalib/qemux86-64-alpine:3.7-run

# remove several traces of python
RUN apk del python*

# http://bugs.python.org/issue19846
# > At the moment, setting "LANG=C" on a Linux system *fundamentally breaks Python 3*, and that's not OK.
ENV LANG C.UTF-8

# install python dependencies
RUN apk add --no-cache ca-certificates libffi \
	&& apk add --no-cache libssl1.0 || apk add --no-cache libssl1.1

# key 63C7CC90: public key "Simon McVittie <smcv@pseudorandom.co.uk>" imported
# key 3372DCFA: public key "Donald Stufft (dstufft) <donald@stufft.io>" imported
RUN gpg --keyserver keyring.debian.org --recv-keys 4DE8FF2A63C7CC90 \
	&& gpg --keyserver keyserver.ubuntu.com --recv-key 6E3CBCE93372DCFA \
	&& gpg --keyserver keyserver.ubuntu.com --recv-keys 0x52a43a1e4b77b059

# point Python at a system-provided certificate database. Otherwise, we might hit CERTIFICATE_VERIFY_FAILED.
# https://www.python.org/dev/peps/pep-0476/#trust-database
ENV SSL_CERT_FILE /etc/ssl/certs/ca-certificates.crt

ENV PYTHON_VERSION 3.5.7

# if this is called "PIP_VERSION", pip explodes with "ValueError: invalid truth value '<VERSION>'"
ENV PYTHON_PIP_VERSION 19.3.1

ENV SETUPTOOLS_VERSION 41.6.0

RUN set -x \
	&& buildDeps=' \
		curl \
		gnupg \
	' \
	&& apk add --no-cache --virtual .build-deps $buildDeps \
	&& curl -SLO "http://resin-packages.s3.amazonaws.com/python/v$PYTHON_VERSION/Python-$PYTHON_VERSION.linux-alpine-amd64-openssl1.0.tar.gz" \
	&& echo "5952cf58963282b9ba67b38db696600a871d9ca0693e5c9e76bec507221382d4  Python-$PYTHON_VERSION.linux-alpine-amd64-openssl1.0.tar.gz" | sha256sum -c - \
	&& tar -xzf "Python-$PYTHON_VERSION.linux-alpine-amd64-openssl1.0.tar.gz" --strip-components=1 \
	&& rm -rf "Python-$PYTHON_VERSION.linux-alpine-amd64-openssl1.0.tar.gz" \
	&& if [ ! -e /usr/local/bin/pip3 ]; then : \
		&& curl -SLO "https://raw.githubusercontent.com/pypa/get-pip/430ba37776ae2ad89f794c7a43b90dc23bac334c/get-pip.py" \
		&& echo "19dae841a150c86e2a09d475b5eb0602861f2a5b7761ec268049a662dbd2bd0c  get-pip.py" | sha256sum -c - \
		&& python3 get-pip.py \
		&& rm get-pip.py \
	; fi \
	&& pip3 install --no-cache-dir --upgrade --force-reinstall pip=="$PYTHON_PIP_VERSION" setuptools=="$SETUPTOOLS_VERSION" \
	&& find /usr/local \
		\( -type d -a -name test -o -name tests \) \
		-o \( -type f -a -name '*.pyc' -o -name '*.pyo' \) \
		-exec rm -rf '{}' + \
	&& cd / \
	&& rm -rf /usr/src/python ~/.cache

# make some useful symlinks that are expected to exist
RUN cd /usr/local/bin \
	&& ln -sf pip3 pip \
	&& { [ -e easy_install ] || ln -s easy_install-* easy_install; } \
	&& ln -sf idle3 idle \
	&& ln -sf pydoc3 pydoc \
	&& ln -sf python3 python \
	&& ln -sf python3-config python-config

CMD ["echo","'No CMD command was set in Dockerfile! Details about CMD command could be found in Dockerfile Guide section in our Docs. Here's the link: https://balena.io/docs"]

 RUN curl -SLO "https://raw.githubusercontent.com/balena-io-library/base-images/8accad6af708fca7271c5c65f18a86782e19f877/scripts/assets/tests/test-stack@python.sh" \
  && echo "Running test-stack@python" \
  && chmod +x test-stack@python.sh \
  && bash test-stack@python.sh \
  && rm -rf test-stack@python.sh