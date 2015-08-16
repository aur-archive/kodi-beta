# Maintainer: graysky <graysky AT archlinux DOT us>
# Contributor: BlackIkeEagle < ike DOT devolder AT gmail DOT com >
# Contributor: DonVla <donvla@users.sourceforge.net>
# Contributor: Ulf Winkelvos <ulf [at] winkelvos [dot] de>
# Contributor: Ralf Barth <archlinux dot org at haggy dot org>
# Contributor: B & monty - Thanks for your hints :)
# Contributor: marzoul
# Contributor: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Brad Fanella <bradfanella@archlinux.us>
# Contributor: [vEX] <niechift.dot.vex.at.gmail.dot.com>
# Contributor: Zeqadious <zeqadious.at.gmail.dot.com>
# Contributor: BartÅ‚omiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Maxime Gauduin <alucryd@gmail.com>
#
# Original credits go to Edgar Hucek <gimli at dark-green dot com>
# for his xbmc-vdpau-vdr PKGBUILD at https://archvdr.svn.sourceforge.net/svnroot/archvdr/trunk/archvdr/xbmc-vdpau-vdr/PKGBUILD

pkgname=kodi-beta
pkgver=14.0b2
_codename=Helix
pkgrel=1
pkgdesc='Kodi Entertainment Center beta version'
arch=('i686' 'x86_64')
url='http://xbmc.org'
license=('GPL2')
depends=('bluez-libs' 'curl' 'flac' 'glew' 'hicolor-icon-theme' 'lame' 'libaacs'
'libass' 'libbluray' 'libcdio' 'libmariadbclient' 'libmicrohttpd' 'libmodplug'
'libmpeg2' 'libpulse' 'libsamplerate' 'libssh' 'libva' 'libvdpau' 'libvorbis'
'libxrandr' 'libxslt' 'lzo' 'mesa' 'mesa-demos' 'python2-pillow'
'python2-simplejson' 'rtmpdump' 'sdl_image' 'smbclient' 'taglib' 'tinyxml'
'xorg-xdpyinfo' 'yajl')
makedepends=('afpfs-ng' 'boost' 'cmake' 'doxygen' 'gperf' 'jasper'
'java-runtime' 'libcec>=2.2.0' 'libnfs' 'libplist' 'nasm' 'shairplay'
'swig' 'unzip' 'upower' 'zip')
optdepends=('afpfs-ng: Apple shares support'
'bluez: Blutooth support'
'libnfs: NFS shares support'
'libplist: AirPlay support'
'libcec: Pulse-Eight USB-CEC adapter support'
'lirc: Remote controller support'
'pulseaudio: PulseAudio support'
'shairplay: AirPlay support'
'udisks: Automount external drives'
'unrar: Archives support'
'unzip: Archives support'
'upower: Display battery level'
'xorg-xinit: kodi standalone')
provides=('xbmc')
conflicts=('xbmc' 'kodi-devel')
install="${pkgname%-*}.install"
source=("kodi-$pkgver.tar.gz::https://github.com/xbmc/xbmc/archive/${pkgver}-${_codename}.tar.gz")
sha256sums=('9e517f0fd25e666b6a4de675f0614960e87c4232cab55cc4eb8e8c8d9063e7f8')

prepare() {
  cd xbmc-${pkgver}-Helix

  find -type f -name *.py -exec sed 's|^#!.*python$|#!/usr/bin/python2|' -i "{}" +
  sed 's|^#!.*python$|#!/usr/bin/python2|' -i tools/depends/native/rpl-native/rpl
  sed 's/python/python2/' -i tools/Linux/kodi.sh.in
}

build() {
  cd xbmc-${pkgver}-Helix

	# Bootstrapping
	./bootstrap

	# Configuring XBMC
	export PYTHON_VERSION=2  # external python v2
	./configure --prefix=/usr --exec-prefix=/usr \
		--disable-optimizations \
		--enable-gl \
		--enable-vaapi \
		--enable-vdpau \
		--enable-joystick \
		--enable-xrandr \
		--enable-rsxs \
		--enable-projectm \
		--enable-x11 \
		--enable-pulse \
		--enable-rtmp \
		--enable-samba \
		--enable-nfs \
		--enable-afpclient \
		--enable-airplay \
		--enable-airtunes \
		--enable-ffmpeg-libvorbis \
		--enable-dvdcss \
		--disable-hal \
		--enable-avahi \
		--enable-webserver \
		--enable-optical-drive \
		--enable-libbluray \
		--enable-texturepacker \
		--enable-udev \
		--enable-libusb \
		--enable-libcec \
		--enable-external-libraries \
		--with-lirc-device=/run/lirc/lircd

	# Now (finally) build
	make
}

package() {
  cd xbmc-${pkgver}-Helix

  make DESTDIR="${pkgdir}" install

  # Tools
  install -Dm755 $srcdir/xbmc-${pkgver}-Helix/tools/TexturePacker/TexturePacker \
    ${pkgdir}/usr/lib/kodi/

  # Licenses
  install -dm755 ${pkgdir}/usr/share/licenses/${pkgname}
  for licensef in LICENSE.GPL copying.txt; do
    mv ${pkgdir}/usr/share/doc/kodi/${licensef} \
      ${pkgdir}/usr/share/licenses/${pkgname}
  done
}

# vim: ts=2 sw=2 et:
