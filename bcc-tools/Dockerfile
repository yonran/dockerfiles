# Usage:
# docker run --rm -it \
#	--privileged \
#	-v /lib/modules:/lib/modules:ro \
#	-v /usr/src:/usr/src:ro \
#	-v /etc/localtime:/etc/localtime:ro \
#	r.j3ss.co/bcc-tools
#
FROM debian:buster-slim
MAINTAINER Jessica Frazelle <jess@linux.com>

ENV PATH /usr/share/bcc/tools:$PATH

# Add non-free apt sources
RUN sed -i "s#deb http://deb.debian.org/debian buster main#deb http://deb.debian.org/debian buster main contrib non-free#g" /etc/apt/sources.list

RUN apt-get update && apt-get upgrade -y && apt-get install -y \
    ca-certificates \
	clang \
	curl \
	git \
	vim \
	rsync \
	# Install dependencies for libbcc
	# FROM: https://github.com/iovisor/bcc/blob/master/INSTALL.md#install-build-dependencies
	debhelper \
	cmake \
	# libllvm \
	llvm-dev \
	libclang-dev \
	libelf-dev \
	bison \
	flex \
	libedit-dev \
	clang-format \
	python \
	python-netaddr \
	python3-pyroute2 \
	luajit \
	libluajit-5.1-dev \
	arping \
	iperf \
	ethtool \
	devscripts \
	zlib1g-dev \
	libfl-dev \
    --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*

# Build libbcc
ENV BCC_VERSION v0.13.0
RUN git clone --depth 1 --branch "$BCC_VERSION" https://github.com/iovisor/bcc.git /usr/src/bcc \
	&& ( \
		cd /usr/src/bcc \
		&& mkdir build \
		&& cd build \
		&& cmake .. -DCMAKE_INSTALL_PREFIX=/usr \
		&& make \
		&& make install \
	) \
	&& rm -rf /usr/src/bcc

COPY entrypoint.sh /usr/local/bin/entrypoint.sh

ENTRYPOINT ["entrypoint.sh"]
