# Maintainer: Mohammad Kamrul Hasan
# Shamelessly stolen from the aur package of luciddream
# https://aur.archlinux.org/packages/lemonade-server-git

pkgname=lemonade-server-git
_pkgname=lemonade
pkgdesc="Lemonade: Local LLM Serving with GPU and NPU acceleration (Server)"
epoch=1
pkgver=10.9.0 # Match this to your pinned tag version
pkgrel=1
arch=('x86_64')
url='https://github.com/lemonade-sdk/lemonade/'
license=('Apache-2.0')

# Included cpp-httplib here since the build now relies on the system version
makedepends=('cmake' 'ninja' 'git' 'openssl' 'cpp-httplib')
depends=('zstd' 'curl' 'libwebsockets' 'libdrm' 'libcap' 'systemd-libs' 'libayatana-appindicator' 'gtk3' 'libnotify')

provides=('lemonade-server')
conflicts=('lemonade-server')
backup=('etc/lemonade/lemonade.conf' 'etc/lemonade/secrets.conf')

_tag=v10.9.0

# Cleaned up: Removed the httplib tarball download completely
source=(
  "${_pkgname}::git+https://github.com/lemonade-sdk/lemonade.git#tag=${_tag}"
  sysusers.conf
  tmpfiles.conf
  zz-secrets.conf
)

# Only local configuration files need checksum verification now
sha256sums=(
  'SKIP'
  '069d5612d570e83128d7eed7ffe4525943d75d22b9c84537d861833157e74b26'
  'f7353d20f265fbdda9121e8587443cef95ba5fb89e1704a87920876ce966804b'
  '9b57a15cd9596881d8141e8bff2504802c2dac415228b9d28a89cce25c482989'
)

build() {
  local _cores=$(nproc)
  if (( _cores > 8 )); then
    _cores=8
  fi

  echo "Building with ${_cores} cores"

  local cmake_options=(
    -B build
    -G Ninja
    -S "${_pkgname}"
    -W no-dev
    -D REQUIRE_LINUX_TRAY=ON
    -D CMAKE_BUILD_TYPE=Release
    -D CMAKE_INSTALL_PREFIX=/usr
  )
  cmake "${cmake_options[@]}"
  cmake --build build --parallel "${_cores}"
}

package() {
  DESTDIR="$pkgdir" cmake --install build

  install -dm0755 "${pkgdir}"/var/lib/lemonade
  install -dm0755 "${pkgdir}"/etc/lemonade
  install -dm0755 "${pkgdir}"/etc/lemonade/conf.d

  install -vDm644 "${srcdir}/sysusers.conf" "${pkgdir}/usr/lib/sysusers.d/lemonade-server.conf"
  install -vDm644 "${srcdir}/tmpfiles.conf" "${pkgdir}/usr/lib/tmpfiles.d/lemonade-server.conf"
  install -vDm644 "${srcdir}/zz-secrets.conf" "${pkgdir}/etc/lemonade/conf.d/zz-secrets.conf"
}
