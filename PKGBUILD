pkgname=niri-center
pkgver=26.04
pkgrel=2
pkgdesc='Niri 26.04 with single-window vertical centering and portrait column fill'
arch=('x86_64')
url='https://github.com/YaLTeR/niri'
license=('GPL-3.0-or-later')
depends=(
  'cairo'
  'glib2'
  'glibc'
  'libdisplay-info'
  'libgcc'
  'libinput'
  'libpipewire'
  'libxkbcommon'
  'mesa'
  'pango'
  'pixman'
  'seatd'
  'systemd-libs'
  'xdg-desktop-portal-impl'
)
makedepends=(
  'clang'
  'git'
  'rust'
)

_commit='8ed0da44d974c32c6877d2f4630c314da0717ecb'
source=(
  "niri-${_commit}.tar.gz::https://github.com/YaLTeR/niri/archive/${_commit}.tar.gz"
  "82f59e2.patch::file://${startdir}/patches/82f59e2.patch"
  "portrait-fill-current-column.patch::file://${startdir}/patches/portrait-fill-current-column.patch"
)
sha256sums=(
  'b065863db54ee44ea64c5019d70655b02f04a252966779f325600e7f953906d0'
  'dce42127c54bd7f22c641dcbd0260dc5f18d34f65d83f645f2ba709cb2c66afc'
  '8031d7956bcbe21391a0404189683a441a3391c4db7d6356b5fb734037099202'
)

prepare() {
  cd "niri-${_commit}"
  patch --no-backup-if-mismatch -Np1 -i "${srcdir}/82f59e2.patch"
  patch --no-backup-if-mismatch -Np1 -i "${srcdir}/portrait-fill-current-column.patch"
}

build() {
  cd "niri-${_commit}"
  CFLAGS+=(' -ffat-lto-objects')
  cargo build --release --locked
}

check() {
  cd "niri-${_commit}"
  cargo test --locked --lib portrait_fill_current_column
}

package() {
  cd "niri-${_commit}"
  install -Dm755 target/release/niri "${pkgdir}/usr/bin/niri-center"
}
