FROM debian:wheezy

MAINTAINER Tommy Lau <tommy@gen-new.com>

# Install the needed packages for OpenWrt
RUN apt-get update && apt-get install -y ca-certificates build-essential file gawk git-core libncurses5-dev libssl-dev python subversion unzip wget zlib1g-dev --no-install-recommends && rm -rf /var/lib/apt/lists/*

# Add user 'openwrt'
RUN useradd -m openwrt

# Get the source code
RUN su -c "cd ~ && git clone git://git.openwrt.org/14.07/openwrt.git" openwrt

# Install feeds
RUN su -c "cd ~/openwrt && ./scripts/feeds update -a" openwrt \
	&& su -c "cd ~/openwrt && ./scripts/feeds install -a" openwrt

# Install Shadowsocks
RUN SS_VERSION=`wget -O- "https://github.com/shadowsocks/shadowsocks-libev/releases/latest" | sed -n 's/^.*tag\/\(.*\)".*/\1/p'` \
	&& wget "https://github.com/shadowsocks/shadowsocks-libev/archive/$SS_VERSION.tar.gz" -O ss.tar.gz \
	&& mkdir -p /home/openwrt/openwrt/package/shadowsocks-libev \
	&& tar -xf ss.tar.gz -C /home/openwrt/openwrt/package/shadowsocks-libev --strip-components=1 \
	&& chown -R openwrt.openwrt /home/openwrt/openwrt/package/shadowsocks-libev \
	&& rm ss.tar.gz

# Make config
RUN su -c "cd ~/openwrt && make defconfig && make prereq" openwrt

WORKDIR /home/openwrt/openwrt

CMD ["/bin/su", "-c", "bash", "openwrt"]

