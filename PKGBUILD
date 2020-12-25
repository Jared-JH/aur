# Maintainer: BrLi <brli at chakralinux dot org>

pkgname=zettlr
pkgver=1.8.4
pkgrel=3
pkgdesc="A markdown editor for writing academic texts and taking notes"
arch=('x86_64')
url='https://www.zettlr.com'
license=('GPL' 'custom') # Noted that the icon and name are copyrighted
depends=(electron)
makedepends=(yarn git)
optdepends=('pandoc: For exporting to various format'
            'texlive-bin: For Latex support'
            'ttf-lato: Display output in a more comfortable way')
_csl_locale_commit=ecb8e70233e9a68e8b1dda4586061be8f8611a38 # Dec 11, 2020
options=(!strip)
install=install
source=("${pkgname}-${pkgver}.tar.gz"::"https://github.com/Zettlr/Zettlr/archive/v${pkgver}.tar.gz"
        pandoc-fix.patch
        # citation style
        "https://github.com/citation-style-language/locales/archive/${_csl_locale_commit}.zip"
        https://github.com/citation-style-language/styles/raw/master/chicago-author-date.csl
        # Chinese(Taiwan) translation
        https://github.com/Brli/zetter-zh-TW/raw/master/zh-TW.json)
sha256sums=('da3ba36a98587e258bdb09700343b7a067ee4659aaef6b5021b16dd5b9d00400'
            '488f3c308e0cbdcc626d95798e07fa3489ea28a631e9fbdacb50e6fcf35c5d4c'
            '24503a6cd5b3651a7003353811ae82d3ed707ec8ff932d341668c2ad377434b6'
            '2b7cd6c1c9be4add8c660fb9c6ca54f1b6c3c4f49d6ed9fa39c9f9b10fcca6f4'
            '9aef5eec4876aa180fa55cf3bc213a5cf68ab96567bc4021ba58a32fa0fa9f94')

prepare() {
    cd "${srcdir}/Zettlr-${pkgver}"

    # Manually add community translation
    cp "${srcdir}/zh-TW.json" source/common/lang/

    # csl:refresh from package.json
    cp $(find "${srcdir}/locales-${_csl_locale_commit}/" -name "*.xml") source/app/service-providers/assets/csl-locales/
    cp "${srcdir}/locales-${_csl_locale_commit}/locales.json" source/app/service-providers/assets/csl-locales/
    cp "${srcdir}/chicago-author-date.csl" source/app/service-providers/assets/csl-styles/

    # fake Pandoc
    patch -Np1 -i "${srcdir}/pandoc-fix.patch"
    ln -sf /dev/null resources/pandoc
}

build() {
    cd "${srcdir}/Zettlr-${pkgver}"
    local NODE_ENV=''
    yarn install --cache-folder "${srcdir}/cache" \
                 --link-folder "${srcdir}/link" \
                 --ignore-scripts
    yarn reveal:build

    rm -rf node_modules/electron
    yarn add -D electron@11.1.0 --cache-folder "${srcdir}/cache" --link-folder "${srcdir}/link"

    node node_modules/.bin/electron-forge package

    # Remove fonts
    cd "${srcdir}/Zettlr-${pkgver}/.webpack"
    find . -type d -name "fonts" -exec rm -rf {} +
}

# check() {
#     cd "${srcdir}/Zettlr-${pkgver}"
#     # Require electron module to test
#     yarn add --cache-folder "${srcdir}/cache" --link-folder "${srcdir}/link" electron
#     # The "test" function in package.json
#     node node_modules/mocha/bin/mocha
#     # The "test-gui" function in package.json, not useful in our case
#     node scripts/test-gui.js
#     # Clean up
#     yarn remove electron
#     rm yarn.lock
#     rm node_modules/.bin -rf
# }

package() {
    local _destdir=usr/lib/"${pkgname}"
    install -dm755 "${pkgdir}/${_destdir}"

    cd "${srcdir}/Zettlr-${pkgver}"

    # Copy the generated electron project
    cp -r --no-preserve=ownership --preserve=mode ./.webpack "${pkgdir}/${_destdir}/"
    cp -r --no-preserve=ownership --preserve=mode ./package.json "${pkgdir}/${_destdir}/"

    # Install start script to /usr/bin
   install -Dm755 /dev/stdin "${pkgdir}/usr/bin/${pkgname}" <<END
#!/bin/sh
exec electron /${_destdir} "\$@"
END

    # install icons of various sizes to hi-color theme
    for px in 16 24 32 48 64 96 128 256 512; do
        install -Dm644 "${srcdir}/Zettlr-${pkgver}/resources/icons/png/${px}x${px}.png" \
            "${pkgdir}/usr/share/icons/hicolor/${px}x${px}/apps/${pkgname}.png"
    done

    # generate freedesktop entry files, aligned with description in package.json and forge.config.js
    install -Dm644 /dev/stdin "${pkgdir}/usr/share/applications/${pkgname}.desktop" <<END
[Desktop Entry]
Name=Zettlr
Comment=A powerful Markdown Editor with integrated tree view
Exec=${pkgname} %U
Terminal=false
Type=Application
Icon=${pkgname}
StartupWMClass=Zettlr
MimeType=text/markdown;
Categories=Office;
END

    # license
    install -Dm644 "${srcdir}/Zettlr-${pkgver}/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
