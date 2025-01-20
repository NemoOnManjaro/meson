# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Contributor: Anatol Pomozov <anatol dot pomozov at gmail>

pkgname=meson
pkgver=1.7.0rc1
pkgrel=1
pkgdesc="High productivity build system"
url="https://mesonbuild.com/"
arch=(any)
license=(Apache-2.0)
depends=(
  bash
  ninja
  python
  python-tqdm
)
makedepends=(
  python-build
  python-installer
  python-setuptools
  python-wheel
)
checkdepends=(
  boost
  clang
  cmake
  cuda
  cython
  doxygen
  gcc-fortran
  gcc-objc
  git
  glib2-devel
  glibc-locales
  gmock
  gnustep-base
  gobject-introspection
  graphviz
  gtest
  gtk-doc
  gtk-sharp-2
  gtk3
  gtkmm3
  hotdoc
  itstool
  java-environment=8
  ldc
  libelf
  libwmf
  llvm
  mercurial
  mono
  nasm
  netcdf-fortran
  openmpi
  openssh
  protobuf
  python-gobject
  python-pytest-xdist
  qt5-base
  qt5-tools
  rust
  rust-bindgen
  sdl2
  vala
  valgrind
  vulkan-validation-layers
  wayland-protocols
  wxgtk3
)
source=(
  https://github.com/mesonbuild/meson/releases/download/$pkgver/meson-$pkgver.tar.gz{,.asc}
  meson-reference-$pkgver.3::https://github.com/mesonbuild/meson/releases/download/$pkgver/meson-reference.3
  meson-reference-$pkgver.json::https://github.com/mesonbuild/meson/releases/download/$pkgver/reference_manual.json
  arch-meson
  cross-lib32
  native-clang
  0001-Skip-broken-tests.patch
)
b2sums=('eb17bc0bd1bf5ba48ab973c3d093184524f3d0afdb14ed403026dc9dfef956abf1b1a8a434071ad4db02b1bcc0f636ffb504aed68e61f95fad7e92496d337504'
        'SKIP'
        'a2615149175714f74d7ea1be426ce23475fa479d4f3ed4e9000b7bee46e15e7f1592d777334385d8858dedf3aa4256bcfaabf5f60150aa0854be6f67ad99ed59'
        '561c60eebdc1a6274c1d557addf927672097fdb3630a5148583077466cf19d2031348a4e0e22b095dc91b750b94b04d00e983f1518001bbc3c4b97747d7e1662'
        '70f042a7603d1139f6cef33aec028da087cacabe278fd47375e1b2315befbfde1c0501ad1ecc63d04d31b232a04f08c735d61ce59d7244521f3d270e417fb5af'
        '9b16477aa77a706492e26fb3ad42e90674b8f0dfe657dd3bd9ba044f921be12ceabeb0050a50a15caee4d999e1ec33ed857bd3bed9e4444d73bb4a4f06381081'
        '7d88929d5a3b49d91c5c9969f19d9b47f3151706526b889515acaeda0141257d5115875ac84832e9ea46f83a7700d673adcc5db84b331cd798c70ae6e90eac1e'
        '076f1650bf889b79a390da82e2e0a3d3b14e3383cdf4cd81ee06d7b776e070bb3d681295926828af123c16f950ab5cca127236f3030124fa9e8c7787a2c98297')
validpgpkeys=(
  19E2D6D9B46D8DAA6288F877C24E631BABB1FE70  # Jussi Pakkanen <jpakkane@gmail.com>
)

prepare() {
  cd $pkgname-$pkgver

  # Pass tests
  patch -Np1 -i ../0001-Skip-broken-tests.patch
}

build() {
  cd $pkgname-$pkgver
  python -m build --wheel --no-isolation
}

check() (
  cd $pkgname-$pkgver
  export LC_CTYPE=en_US.UTF-8 CPPFLAGS= CFLAGS= CXXFLAGS= LDFLAGS=
  ./run_tests.py --failfast
)

package() {
  cd $pkgname-$pkgver
  python -m installer --destdir="$pkgdir" dist/*.whl

  install -d "$pkgdir/usr/share/vim/vimfiles"
  cp -rt "$pkgdir/usr/share/vim/vimfiles" data/syntax-highlighting/vim/*/

  install -Dm644 data/shell-completions/bash/* -t "$pkgdir/usr/share/bash-completion/completions"
  install -Dm644 data/shell-completions/zsh/*  -t "$pkgdir/usr/share/zsh/site-functions"

  install -Dm644 ../meson-reference-$pkgver.3    "$pkgdir/usr/share/man/man3/meson-reference.3"
  install -Dm644 ../meson-reference-$pkgver.json "$pkgdir/usr/share/doc/$pkgname/reference_manual.json"

  install -D ../arch-meson -t "$pkgdir/usr/bin"

  install -Dm644 ../cross-lib32 "$pkgdir/usr/share/meson/cross/lib32"
  install -Dm644 ../native-clang "$pkgdir/usr/share/meson/native/clang"
}

# vim:set sw=2 sts=-1 et:
