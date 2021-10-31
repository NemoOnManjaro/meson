# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Contributor: Anatol Pomozov <anatol dot pomozov at gmail>

pkgname=meson
pkgver=0.60.0
pkgrel=2
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
source=(https://github.com/mesonbuild/meson/releases/download/${pkgver/rc/.rc}/meson-${pkgver}.tar.gz{,.asc}
        0001-i18n-merge_file-deprecate-positional-arguments.patch
        0002-test_clang_format-Do-not-assume-meson-source-is-in-g.patch
        0003-modules-gnome-ensure-that-install_dir-is-set.patch
        0004-modules-gnome-fix-missing-install_dir-again-harder.patch
        0005-modules-gnome-use-install_dir-instead-of-false.patch
        0006-mtest-accept-very-long-lines.patch
        skip-test.diff
        arch-meson)
sha512sums=('05275f003445672c1613db6c4c62ab39bfedc64cf2261f91d361a2aa24116a08185874b2e5dfa7daeebd584313bd91f4e0856fe68b36d8709065f80529902dda'
            'SKIP'
            '08b603acb19cf85f824e2e05ee7f2cd720ee46e488042498e4ef671e2a341c8f6ca22ef4f76fab2c48ab4d99949990753b3489ddaf20f08d3a9145899140d3aa'
            'e5b98b635f0bbc36aafd180a8fc683a5aac5f6f06b245b71e9ab536f180538330c279bdc0c33a898d5adb3bb1d1feb0746f577eba2ce6ce1a915a3fba3c24568'
            '1a71752d98adf3759077a0f93483a4051af9365dbd40626fb96897eb65a0ee29e3b9fc77fdd9bdc68fd257247e7e4c97c3052efbbeacbde5329ce0790373c8c2'
            '56453fec090b7f64ae55d7938fdeda50f5746354a5f179992e7edcde150a610039a35ec4d5f0bb88436b5db5fb5c7dd072731166abaab7dcffc1a14c258d81fb'
            'dd5d6e640ba71ea84617b856ee0dc9a222fbcbdf506e75ce4777b7f2280af4c8f27a5f03c1fc0c173f455d1b85d18d0a779ff95a1df1d06ae5db1848c43a4e54'
            'ff39077b1c6e8f1b348e2bf3f6d1801fb69bfb37f25f2a8d935b5456bc38da7528309cd7d5f279205d77759b80f4746be1ab5c25e3c7ee9536aa5a72c096fc8d'
            '486e60b1d470c64d07d4686bd0b4374924967b9e024ff6d9fe248f512995ece19b8b09dd69c91f26dec4bb92c61fc3d275a2b7e481c2acd10ebb90d7e3cb7e20'
            'f451f8a7ef9cf1dd724c2ce20bb85a3f1611b87b2e7a17ef0fdbe8ab82a67389f818ea30a5adfe8413143e4eac77ea2e0b8234b5b2466b41a892e2bd0435376c')
validpgpkeys=('19E2D6D9B46D8DAA6288F877C24E631BABB1FE70') # Jussi Pakkanen <jpakkane@gmail.com>

prepare() {
  cd ${pkgname}-${pkgver}

  # Backports from 0.60 branch
  patch -Np1 -i ../0001-i18n-merge_file-deprecate-positional-arguments.patch
  patch -Np1 -i ../0002-test_clang_format-Do-not-assume-meson-source-is-in-g.patch
  patch -Np1 -i ../0003-modules-gnome-ensure-that-install_dir-is-set.patch
  patch -Np1 -i ../0004-modules-gnome-fix-missing-install_dir-again-harder.patch
  patch -Np1 -i ../0005-modules-gnome-use-install_dir-instead-of-false.patch
  patch -Np1 -i ../0006-mtest-accept-very-long-lines.patch

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
