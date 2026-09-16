# Maintainer: Nojjia
pkgname=pwvucontrol
pkgver=0.5.3
pkgrel=1
pkgdesc="Pipewire volume control applet"
arch=(x86_64)
url="https://github.com/saivert/pwvucontrol"
license=('GPL-3.0-only')
depends=(
	dconf
	glibc
	glib2
	graphene
	gtk4
	hicolor-icon-theme
	libadwaita
	libgcc
	libpipewire
	libwireplumber
)
makedepends=(
	clang
	meson
	rust
	blueprint-compiler
)
checkdepends=(appstream-glib)
optdepends=(
	'easyeffect: Audio Effects for Pipewire applications'
)
options=()
source=("https://github.com/saivert/$pkgname/archive/refs/tags/$pkgver.tar.gz")
sha256sums=('5ac6db39d8bb347c3447592fbc923465c9e4b001da8e330861a215c964c74e27')

build() {
	arch-meson --reconfigure "$pkgname-$pkgver" build
	meson compile -C build
}

check() {
	meson test -C build --print-errorlogs
}

package() {
	meson install -C build --no-rebuild --destdir "$pkgdir"
}
