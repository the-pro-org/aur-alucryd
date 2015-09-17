# Maintainer: Maxime Gauduin <alucryd@archlinux.org>
# Contributor: Daniel Seymour <dannyseeless@gmail.com>

pkgname=emby-server-devel
pkgver=3.0.5724.4
pkgrel=1
pkgdesc='Bring together your videos, music, photos, and live television'
arch=('i686' 'x86_64' 'armv6h')
url='http://emby.media'
license=('GPL2')
depends=('ffmpeg' 'imagemagick' 'libmediainfo' 'mono' 'sqlite')
makedepends=('git')
provides=('emby-server')
conflicts=('emby-server')
install='emby-server.install'
source=("git+https://github.com/MediaBrowser/Emby.git#tag=${pkgver}"
        'emby-server'
        'emby-migrate-database'
        'emby-server.conf'
        'emby-server.service')
backup=('etc/conf.d/emby-server')
sha256sums=('SKIP'
            '7b1974f7bba8ac4b76e51ef7fe1257d165c7c4abbd0915e192391336048a3d74'
            '0e3f6b7fe700a3bbdf97bdae8655453b34b1bd08fa8ae339e0fd130fe8670b0b'
            'c9ad78f3e2f0ffcb4ee66bb3e99249fcd283dc9fee17895b9265dc733288b953'
            '8a91ea49a1699c820c4a180710072cba1d6d5c10e45df97477ff6a898f4e1d70')

pkgver() {
  cd Emby

  git describe --tags | sed 's/-/.r/; s/-g/./'
}

prepare() {
  cd Emby

  sed 's/libMagickWand-6.Q8.so/libMagickWand-6.Q16HDRI.so/' -i MediaBrowser.Server.Mono/ImageMagickSharp.dll.config
}

build(){
  cd Emby

  xbuild \
    /p:Configuration='Release Mono' \
    /p:Platform='Any CPU' \
    /p:OutputPath="${srcdir}/build" \
    /t:build MediaBrowser.Mono.sln
}

package() {
  install -dm 755 "${pkgdir}"/{etc/conf.d,usr/{bin,lib/systemd/system}}
  cp -dr --no-preserve='ownership' build "${pkgdir}"/usr/lib/emby-server
  install -m 755 emby-server "${pkgdir}"/usr/bin/
  install -m 755 emby-migrate-database "${pkgdir}"/usr/bin/
  install -m 644 emby-server.service "${pkgdir}"/usr/lib/systemd/system/
  install -m 644 emby-server.conf "${pkgdir}"/etc/conf.d/emby-server

  install -dm 755 "${pkgdir}"/var/lib/emby
  chown 422:422 -R "${pkgdir}"/var/lib/emby
}

# vim: ts=2 sw=2 et:
