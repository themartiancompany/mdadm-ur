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
  (
    # _offline=true
    _systemd=false
 )
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
  "${pkgname}::git+${_url}#tag=${pkgname}-${pkgver}?signed"
  "${pkgname}.conf"
)
sha256sums=(
  'SKIP'
  '4ce1e90690282f98e4828e11576fbd61be65e97a2cdae6c7eac7035ea5ee53e5'
)

prepare() {
  cd \
    "${pkgname}"
  sed \
    "s/ -Wall //g" \
    -i \
    Makefile
  sed \
    "s/-Werror //g" \
    -i \
    Makefile
  # sed \
  #   "s/udev.o //g" \
  #   -i \
  #   Makefile
  # sed \
  #   "s/super-ddf.o //g" \
  #   -i \
  #   Makefile
  sed \
    "s/super-intel.o //g" \
    -i \
    Makefile
  sed \
    "s/platform-intel.o //g" \
    -i \
    Makefile
}

_get_usr() {
  local \
    _bin
  _bin="$( \
    dirname \
      "$( \
        command \
          -v \
	  "cc")")"
  echo \
    "$( \
      dirname \
       "${_bin}")"
}

build() {
  local \
    _cxflags=()
  _cxflags=(
    "${CFLAGS}"
    -DNO_LIBUDEV
    -I"$(_get_usr)/include"
  )
  cd \
    "${pkgname}"
  echo \
    "detected /usr: $(_get_usr)"
  export \
    RUN_DIR="$( \
      _get_usr)/run"
  make \
    CXFLAGS="${_cxflags[*]}" \
    BINDIR=/usr/bin \
    UDEVDIR=/usr/lib/udev \
    RUN_DIR="$( \
      _get_usr)/run"
}

package() {
  cd \
    "${pkgname}"
  make \
    INSTALL=/usr/bin/install \
    BINDIR=/usr/bin \
    DESTDIR="${pkgdir}" \
    UDEVDIR=/usr/lib/udev \
    install-bin
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
