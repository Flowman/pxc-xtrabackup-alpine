# Contributor: Peter Szalatnay <theotherland@gmail.com>
# Maintainer: Peter Szalatnay <theotherland@gmail.com>
pkgname=percona-xtrabackup
pkgver=2.4.13
pkgrel=0
pkgdesc='Percona XtraBackup is a MySQL non-blocking backup solution for InnoDB and XtraDB data'
arch=all
url='http://www.percona.com/software/percona-xtrabackup/'
license='GPL'
makedepends='bash bison cmake ncurses-dev libaio-dev libgcrypt-dev zlib-dev curl-dev libev-dev vim'
source="https://github.com/percona/percona-xtrabackup/archive/percona-xtrabackup-$pkgver.tar.gz
        fix-posix_timers.patch
        "

subpackages="$pkgname-doc $pkgname-test:mytest
        "

builddir="$srcdir/percona-xtrabackup-percona-xtrabackup-$pkgver"

# Notes:
# PXC require boost 1.59 or it will now compile, alpine edge uses boost 1.66 hence using the download flag
build() {
        cd "$builddir"

        local CFLAGS="-DSIGEV_THREAD_ID=4"

        cmake . -DBUILD_CONFIG=xtrabackup_release \
                -DCMAKE_INSTALL_PREFIX=/usr \
                -DWITH_MAN_PAGES=OFF \
                -DDOWNLOAD_BOOST=1 \
                -DWITH_BOOST="$builddir"/libboost

        make
}

check() {
    cd "$builddir"
    make test
}

package() {
        cd "$builddir"
        make DESTDIR="$pkgdir/" install

        install -Dm644 COPYING "$pkgdir"/usr/share/licenses/$pkgname/COPYING
}

mytest() {
    pkgdesc="The test suite distributed with Percona XtraBackup"
    mkdir -p "$subpkgdir"/usr/bin
    mv "$pkgdir"/usr/xtrabackup-test \
        "$subpkgdir"/usr/bin/
}
