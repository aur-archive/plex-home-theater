# Maintainer: Maxime Gauduin <alucryd@gmail.com>

pkgname=plex-home-theater
pkgver=1.0.12
pkgrel=1
pkgdesc='Plex Home Theater'
arch=('i686' 'x86_64')
url='http://www.plexapp.com/'
license=('GPL2')
depends=('boost-libs' 'curl' 'fontconfig' 'glew' 'java-runtime' 'lame' 'libass' 'libcdio' 'libjpeg-turbo' 'libmad' 'libmicrohttpd' 'libmodplug' 'libmpeg2' 'libpulse' 'libsamplerate' 'libssh' 'libusb-compat' 'libva' 'libxrandr' 'lzo2' 'mesa' 'rtmpdump' 'sdl_image' 'sdl_mixer' 'smbclient' 'taglib' 'tinyxml' 'yajl')
makedepends=('boost' 'cmake' 'doxygen' 'ftgl' 'java-environment' 'libcec' 'libplist' 'libshairport' 'nasm' 'swig' 'unzip' 'zip')
optdepends=('libplist: AirPlay support'
            'libshairport: AirPlay support'
            'libcec: Pulse-Eight USB-CEC adapter support'
            'pulseaudio: PulseAudio support')
source=("https://github.com/plexinc/plex-home-theater-public/archive/pht-v${pkgver}.tar.gz"
        'plexhometheater.sh'
        'plex-fribidi.patch')
sha256sums=('408edd875fd8b4d2e5fc9c97719e8c2399c6a84e4c1c10222d32f46f46ef82bd'
            '15993c2dad0f874253d93f2198320d97e35801962363ad92edbfe936567fcaa3'
            'de777a4582932344408cbffd1f512ca75b6a7e8a2dc716ffc01cd512307c3433')

prepare() {
  cd plex-home-theater-public-pht-v${pkgver}

  patch -Np1 -i ../plex-fribidi.patch
}

build() {
  cd plex-home-theater-public-pht-v${pkgver}

  if [[ -d build ]]; then
    rm -rf build
  fi
  mkdir build && cd build

  cmake .. -DCMAKE_BUILD_TYPE='Release' -DCMAKE_INSTALL_PREFIX='/usr' -DCMAKE_C_FLAGS="$CMAKE_C_FLAGS -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include" -DCMAKE_CXX_FLAGS="$CMAKE_CXX_FLAGS -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include" -DENABLE_{AUTOUPDATE='OFF',DUMP_SYMBOLS='OFF',PYTHON='OFF'} -DUSE_INTERNAL_FFMPEG='ON' -DCREATE_BUNDLE='OFF'
  make
}

package() {
  cd plex-home-theater-public-pht-v${pkgver}/build

  make DESTDIR="${pkgdir}" install

# Install additional files
  install -dm 755 "${pkgdir}"/usr/{lib/plexhometheater,share/{applications,pixmaps}}
  install -m 755 "${srcdir}"/plexhometheater.sh "${pkgdir}"/usr/bin/
  install -m 644 ../plex/Resources/plexhometheater.desktop "${pkgdir}"/usr/share/applications/
  install -m 644 ../plex/Resources/plex-icon-256.png "${pkgdir}"/usr/share/pixmaps/plexhometheater.png

# Move things where they should be
  mv "${pkgdir}"/usr/{bin/system,lib/plexhometheater/}
  mv "${pkgdir}"/usr/share/{XBMC,plexhometheater}

# Very ugly workaround
  ln -s ../lib/plexhometheater/system "${pkgdir}"/usr/bin/
}

# vim: ts=2 sw=2 et:
