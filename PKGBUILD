# Maintainer: Knut Ahlers
# Contributor: Riccardo Berto <riccardo at rcrdbrt dot-symbol com>
# Contributor:  Moses Narrow <moe-narrow@use.startmail.com>
# Contributor:  Dimitris Kiziridis <ragouel at outlook dot com>
# Contributor: Konrad Tegtmeier <konrad.tegtmeier+aur@gmail.com>
# Contributor: Marcel O'Neil <marcel@marceloneil.com>

pkgname=cockroachdb-bin
pkgver=24.1.3
pkgrel=1
pkgdesc='An open source, survivable, strongly consistent, scale-out SQL database'
arch=('x86_64')
url='https://www.cockroachlabs.com'
license=('Apache' 'custom:BSL' 'custom:CCL')
conflicts=('cockroachdb')
provides=('cockroachdb')
depends=('glibc')
source=("${pkgname}-${pkgver}.tgz::https://binaries.cockroachdb.com/cockroach-v${pkgver}.linux-amd64.tgz"
        "LICENSE::https://raw.githubusercontent.com/cockroachdb/cockroach/v${pkgver}/LICENSE"
        'cockroach.service'
        'cockroach.default'
        'cockroach.sysusers'
        'cockroach.tmpfiles')

sha256sums=('495d3fb2a417d180901e045a6faae324f5ea1bac27613e3f5ea5aa1def52f7f6'
            '0384efdf47789c3548e7eff949c5c2c9c4ee95741f805a39cdb6f02035919889'
            '6c336d30983d6295995823a134e3cc85a06ef9418339b53cf6f375df816bea51'
            '55f380f5cb201c6afeafbf1a6fb5a6400dbffa0edc134d30960d1d04e3d19ef2'
            '8be2f52529135d8d173bba130e000a187bbadc869ac2c603a4714af435840821'
            'c74cf876197312b91970bdd7832081750d2ab4d47e553bb46f38d57cba52641e')
package() {
  # generate shell completion
  "${srcdir}/cockroach-v${pkgver}.linux-amd64/cockroach" \
  gen autocomplete bash --out "${srcdir}/cockroach.bash"
  "${srcdir}/cockroach-v${pkgver}.linux-amd64/cockroach" \
  gen autocomplete zsh --out "${srcdir}/cockroach.zsh"

  # generate man pages
  "${srcdir}/cockroach-v${pkgver}.linux-amd64/cockroach" \
  gen man --path "${srcdir}/man"

  # binary
  install -Dm755 "${srcdir}/cockroach-v${pkgver}.linux-amd64/cockroach" "${pkgdir}/usr/bin/cockroach"

  # user/group & owned directories
  install -Dm644 "${srcdir}/cockroach.sysusers" "${pkgdir}/usr/lib/sysusers.d/cockroach.conf"
  install -Dm644 "${srcdir}/cockroach.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/cockroach.conf"

  # services & runtime
  install -Dm644 "${srcdir}/cockroach.service" "${pkgdir}/usr/lib/systemd/system/cockroach.service"
  install -Dm644 "${srcdir}/cockroach.default" "${pkgdir}/etc/default/cockroach"

  # man pages
  install -d "${pkgdir}/usr/share/man/man1/"
  install -m644 "${srcdir}"/man/*.1 "${pkgdir}/usr/share/man/man1/"

  # shell completion
  install -Dm644 cockroach.bash "${pkgdir}/usr/share/bash-completion/completions/cockroach"
  install -Dm644 cockroach.zsh  "${pkgdir}/usr/share/zsh/site-functions/_cockroach"

  # licenses
  install -Dm644 "${srcdir}/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

  # GEOS libraries (see: https://www.cockroachlabs.com/docs/stable/install-cockroachdb-linux.html)
  install -Dm644 "${srcdir}/cockroach-v${pkgver}.linux-amd64/lib/libgeos.so" "${pkgdir}/usr/local/lib/cockroach/libgeos.so"
  install -Dm644 "${srcdir}/cockroach-v${pkgver}.linux-amd64/lib/libgeos_c.so" "${pkgdir}/usr/local/lib/cockroach/libgeos_c.so"
}
