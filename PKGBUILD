# Maintainer: Chocobo1

pkgname=iwlwifi
pkgver=2018.05.30.r0.g50624782
pkgrel=1
pkgdesc="Intel wireless chips driver (git tag)"
arch=('i686' 'x86_64')
url="https://wireless.wiki.kernel.org/en/users/drivers/iwlwifi"
license=('GPL')
makedepends=('git' 'linux-headers' 'curl' 'xz')
#source=('git+https://git.kernel.org/pub/scm/linux/kernel/git/iwlwifi/iwlwifi-fixes.git#commit=70f46c1438bf8342578d2b34e465ab352ccae357')
#sha256sums=('SKIP')
#validpgpkeys=('ABAF11C65A2970B130ABE3C479BE3E4300411886'  # Linus Torvalds
#              '647F28654894E3BD457199BE38DBBDC86092693E') # Greg Kroah-Hartman


_gittag="iwlwifi-next-for-kalle-2018-05-30"
_moduleSrc="iwlwifi-next/drivers/net/wireless/intel/iwlwifi"

prepare() {
  cd "$srcdir"
  if [ ! -d "iwlwifi-next" ]; then
    git clone "https://kernel.googlesource.com/pub/scm/linux/kernel/git/iwlwifi/iwlwifi-next.git" --branch "$_gittag" --depth 1
  fi

  cd "$srcdir/iwlwifi-next"
  git checkout tags/"$_gittag"

  # patches
  # dynamic queue allocation (DQA) is not ready for production use
  #curl -s "https://git.kernel.org/pub/scm/linux/kernel/git/iwlwifi/iwlwifi-fixes.git/patch/?id=7948b87308a489c2caa23574ea3c72298288c374" | git apply - --reverse
}

pkgver() {
  cd "$srcdir/$_moduleSrc"

  _tag=$(sed 's/iwlwifi-next-for-kalle-//;s/-/./g' <<< "$_gittag")
  _hash=$(git rev-parse --short HEAD)
  _rev=$(git rev-list --count "$_hash"..HEAD)
  printf "%s.r%s.g%s" "$_tag" "$_rev" "$_hash"
}

build() {
  make -C "/usr/lib/modules/$(uname -r)/build" M="$srcdir/$_moduleSrc"
}

package() {
  cd "$srcdir/$_moduleSrc"

  find './' -name '*.ko' -exec xz -0 --force {} \;

  _updates="/usr/lib/modules/$(uname -r)/updates"

  install -Dm644 'iwlwifi.ko.xz' "$pkgdir/$_updates/iwlwifi.ko.xz"

  pushd "dvm"
  install -Dm644 'iwldvm.ko.xz' "$pkgdir/$_updates/iwldvm.ko.xz"
  popd

  pushd "mvm"
  install -Dm644 'iwlmvm.ko.xz' "$pkgdir/$_updates/iwlmvm.ko.xz"
  popd
}
