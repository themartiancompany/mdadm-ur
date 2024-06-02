# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Tobias Powalowski <tpowa@archlinux.org>
# Contributor: Judd Vinet <jvinet@zeroflux.org>

_os="$( \
  uname \
    -o)"
_systemd=true
_offline=false
[[ "${_os}" == "Android" ]] && \
  _offline=true \
  _systemd=false
pkgname=mdadm
pkgver=4.3
pkgrel=2
_pkgdesc=(
  'A tool for managing/monitoring'
  'Linux md device arrays,'
  'also known as Software RAID'
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'x86_64'
  'arm'
  'aarch64'
  'i686'
)
license=(
  'GPL-2.0-or-later'
)
_http="https://git.kernel.org/pub/scm"
_ns="utils"
url="${_http}/${_ns}/${pkgname}"
makedepends=(
  'git'
)
depends=(
  'glibc'
)
[[ "${_systemd}" == "true" ]] && \
  depends+=(
    'systemd'
  )
conflicts=(
  'mkinitcpio<38'
)
optdepends=(
  'bash: mdcheck'
)
replaces=(
  'raidtools'
)
backup=(
  "etc/${pkgname}.conf"
)
validpgpkeys=(
  # Jes Sorensen
  '6A86B80E1D22F21D0B26BA75397D82E0531A9C91'
  # Mariusz Tkaczyk
  'EED84966493AEEAF4B466F696F9E3E9D4EDEBB11'
)
_url="${url}/${pkgname}.git"
[[ "${_offline}" == true ]] && \
  _url="file://${HOME}/${pkgname}"
source=(
  "${pkgname}::git+${url}#tag=${pkgname}-${pkgver}?signed"
  "${pkgname}.conf"
)
sha256sums=(
  'SKIP'
  '4ce1e90690282f98e4828e11576fbd61be65e97a2cdae6c7eac7035ea5ee53e5'
)

build() {
  cd \
    "${pkgname}"
  make \
    CXFLAGS="${CFLAGS}" \
    BINDIR=/usr/bin \
    UDEVDIR=/usr/lib/udev 
}

package() {
  cd \
    "${pkgname}"
  make \
    INSTALL=/usr/bin/install \
    BINDIR=/usr/bin \
    DESTDIR="${pkgdir}" \
    UDEVDIR=/usr/lib/udev \
    install
  make \
    SYSTEMD_DIR="$pkgdir"/usr/lib/systemd/system \
    install-systemd
  install \
    -Dm644 \
    "../${pkgname}.conf" \
    "${pkgdir}/etc/${pkgname}.conf"
  install \
    -Dm755 \
    misc/mdcheck \
    "${pkgdir}/usr/share/${pkgname}/mdcheck"
}
