# Maintainer: Ashley Whetter <(firstname) @ awhetter.co.uk>
# Co-Maintainer: NicoHood <archlinux {cat} nicohood {dog} de>
# PGP ID: 97312D5EB9D7AE7D0BD4307351DAE9B7C1AE9161
# Contributor: Eothred <yngve.levinsen@gmail.com>

pkgname=spotify
pkgver=1.1.46.916.g416cacf1
epoch=1
_snapid=pOBIoZ2LrCB3rDohMxoYGnbN14EHOgD7
_revision=43
pkgrel=2
pkgdesc='A proprietary music streaming service'
arch=('x86_64')
license=('custom')
url='https://www.spotify.com'
depends=('alsa-lib>=1.0.14' 'gtk2' 'libsystemd' 'libxss' 'desktop-file-utils' 'openssl' 'nss' 'at-spi2-atk')
depends_x86_64=('libcurl-gnutls')
makedepends=('squashfs-tools')
optdepends=('ffmpeg-compat-57: Adds support for playback of local files'
            'zenity: Adds support for importing local files'
            'libnotify: Desktop notifications')
options=('!strip')
source=('spotify.protocol'
        'LICENSE')
# Get latest version info: curl -H 'Snap-Device-Series: 16' http://api.snapcraft.io/v2/snaps/info/spotify
source_x86_64=("https://api.snapcraft.io/api/v1/snaps/download/${_snapid}_${_revision}.snap")
sha512sums=('999abe46766a4101e27477f5c9f69394a4bb5c097e2e048ec2c6cb93dfa1743eb436bde3768af6ba1b90eaac78ea8589d82e621f9cbe7d9ab3f41acee6e8ca20'
            '2e16f7c7b09e9ecefaa11ab38eb7a792c62ae6f33d95ab1ff46d68995316324d8c5287b0d9ce142d1cf15158e61f594e930260abb8155467af8bc25779960615')
sha512sums_x86_64=('5b3d5d1f52a554c8e775b8aed16ef84e96bf3b61a2b53266e10d3c47e341899310af13cc8513b04424fc14532e36543a6fae677f80a036e3f51c75166d8d53d1')

prepare() {
    cd "${srcdir}"
    unsquashfs ${_snapid}_${_revision}.snap
}

package() {
    cd "${srcdir}"

    mkdir -p "${pkgdir}/usr/share/spotify"
    cp -a squashfs-root/usr/share/spotify "${pkgdir}/usr/share/"

    install -Dm644 "${pkgdir}"/usr/share/spotify/spotify.desktop "${pkgdir}"/usr/share/applications/spotify.desktop
    sed -i 's/\/usr\/share\/spotify\/icons\/spotify-linux-128.png/spotify/' "${pkgdir}"/usr/share/applications/spotify.desktop

    for size in 22 24 32 48 64 128 256 512; do
        install -Dm644 "${pkgdir}/usr/share/spotify/icons/spotify-linux-$size.png" \
            "${pkgdir}/usr/share/icons/hicolor/${size}x${size}/apps/spotify.png"
    done

    # Move spotify binary to its proper location
    mkdir -p "${pkgdir}"/opt/spotify
    mv "${pkgdir}/usr/share/spotify" "${pkgdir}/opt/"

    # Symlink spotify binary which is located in /opt
    mkdir -p "${pkgdir}/usr/bin"
    ln -sf /opt/spotify/spotify "${pkgdir}/usr/bin/spotify"

    # Copy protocol file for KDE
    install -Dm644 "${srcdir}/spotify.protocol" "${pkgdir}/usr/share/kservices5/spotify.protocol"

    # Install license
    # https://www.spotify.com/legal/end-user-agreement
    install -Dm 644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

    # Fix permissions
    chmod -R go-w "${pkgdir}"
}
