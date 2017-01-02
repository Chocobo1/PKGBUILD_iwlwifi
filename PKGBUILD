# Maintainer: Chocobo1

pkgname=iwlwifi
pkgver=4.8.10
pkgrel=1
pkgdesc="Wireless driver for Intel's current wireless chips (Kernel stable)"
arch=('i686' 'x86_64')
url="https://wireless.wiki.kernel.org/en/users/drivers/iwlwifi"
license=('GPL')
makedepends=('linux-headers')
install=$pkgname.install
source=("https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-$pkgver.tar.xz"
        "https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-$pkgver.tar.sign")
sha256sums=('a340eb15d20bb8cd69361804e6e3e3b5c55b7cd9f66ab14724ff0f2a7bf370c4'
            'SKIP')
validpgpkeys=('ABAF11C65A2970B130ABE3C479BE3E4300411886'  # Linus Torvalds
              '647F28654894E3BD457199BE38DBBDC86092693E') # Greg Kroah-Hartman


_moduleSrc="linux-$pkgver/drivers/net/wireless/intel/iwlwifi"

build() {
    make -C "/usr/lib/modules/$(uname -r)/build" M="$srcdir/$_moduleSrc"
}

package() {
    cd "$srcdir/$_moduleSrc"

    find './' -name '*.ko' -exec gzip --force --fast {} \;

    _kernver=$(pacman -Q linux | grep -Po '(?<= )\d+\.\d+')
    _extramodules="/usr/lib/modules/extramodules-$_kernver-ARCH"

    install -Dm644 'iwlwifi.ko.gz' "$pkgdir/$_extramodules/iwlwifi.ko.gz"

    cd "dvm"
    install -Dm644 'iwldvm.ko.gz' "$pkgdir/$_extramodules/iwldvm.ko.gz"
    cd ..

    cd "mvm"
    install -Dm644 'iwlmvm.ko.gz' "$pkgdir/$_extramodules/iwlmvm.ko.gz"
    cd ..
}
