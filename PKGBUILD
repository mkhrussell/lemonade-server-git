# Maintainer: Mohammad Kamrul Hasan
pkgname=lemonade-server-git
pkgbase=lemonade
pkgdesc="Lemonade: Local LLM Serving with GPU and NPU acceleration (Server)"
pkgver=10.10.0
pkgrel=1
arch=('x86_64')
url='https://github.com/lemonade-sdk/lemonade/'
license=('Apache-2.0')
provides=('lemonade-server')
conflicts=('lemonade-server')
backup=('etc/lemonade/lemonade.conf' 'etc/lemonade/secrets.conf')

makedepends=('cmake' 'ninja' 'git' 'openssl' 'cpp-httplib')
depends=('zstd' 'curl' 'libwebsockets' 'libdrm' 'libcap' 'systemd-libs')

source=(
  "${pkgbase}::git+https://github.com/lemonade-sdk/lemonade.git"
  'sysusers.conf'
  'tmpfiles.conf'
  'zz-secrets.conf'
)

sha256sums=(
  'SKIP'
  '069d5612d570e83128d7eed7ffe4525943d75d22b9c84537d861833157e74b26'
  'f7353d20f265fbdda9121e8587443cef95ba5fb89e1704a87920876ce966804b'
  '9b57a15cd9596881d8141e8bff2504802c2dac415228b9d28a89cce25c482989'
)

# 3. Fixed: Robust pkgver function
pkgver() {
  cd "${pkgbase}"

  # This regex:
  # - Removes a leading 'v' (if present), everything from the first dash '-' onwards
  # Example: 'v10.10.0-0-g123456' becomes '10.10.0'
  git describe --long --tags | sed 's/^v//; s/-.*//'
}

build() {
  cmake -B build -G Ninja -S "${pkgbase}" \
    -W no-dev \
    -D REQUIRE_LINUX_TRAY=OFF \
    -D CMAKE_BUILD_TYPE=Release \
    -D CMAKE_INSTALL_PREFIX=/usr

  cmake --build build
}

package() {
  DESTDIR="$pkgdir" cmake --install build

  install -dm0755 "${pkgdir}/var/lib/lemonade"
  install -dm0755 "${pkgdir}/etc/lemonade"
  install -dm0755 "${pkgdir}/etc/lemonade/conf.d"

  install -vDm644 "${srcdir}/sysusers.conf" "${pkgdir}/usr/lib/sysusers.d/lemonade-server.conf"
  install -vDm644 "${srcdir}/tmpfiles.conf" "${pkgdir}/usr/lib/tmpfiles.d/lemonade-server.conf"
  install -vDm644 "${srcdir}/zz-secrets.conf" "${pkgdir}/etc/lemonade/conf.d/zz-secrets.conf"
}
