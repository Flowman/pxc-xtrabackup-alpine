# Contributor: Peter Szalatnay <theotherland@gmail.com>

pkgname=percona-xtrabackup
pkgver=2.4.6
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

_builddir="$srcdir/percona-xtrabackup-percona-xtrabackup-$pkgver"
prepare() {
        local i
        cd "$_builddir"
        for i in $source; do
                case $i in
                *.patch) msg $i; patch -p1 -i "$srcdir"/$i || return 1;;
                esac
        done
}

build() {
        cd "$_builddir"

        local CFLAGS="-DSIGEV_THREAD_ID=4"

        cmake . -DBUILD_CONFIG=xtrabackup_release \
                -DCMAKE_INSTALL_PREFIX=/usr \
                -DWITH_MAN_PAGES=OFF \
                -DDOWNLOAD_BOOST=1 \
                -DWITH_BOOST="$_builddir"/libboost \
                || return 1

        make || return 1
}

package() {
        cd "$_builddir"
        make DESTDIR="$pkgdir/" install || return 1

        install -Dm644 COPYING "$pkgdir"/usr/share/licenses/$pkgname/COPYING || return 1
}

mytest() {
    pkgdesc="The test suite distributed with Percona XtraBackup"
    mkdir -p "$subpkgdir"/usr/bin || return 1
    mv "$pkgdir"/usr/xtrabackup-test \
        "$subpkgdir"/usr/bin/ \
        || return 1
}

