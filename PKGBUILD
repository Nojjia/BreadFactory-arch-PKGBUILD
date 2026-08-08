# Maintainer: Nojjia
pkgname=arduino-ide-v2-bin
pkgver=2.3.10
pkgrel=1
pkgdesc='Arduino prototyping platform IDE, rewrite based on the Theia IDE framework.'
arch=(x86_64)
url="https://github.com/arduino/arduino-ide"
license=(AGPL-3.0-only)
depends=(
	alsa-lib
	at-spi2-core
	bash
	cairo
	dbus
	expat
	glib2
	glibc
	gtk3
	hicolor-icon-theme
	libcups
	libdrm
	libgcc
	libsecret
	libstdc++
	libx11
	libxcb
	libxcomposite
	libxdamage
	libxext
	libxfixes
	libxkbcommon
	libxkbfile
	libxrandr
	mesa
	nspr
	nss
	pango
	python
)
optdepends=(
	'libusb: Needed for some libraries or boards'
	'usbutils: Needed for stm32 boards using st-link'
	'libusb-compat: Needed for the `micronucleus` cli utility'
	'python-pyserial: Needed for esptool'
)
conflicts=(arduino-ide-v2 arduino-ide-v2-git arduino-ide arduino-ide-git)
options=(!debug !strip)
source=("${url}/releases/download/${pkgver}/arduino-ide_${pkgver}_Linux_64bit.zip")
sha256sums=('cc8a0b01e763d4646b670ce70c1bc8c389a0fa14ab556dcc0749c03f475a7975')

prepare() {
	rm -rf "$srcdir/arduino-ide_${pkgver}_Linux_64bit/resources/app/plugins/cortex-debug/extension/binary_modules/v12.14.1/win32"
	rm -rf "$srcdir/arduino-ide_${pkgver}_Linux_64bit/resources/app/plugins/cortex-debug/extension/binary_modules/v12.18.3"
	rm -rf "$srcdir/arduino-ide_${pkgver}_Linux_64bit/resources/app/plugins/cortex-debug/extension/binary_modules/v12.14.1/darwin"

	rm -rf "$srcdir/arduino-ide_${pkgver}_Linux_64bit/resources/app/plugins/cortex-debug/extension/binary_modules/v12.14.1/linux/arm"
	rm -rf "$srcdir/arduino-ide_${pkgver}_Linux_64bit/resources/app/plugins/cortex-debug/extension/binary_modules/v12.14.1/linux/arm64"
}

package() {
	local _pkgname='arduino-ide-v2'
	local appdir="${pkgdir}/usr/lib/${_pkgname}"

	# Install arduino-ide-v2
	(
		cd "${srcdir}/arduino-ide_${pkgver}_Linux_64bit" || exit
		while IFS= read -r -d '' file; do
			if [[ -x "${file}" ]]; then
				install -Dm755 "${file}" "${appdir}/${file}"
			else
				install -Dm644 "${file}" "${appdir}/${file}"
			fi
		done < <(find . -type f -print0)
	)

	# Install launcher
	install -Dm755 /dev/stdin "${pkgdir}/usr/bin/arduino-ide" <<EOF
#!/bin/sh
exec /usr/lib/arduino-ide-v2/arduino-ide "$@"
EOF

	# Install desktop entry
	install -Dm644 /dev/stdin "${pkgdir}/usr/share/applications/${_pkgname}.desktop" <<EOF
[Desktop Entry]
Name=Arduino IDE 2.x
Comment=Opensource electronics prototyping platform
Exec=/usr/lib/arduino-ide-v2/arduino-ide %u
Icon=arduino-ide-v2
Type=Application
StartupWMClass=Arduino IDE
Categories=Development;IDE;Electronics;
StartupNotify=true
Terminal=false
GenericName=Arduino IDE 2.x
Keywords=embedded electronics;avr;microcontroller;
MimeType=text/x-arduino;
EOF

	# Install application icons
	install -Dm644 "${appdir}/resources/app/resources/icons/512x512.png" "${pkgdir}/usr/share/icons/hicolor/512x512/apps/${_pkgname}.png"
}
