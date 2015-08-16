# Contributor: Kevin Mihelich <kevin@archlinuxarm.org>
# Contributor: Coolgeek
# Maintainer: N30N <archlinux@alunamation.com>

pkgname="marvell-ipp"
pkgdesc="Marvell IPP library"
pkgver=0.2.1
pkgrel=2
arch=("armv7h")
url="http://www.marvell.com/"
license=("custom:MARVELL Software")
source=("http://sources.openbricks.org/devel/marvell-ipp_0.2.1-0ubuntu1~ppa10.tar.gz")
md5sums=("87fb47c4ce1236eba5730e2c9b7500b0")
depends=("marvell-libvmeta")

MAKEFLAGS=-j1

build() {
	cd "${srcdir}/marvell-ipp/"
	sed -r \
		-e "s|^(PATH_GNU_BIN=).+|\1/usr/bin|" \
		-e "s|^(TOOLCHAIN_PREFIX=).+|\1${CHOST}|" \
		-e "s|^(CFLAGS=).*|\1 -mcpu=cortex-a9|" \
		-e "s|^(AR=).+|\1/usr/bin/ar|" \
		-i example/Rules.make
	cd example/misc/build/wmmx2_linux/
	make -f  makefile_miscGen
}

package() {
	cd "${srcdir}/marvell-ipp/"

	install -Dm644 bin/License.txt \
		"$pkgdir/usr/share/licenses/$pkgname/LICENSE"
	install -Dm644 etc/99-uio.rules \
		"${pkgdir}/usr/lib/udev/rules.d/99-uio.rules"
	install -d "${pkgdir}/usr/lib" "${pkgdir}/usr/include/marvell-ipp"
	install -m644 include/*.h "${pkgdir}/usr/include/marvell-ipp"
	find bin lib -not -type d \( -name "*.so" -o -name "*.a" \) \
		-not -path "*libcodecvmetadec.a" \
		-not -path "*libvmetahal.a" \
		-not -path "*libvmetahal.so" \
		-not -path "*libcodecvmetadec.so" \
		-not -path "bin/libmiscgen.so" \
		-exec install -m644 "{}"  "${pkgdir}/usr/lib/" \;

	install -d "${pkgdir}/usr/include/OpenMAX/IL/"
	install -m644 openmax/include/* "${pkgdir}/usr/include/OpenMAX/IL/"
	install -m644 openmax/*.a "${pkgdir}/usr/lib/"
}
