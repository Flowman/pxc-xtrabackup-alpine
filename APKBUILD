# Contributor: Peter Szalatnay <theotherland@gmail.com>
# Maintainer: Peter Szalatnay <theotherland@gmail.com>
pkgname=percona-xtrabackup
pkgver=8.0.22
_pkgver="$pkgver-15"
pkgrel=0
pkgdesc="Percona XtraBackup is a MySQL non-blocking backup solution for InnoDB and XtraDB data"
arch=all
url="http://www.percona.com/software/percona-xtrabackup/"
license="GPL2"
makedepends="
		bash
		bison
		cmake
		ncurses-dev
		libaio-dev
		libgcrypt-dev
		zlib-dev
		curl-dev
		libev-dev
		vim
		libcap-dev
		"
source="https://github.com/percona/percona-xtrabackup/archive/percona-xtrabackup-$_pkgver.tar.gz
		fix-dns_srv.patch
		"

subpackages="$pkgname-test:mytest"

builddir="$srcdir/percona-xtrabackup-percona-xtrabackup-$_pkgver"

# Notes:
# PXC require boost 1.73 or it will now compile, alpine never have the right version required to build
# man pages requires python-sphinx
build() {
	cd "$builddir"

	#local CFLAGS="-DSIGEV_THREAD_ID=4"

	cmake . -DBUILD_CONFIG=xtrabackup_release \
			-DCMAKE_INSTALL_PREFIX=/usr \
			-DWITH_MAN_PAGES=OFF \
			-DDOWNLOAD_BOOST=ON \
			-DWITH_BOOST="$builddir"/libboost \
			-DFORCE_INSOURCE_BUILD=ON

	# print config options to log
	cmake -L

	make
}

check() {
	cd "$builddir"
	make test
}

package() {
	cd "$builddir"
	make DESTDIR="$pkgdir/" install
}

mytest() {
	pkgdesc="The test suite distributed with Percona XtraBackup"
	mkdir -p "$subpkgdir"/usr/bin
	mv "$pkgdir"/usr/xtrabackup-test \
		"$subpkgdir"/usr/bin/
}
