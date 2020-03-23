# Based on the file created for Arch Linux by:
# Massimiliano Torromeo <massimiliano.torromeo@gmail.com>
# Bob Fanger < bfanger(at)gmail >
# Filip <fila pruda com>, Det < nimetonmaili(at)gmail >

# Maintainer: Philip Müller <philm@manjaro.org>
# Contributor: Helmut Stult <helmut[at]manjaro[dot]org>

_linuxprefix=linux56
_extramodules=extramodules-5.6-MANJARO
pkgname=$_linuxprefix-r8168
_pkgname=r8168
pkgver=8.048.00
pkgrel=1
pkgdesc="A kernel module for Realtek 8168 network cards"
url="http://www.realtek.com.tw"
license=("GPL")
arch=('i686' 'x86_64')
depends=('glibc' "$_linuxprefix")
makedepends=("$_linuxprefix-headers")
provides=("$_pkgname=$pkgver")
groups=("$_linuxprefix-extramodules")
source=("https://github.com/mtorromeo/r8168/archive/$pkgver/$_pkgname-$pkgver.tar.gz"
        'r8168-5_6_0.patch')
sha256sums=('0aacba20d985ba5e67e21bdad89a099e102f7bef3027adb647ffbb80b01ac8d0'
            '40ec11bccaaed8bc8b5152d13daeaaccdb766c375bc6538378970f21cf5a8d0b')

install=$_pkgname.install

prepare() {
    cd "$_pkgname-$pkgver"
#add 5.6 patch
    patch -Np1 -i "${srcdir}/r8168-5_6_0.patch"
}

build() {
    _kernver="$(cat /usr/lib/modules/$_extramodules/version || true)"

    cd "$_pkgname-$pkgver"

    # avoid using the Makefile directly -- it doesn't understand
    # any kernel but the current.

    make -C /usr/lib/modules/$_kernver/build \
      M="$srcdir/$_pkgname-$pkgver/src" \
      EXTRA_CFLAGS="-DCONFIG_R8168_NAPI -DCONFIG_R8168_VLAN" \
      modules
}

package() {
    cd "$srcdir/$_pkgname-$pkgver/src"
    install -D -m644 $_pkgname.ko "$pkgdir/usr/lib/modules/$_extramodules/$_pkgname.ko"

    # set the kernel we've built for inside the install script
    sed -i -e "s/EXTRAMODULES=.*/EXTRAMODULES=${_extramodules}/g" "${startdir}/${_pkgname}.install"

    find "$pkgdir" -name '*.ko' -exec gzip -9 {} \;
}
