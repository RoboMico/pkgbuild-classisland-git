# Maintainer: RoboMico <robomico at outlook dot com>

pkgname=classisland-git
_pkgname=classisland
_appname=cn.classisland.app
pkgver=1.7.103.0.r0.g54688be
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
)
sha256sums=(
    'SKIP'
    'SKIP'
    '5342aed758213e2068c1a41c696b317b935fe491158fc750f454156686a35388'
)
provides=("${_pkgname}=1.7.103.0")
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
    mkdir -p ./_output/${_pkgname}
    dotnet build -c Release -o ./_output/${_pkgname}
}
package() {
    mkdir -p "${pkgdir}/opt" "${pkgdir}/usr/bin"
    cp -r "${srcdir}/ClassIsland/ClassIsland.Desktop/_output/${_pkgname}" "${pkgdir}/opt"
    printf "deb" > "${pkgdir}/opt/${_pkgname}/PackageType"
    install -Dm644 "${srcdir}/ClassIsland/ClassIsland/Assets/AppLogo_AppLogo.svg" "${pkgdir}/usr/share/icons/hicolor/scalable/apps/${_pkgname}.svg"
    install -Dm644 "${srcdir}/ClassIsland/ClassIsland/Assets/ShortcutTemplates/${_appname}.desktop" "${pkgdir}/usr/share/applications/${_appname}.desktop"
    sed -i "s/{0}/${pkgver}/" "${pkgdir}/usr/share/applications/${_appname}.desktop"
    sed -i "s/{1}/\/usr\/bin\/${_pkgname}/" "${pkgdir}/usr/share/applications/${_appname}.desktop"
    install -Dm755 "${srcdir}/${_pkgname}.sh" "${pkgdir}/usr/bin/${_pkgname}"
}
