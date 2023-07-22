_pkgbase="runelite"
pkgname="${_pkgbase}-dev"
pkgver=1.10.8.3
pkgrel=1
pkgdesc="Open source Old School RuneScape client. (Developer Version)"
license=("BSD")
arch=("any")
makedepends=("maven" 'java-environment>=11')
url="https://github.com/runelite/runelite"
depends=('java-runtime>=11' 'ttf-font')
source=("git+${url}.git")
sha512sums=('SKIP')

pkgver() {
    cd "${srcdir}/${_pkgbase}"
    git describe --tags $(git rev-list --tags --max-count=1) | cut -d '-' -f 3
}

build() {
    cd "${srcdir}/${_pkgbase}"
    git checkout $(git rev-list --tags --max-count=1)
    mvn clean package -DskipTests=true
}

check() {
    cd "${srcdir}/${_pkgbase}"
    mvn test verify
}

package() {
    client_jar=$(find ${srcdir}/${_pkgbase}/runelite-client/target -type f -name client-*-shaded.jar)
    
    install -D -m644 \
        "${client_jar}" \
        "${pkgdir}/usr/share/${pkgname}/RuneLite.jar"

    install -D -m644 \
        "${srcdir}/${_pkgbase}/LICENSE" \
        "${pkgdir}/usr/share/licenses/${pkgname}"

    install -D -m755 \
        "/dev/null" \
        "${pkgdir}/usr/bin/${pkgname}"

    echo '#!/bin/sh' > "${pkgdir}/usr/bin/${pkgname}"
    echo 'exec java -ea -Drunelite.pluginhub.version='"${pkgver}" '-Dhttps.protocols=TLSv1.3 -jar /usr/share/'"${pkgname}"'/RuneLite.jar "$@" --developer-mode' >> "${pkgdir}/usr/bin/${pkgname}"
}
