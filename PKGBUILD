# Maintainer: Your Name <youremail@domain.com>
pkgname=onlyoffice-desktopeditors-bin
pkgver=9.4.0
pkgrel=1
pkgdesc='An office suite that combines text, spreadsheet and presentation editors'
arch=(x86_64 aarch64)
url='https://www.onlyoffice.com/'
license=('AGPL-3.0-only')
depends=(
	alsa-lib
	at-spi2-core
	cairo
	dbus
	expat
	fontconfig
	freetype2
	glib2
	glibc
	gst-plugins-base-libs
	gstreamer
	gtk3
	hicolor-icon-theme
	libcups
	libdrm
	libgcc
	libglvnd
	libice
	libnotify
	libpulse
	libsm
	libstdc++
	libx11
	libxcb
	libxcomposite
	libxdamage
	libxext
	libxfixes
	libxi
	libxkbcommon
	libxkbcommon-x11
	libxrandr
	mesa
	nspr
	nss
	pango
	qt5-base
	qt5-svg
	qt5-declarative
	qt5-multimedia
	qt5-x11extras
	sh
	ttf-font
)
optdepends=('gst-plugins-ugly: support additional multimedia codecs')
conflicts=(onlyoffice-desktopeditors onlyoffice-desktopeditors-git onlyoffice onlyoffice-git onlyoffice-bin)
options=(!debug !strip)
source_aarch64=("https://github.com/ONLYOFFICE/DesktopEditors/releases/download/v${pkgver}/onlyoffice-desktopeditors-aarch64.tar.xz")
source_x86_64=("https://github.com/ONLYOFFICE/DesktopEditors/releases/download/v${pkgver}/onlyoffice-desktopeditors-x64.tar.xz")
sha256sums_x86_64=('d054d35f6c11274755cdad32683e8238f886420756c6cdcbf68c3b89ea66675c')
sha256sums_aarch64=('3cc3676643bc79717fc3b5de3a1a7ebf27c085573b030295a7af46a39a6f676f')

prepare() {
	sed -i 's|/opt/onlyoffice/desktopeditors|/usr/lib/onlyoffice/desktopeditors|g' "${srcdir}/usr/bin/onlyoffice-desktopeditors"
}

package() {
	local _pkgname='onlyoffice-desktopeditors'
	local _pkgpath='onlyoffice/desktopeditors'
	local appdir="${pkgdir}/usr/lib/${_pkgpath}"

	# Install onlyoffice-desktop-editors while respecting file modes
	(
		cd "${srcdir}/opt/onlyoffice/desktopeditors" || exit
		while IFS= read -r -d '' file; do
			if [[ -x "${file}" ]]; then
				install -Dm755 "${file}" "${appdir}/${file}"
			else
				install -Dm644 "${file}" "${appdir}/${file}"
			fi
		done < <(find . -type f -print0)
	)

	# Install provided binary wrapper to /usr/bin
	install -Dm755 "${srcdir}/usr/bin/${_pkgname}" -t "${pkgdir}/usr/bin/"

	# Install desktop entry
	install -Dm644 "${srcdir}/usr/share/applications/${_pkgname}.desktop" \
		-t "${pkgdir}/usr/share/applications/"

	# Install application icons
	for i in 16 24 32 48 64 128 256; do
		install -Dm644 "${srcdir}/usr/share/icons/hicolor/${i}x${i}/apps/${_pkgname}.png" \
			-t "${pkgdir}/usr/share/icons/hicolor/${i}x${i}/apps"
	done

	# Install documentations
	install -Dm644 "${srcdir}/usr/share/doc/${_pkgname}/ChangeLog" \
		-t "${pkgdir}/usr/share/doc/${_pkgname}/"
	install -Dm644 "${srcdir}/usr/share/doc/${_pkgname}/NEWS" \
		-t "${pkgdir}/usr/share/doc/${_pkgname}/"

	# Install licenses
	install -Dm644 "${srcdir}/usr/share/licenses/${_pkgname}/3DPARTYLICENSE" \
		-t "${pkgdir}/usr/share/licenses/${_pkgname}/"
	install -Dm644 "${srcdir}/usr/share/licenses/${_pkgname}/LICENSE" \
		-t "${pkgdir}/usr/share/licenses/${_pkgname}/"
}
