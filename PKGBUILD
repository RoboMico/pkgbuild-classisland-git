# Maintainer: RoboMico <robomico at outlook dot com>

pkgname=classisland-git
_pkgname=classisland
pkgver=1.7.102.0.r4.gbecf2c2
pkgrel=1
pkgdesc="Class schedule displaying tool for interactive whiteboards in classrooms."
arch=('x86_64' 'aarch64')
url="https://github.com/ClassIsland/ClassIsland"
_branch="dev"
license=('GPL-3.0-only')
depends=('dotnet-runtime-8.0')
makedepends=(
    'dotnet-sdk-8.0'
    'git'
)
source=(
    "git+${url}.git#branch=${_branch}"
    "git+https://github.com/ClassIsland/EdgeTtsSharp.git#branch=classisland-v2"
    "${_pkgname}.sh"
    "${_pkgname}.desktop"
)
sha256sums=(
    'SKIP'
    'SKIP'
    '5342aed758213e2068c1a41c696b317b935fe491158fc750f454156686a35388'
    '9cc6b7f0d67d1573da4dd864b8affdfa8c5ae3f99051ef7349959d260f4f0822'
)
provides=("${_pkgname}=1.7.102.0")
conflicts=("${_pkgname}")

pkgver() {
    cd "${srcdir}/ClassIsland"
    git describe --long --tags --abbrev=7 | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}
prepare() {
    cd "${srcdir}/ClassIsland"
    cp -r "${srcdir}/EdgeTtsSharp" ./vendors
    git remote set-url origin https://github.com/ClassIsland/ClassIsland # resolve the SourceLink issue
}
build() {
    cd "${srcdir}/ClassIsland/ClassIsland.Desktop"
    mkdir -p ./_output/classisland
    dotnet build -c Release -o ./_output/classisland
}
package() {
    mkdir -p "${pkgdir}/opt" "${pkgdir}/usr/bin"
    cp -r "${srcdir}/ClassIsland/ClassIsland.Desktop/_output/classisland" "${pkgdir}/opt"
    printf "deb" > "${pkgdir}/opt/classisland/PackageType"
    install -Dm644 "${srcdir}/ClassIsland/ClassIsland/Assets/AppLogo.png" "${pkgdir}/usr/share/pixmaps/${_pkgname}.png"
    install -Dm644 "${srcdir}/${_pkgname}.desktop" "${pkgdir}/usr/share/applications/${_pkgname}.desktop"
    install -Dm755 "${srcdir}/${_pkgname}.sh" "${pkgdir}/usr/bin/${_pkgname}"
}
