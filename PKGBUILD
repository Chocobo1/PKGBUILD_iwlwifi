# Maintainer: Chocobo1

pkgname=iwlwifi
pkgver=03c902bff
pkgrel=1
epoch=1
pkgdesc="Wireless driver for Intel's current wireless chips (git tag)"
arch=('i686' 'x86_64')
url="https://wireless.wiki.kernel.org/en/users/drivers/iwlwifi"
license=('GPL')
makedepends=('linux-headers' 'curl')
install=$pkgname.install
#source=('git+https://git.kernel.org/pub/scm/linux/kernel/git/iwlwifi/iwlwifi-fixes.git#commit=70f46c1438bf8342578d2b34e465ab352ccae357')
#sha256sums=('SKIP')
#validpgpkeys=('ABAF11C65A2970B130ABE3C479BE3E4300411886'  # Linus Torvalds
#              '647F28654894E3BD457199BE38DBBDC86092693E') # Greg Kroah-Hartman


_moduleSrc="iwlwifi-fixes/drivers/net/wireless/intel/iwlwifi"

prepare() {
  cd "$srcdir"
  if [ ! -d "iwlwifi-fixes" ]; then
    git clone "https://kernel.googlesource.com/pub/scm/linux/kernel/git/iwlwifi/iwlwifi-fixes.git" --depth=1 --no-checkout
  fi

  cd "$srcdir/iwlwifi-fixes"
  git fetch origin "tags/iwlwifi-for-kalle-2017-01-23" --depth 1
  git reset --hard "FETCH_HEAD"

  # dynamic queue allocation (DQA) is not ready for production use
  curl -s "https://git.kernel.org/pub/scm/linux/kernel/git/iwlwifi/iwlwifi-fixes.git/patch/?id=7948b87308a489c2caa23574ea3c72298288c374" | git apply - --reverse
}

pkgver() {
  cd "$srcdir/$_moduleSrc"
  git describe --long --tags --always | sed 's/.*-\([^-]*-[^-]*$\)/r\1/;s/-/./g'
}

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
