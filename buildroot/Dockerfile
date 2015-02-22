FROM debian:wheezy

MAINTAINER Tommy Lau <tommy@gen-new.com>

# Install the needed packages for OpenWrt
RUN apt-get update && apt-get install -y ca-certificates build-essential file git-core libssl-dev python subversion unzip wget --no-install-recommends && rm -rf /var/lib/apt/lists/*

# Add user 'openwrt'
RUN useradd -m openwrt

# Get the source code
RUN su -c "cd ~ && git clone git://git.openwrt.org/14.07/openwrt.git" openwrt

# Install feeds
RUN su -c "cd ~/openwrt && ./scripts/feeds update -a" openwrt \
	&& su -c "cd ~/openwrt && ./scripts/feeds install -a" openwrt

ENTRYPOINT ["/bin/bash"]

