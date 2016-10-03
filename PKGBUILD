pkgname=collectd-stackdriver-agent
pkgver=5.5.2
pkgrel=1
pkgdesc='Daemon which collects system performance statistics periodically'
url='http://github.com/Stackdriver/collectd'
arch=('i686' 'x86_64')
license=('GPL')
conflicts=('collectd')

optdepends=('curl: apache, ascent, curl, nginx, and write_http plugins'
            'libdbi: dbi plugin'
            'libesmtp: notify_email plugin'
            'libgcrypt: encryption and authentication for network plugin'
            'libmemcached: memcachec plugin'
            'libmariadbclient: mysql plugin'
            'iproute2: netlink plugin'
            'net-snmp: snmp plugin'
            'libnotify: notify_desktop plugin'
            'openipmi: ipmi plugin'
            'liboping: ping plugin'
            'libpcap: dns plugin'
            'perl: perl plugin'
            'postgresql-libs: postgresql plugin'
            'python2: python plugin'
            'rrdtool: rrdtool and rrdcached plugins'
            'lm_sensors: lm_sensors and sensors plugins'
            'libvirt: libvirt plugin'
            'libxml2: ascent and libvirt plugins'
            'yajl: curl_json plugin'
            'libatasmart: smart plugin'
            'lvm2: lvm plugin'
            'protobuf-c: write_riemann plugin')

makedepends=('curl' 'libdbi' 'libesmtp' 'libgcrypt' 'libmemcached'
             'libmariadbclient' 'iproute2' 'net-snmp' 'libnotify' 'openipmi'
             'liboping' 'libpcap' 'postgresql-libs' 'python2' 'rrdtool'
             'lm_sensors' 'libvirt' 'libxml2' 'yajl' 'libatasmart' 'lvm2'
             'protobuf-c')

depends=('libltdl' 'iptables')

source=('https://github.com/Stackdriver/collectd/archive/stackdriver-agent-5.5.2.zip')
sha256sums=('7fdea24bdcb7e3c593b7ece002c7fb74db5bd7bd41bb20ef73ce5c23fbaf0298')

backup=('etc/collectd.conf')

prepare() {
        cd "${srcdir}/${pkgname}-${pkgver}"
	sed 's/-Werror//g' -i *.ac */*.{am,in} */*/*.{am,in}
	./build.sh
}

build() {
	cd "${srcdir}/${pkgname}-${pkgver}"
	export MAKEFLAGS='-j1'
	./configure \
		--prefix=/usr \
		--sysconfdir=/etc \
		--localstatedir=/var \
		--sbindir=/usr/bin \
		--with-python=/usr/bin/python2 \
		--with-perl-bindings='INSTALLDIRS=vendor'
	make all
}

package() {
	cd "${srcdir}/${pkgname}-${pkgver}"
	make DESTDIR="${pkgdir}" install
	rmdir "${pkgdir}/var/run" # FS#30201
	install -Dm644 ../service "${pkgdir}"/usr/lib/systemd/system/collectd.service
	install -Dm644 contrib/collectd2html.pl "${pkgdir}"/usr/share/collectd/collectd2html.pl
}
