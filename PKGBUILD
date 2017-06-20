# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Contributor: Anatol Pomozov <anatol dot pomozov at gmail>

pkgname=meson
pkgver=0.41.1
pkgrel=1
pkgdesc='High productivity build system'
url='http://mesonbuild.com/'
arch=('any')
license=('Apache')
depends=('python' 'ninja')
makedepends=('python-setuptools')
checkdepends=('gcc-objc' 'vala' 'rust' 'gcc-fortran' 'mono' 'boost' 'qt4' 'qt5-base' 'git' 'gnustep-base'
              'cython' 'gtkmm3' 'gtest' 'gmock' 'protobuf' 'wxgtk' 'python-gobject' 'gobject-introspection'
              'itstool' 'gtk3' 'valgrind' 'ldc' 'java-environment>=8' 'gtk-doc' 'llvm' 'clang' 'sdl2'
              'doxygen')
source=(https://github.com/mesonbuild/meson/releases/download/${pkgver}/meson-${pkgver}.tar.gz{,.asc})
sha512sums=('fd0226e8cceed99a48f82bf96c71742df92091cc80001fc36d216ec043ec6e67829244838a350acd7620682e0ae65abd6ddf46561cde72ba7c03b4f49f86ecf8'
            'SKIP')
validpgpkeys=('95181F4EED14FDF4E41B518D3BF4693BFEEB9428') # Jussi Pakkanen <jpakkane@gmail.com>

build() {
  cd ${pkgname}-${pkgver}
  python setup.py build
}

check() {
  cd ${pkgname}-${pkgver}
  unset CLASSPATH  # GNUstep breaks java tests
  # Installing graphviz breaks doxygen tests
  LC_CTYPE=en_US.UTF-8 DC=ldc ./run_tests.py
}

package() {
  cd ${pkgname}-${pkgver}
  python setup.py install --root="${pkgdir}" --optimize=1 --skip-build
  install -Dm 644 syntax-highlighting/vim/ftdetect/meson.vim -t "${pkgdir}/usr/share/vim/vimfiles/ftdetect"
  install -Dm 644 syntax-highlighting/vim/indent/meson.vim -t "${pkgdir}/usr/share/vim/vimfiles/indent"
  install -Dm 644 syntax-highlighting/vim/syntax/meson.vim -t "${pkgdir}/usr/share/vim/vimfiles/syntax"
}

# vim: ts=2 sw=2 et:
