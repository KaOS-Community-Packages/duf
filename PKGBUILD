pkgname=duf
pkgver=0.9.0
pkgrel=1
pkgdesc='Disk Usage/Free Utility'
arch=('x86_64')
url="https://github.com/muesli/${pkgname}"
license=('MIT')
makedepends=('go')
source=("${pkgname}-${pkgver}.tar.gz::${url}/archive/refs/tags/v${pkgver}.tar.gz")
sha256sums=('a81883475ab5591840882892a85185b7cda153e87f4d3978b45dec215761585c')

build() {
    local commit=$(zcat \${source[1]##*/} | git get-tar-commit-id)
    local extraflags="-X main.Version=${pkgver} -X main.CommitSHA=${commit}"
    export CGO_CPPFLAGS="${CPPFLAGS}"
    export CGO_CFLAGS="${CFLAGS}"
    export CGO_CXXFLAGS="${CXXFLAGS}"

    cd "${pkgname}-${pkgver}"
    go build \
        -trimpath \
        -buildmode=pie \
        -mod=readonly \
        -modcacherw \
        -ldflags "-linkmode external ${extraflags} -extldflags \"${LDFLAGS}\"" \
        -o "${pkgname}" .
}

package() {
    cd "${pkgname}-${pkgver}"
    install -Dm755 -t "${pkgdir}/usr/bin/" "${pkgname}"
    install -Dm644 -t "${pkgdir}/usr/share/man/man1/" "${pkgname}.1"
    install -Dm644 -t "${pkgdir}/usr/share/licenses/${pkgname}/" LICENSE
}
