# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Contributor: Anatol Pomozov <anatol dot pomozov at gmail>

pkgname=meson
pkgver=0.61.0
pkgrel=3
pkgdesc='High productivity build system'
url='https://mesonbuild.com/'
arch=('any')
license=('Apache')
depends=('python-setuptools' 'ninja')
checkdepends=('gcc-objc' 'vala' 'rust' 'gcc-fortran' 'mono' 'boost' 'qt5-base' 'git' 'cython'
              'gtkmm3' 'gtest' 'gmock' 'protobuf' 'wxgtk3' 'python-gobject' 'gobject-introspection'
              'itstool' 'gtk3' 'java-environment=8' 'gtk-doc' 'llvm' 'clang' 'sdl2' 'graphviz'
              'doxygen' 'vulkan-validation-layers' 'openssh' 'mercurial' 'gtk-sharp-2' 'qt5-tools'
              'libwmf' 'valgrind' 'cmake' 'netcdf-fortran' 'openmpi' 'nasm' 'gnustep-base' 'libelf'
              'python-pytest-xdist' 'python2-setuptools' 'ldc' 'rust-bindgen' 'cuda' 'hotdoc')
source=(https://github.com/mesonbuild/meson/releases/download/${pkgver}/meson-${pkgver}.tar.gz{,.asc}
        https://github.com/mesonbuild/meson/commit/81fbcd1df48f669b6b23b9dd0752d6d366cc9bf2.patch
        https://github.com/mesonbuild/meson/commit/704e9802c9f424a57c32d9e63ececfece807529a.patch
        skip-test.diff
        arch-meson)
sha512sums=('ff739f767710c09a1b238f135c81bdb79675d06cec1b091503809cdbd71f0f92fd76bf068650bbec60688b79fbda94e56cb3203c948aa79f16a88f6d9db219d1'
            'SKIP'
            '7e57c59394b73819e3af9722dc26e68edebec35187ceb39116f15a890fb0277cfe198b0ba84218a7f3333cffb16e217717b94d325224167175c0c1479e0f94a5'
            'f7b0d8e77af19cd666c1efcbed8767483efa4d2b58a08d9a01151f77dbcbd8047a6dfd95fa57146d565c0b7f404368efef6fdc096e127c640c66747641ecce3c'
            '8c4e9053980bb9cec7d8756a844e13adb44f29615aa279d57f35733925ff3ba7a8451f796e335d63f1fa180a72d42ecbd4ab3782a25497e5ceff551304ae255f'
            'f451f8a7ef9cf1dd724c2ce20bb85a3f1611b87b2e7a17ef0fdbe8ab82a67389f818ea30a5adfe8413143e4eac77ea2e0b8234b5b2466b41a892e2bd0435376c')
validpgpkeys=('19E2D6D9B46D8DAA6288F877C24E631BABB1FE70') # Jussi Pakkanen <jpakkane@gmail.com>

prepare() {
  cd ${pkgname}-${pkgver}

  # https://bugs.archlinux.org/task/73329
  patch -Np1 -i ../81fbcd1df48f669b6b23b9dd0752d6d366cc9bf2.patch

  # Fix building gdm
  patch -Np1 -i ../704e9802c9f424a57c32d9e63ececfece807529a.patch

  # Our containers do not allow sanitizers to run
  patch -Np1 -i ../skip-test.diff
}

build() {
  cd ${pkgname}-${pkgver}
  python setup.py build
}

check() (
  cd ${pkgname}-${pkgver}
  export LC_CTYPE=en_US.UTF-8 CPPFLAGS= CFLAGS= CXXFLAGS= LDFLAGS=
  ./run_tests.py
)

package() {
  cd ${pkgname}-${pkgver}
  python setup.py install --root="${pkgdir}" --optimize=1 --skip-build

  install -d "${pkgdir}/usr/share/vim/vimfiles"
  cp -rt "${pkgdir}/usr/share/vim/vimfiles" data/syntax-highlighting/vim/*/

  install -Dt "${pkgdir}/usr/share/bash-completion/completions" -m644 data/shell-completions/bash/*
  install -Dt "${pkgdir}/usr/share/emacs/site-lisp" -m644 data/syntax-highlighting/emacs/*
  install -Dt "${pkgdir}/usr/share/zsh/site-functions" -m644 data/shell-completions/zsh/*

  # Arch packaging helper
  install -D ../arch-meson -t "${pkgdir}/usr/bin"
}

# vim:set sw=2 et:
