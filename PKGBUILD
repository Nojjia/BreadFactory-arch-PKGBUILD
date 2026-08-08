# Maintainer: Nojjia

pkgname=zen-browser-bin
pkgver=1.21.10b
pkgrel=1
pkgdesc='Performance oriented Firefox-based web browser'
arch=('x86_64' 'aarch64')
url='https://github.com/zen-browser/desktop'
license=(MPL-2.0)
depends=(
	alsa-lib
	at-spi2-core
	cairo
	dbus
	fontconfig
	freetype2
	gdk-pixbuf2
	glib2
	glibc
	gtk3
	hicolor-icon-theme
	libgcc
	libstdc++
	libx11
	libxcb
	libxcomposite
	libxcursor
	libxdamage
	libxext
	libxfixes
	libxi
	libxrandr
	libxrender
	mime-types
	nspr
	nss
	pango
	sh
	ttf-font
)
optdepends=(
	'libnotify: Notification integration'
	'networkmanager: Location detection via available WiFi networks'
	'onnxruntime: Local machine learning features such as smart tab groups'
	'pipewire-pulse: Audio support through PipeWire'
	'speech-dispatcher: Text-to-Speech'
	'xdg-desktop-portal: Screensharing with Wayland'
)
conflicts=('zen-browser' 'zen-browser-git')
options=(!strip !debug)
source_x86_64=("https://github.com/zen-browser/desktop/releases/download/${pkgver}/zen.linux-${CARCH}.tar.xz")
source_aarch64=("https://github.com/zen-browser/desktop/releases/download/${pkgver}/zen.linux-${CARCH}.tar.xz")
source=('zen-browser.desktop')
sha256sums=('8bc43491b64acf38565b4d7adc5314757d26166decde0d5943cfc2b5e48bc0d6')
sha256sums_x86_64=('6a134befd30e9618f4d7e50dddb3ae721de117bab728a97c75f9e5c2123a1d0f')
sha256sums_aarch64=('6a134befd30e9618f4d7e50dddb3ae721de117bab728a97c75f9e5c2123a1d0f')

package() {
	local _pkgname='zen-browser'
	local appdir="${pkgdir}/usr/lib/${_pkgname}"

	# Install zen while respecting file modes
	(
		cd "${srcdir}/zen" || exit
		while IFS= read -r -d '' file; do
			if [[ -x "${file}" ]]; then
				install -Dm755 "${file}" "${appdir}/${file}"
			else
				install -Dm644 "${file}" "${appdir}/${file}"
			fi
		done < <(find . -type f -print0)
	)

	# Install a binary wrapper because firefox uses one
	install -Dvm755 /dev/stdin "${pkgdir}/usr/bin/${_pkgname}" <<END
#!/bin/sh
exec /usr/lib/${_pkgname}/zen "\$@"
END

	# Install desktop entry
	install -Dvm644 "${srcdir}/zen-browser.desktop" -t "${pkgdir}/usr/share/applications/"

	# Install application icons
	for i in 16 32 48 64 128; do
		install -Dvm644 "${appdir}/browser/chrome/icons/default/default${i}.png" "${pkgdir}/usr/share/icons/hicolor/${i}x${i}/apps/${_pkgname}.png"
	done
	
	# Use system certificates and Symlink libonnxruntime
	ln -sfv "/usr/lib/libonnxruntime.so" -t "${appdir}"
	ln -sfv "/usr/lib/libnssckbi.so" -t "${appdir}"

	# Install policies.json to disable auto-update
	install -Dvm644 /dev/stdin "${appdir}/distribution/policies.json" <<END
{
	"policies": {
		"DisableAppUpdate": true,
		"AppAutoUpdate": false,
		"ManualAppUpdateOnly": true,
		"DefaultSerialGuardSetting": 3
	}
}
END
}
