# Maintainer: Tobias Powalowski <tpowa@archlinux.org>
# Contributor: Judd Vinet <jvinet@zeroflux.org>
pkgname=mdadm
pkgver=3.2.6
pkgrel=2
pkgdesc="A tool for managing/monitoring Linux md device arrays, also known as Software RAID"
arch=(i686 x86_64)
license=('GPL')
url="http://neil.brown.name/blog/mdadm"
groups=('base')
conflicts=('mkinitcpio<0.7')
depends=('glibc')
backup=('etc/mdadm.conf')
source=(ftp://ftp.kernel.org/pub/linux/utils/raid/mdadm/mdadm-$pkgver.tar.bz2
        mdadm 
        mdadm.conf 
        mdadm_install
        mdadm_hook
        mdadm_udev_install
        mdadm.service
        disable-werror.patch
        mdadm-fix-udev-rules.patch)
replaces=('raidtools')

build() {
  cd $srcdir/$pkgname-$pkgver
  patch -Np0 -i ../disable-werror.patch
  patch -p1 -i ../mdadm-fix-udev-rules.patch
  make CXFLAGS="$CFLAGS"
  # build static mdassemble for Arch's initramfs
  make MDASSEMBLE_AUTO=1 mdassemble
  
}

package() {
  cd $srcdir/$pkgname-$pkgver
  make INSTALL=/usr/bin/install DESTDIR=$pkgdir install
  install -D -m755 mdassemble $pkgdir/sbin/mdassemble
  install -D -m644 ../mdadm.conf $pkgdir/etc/mdadm.conf
  install -D -m755 ../mdadm $pkgdir/etc/rc.d/mdadm
  install -D -m644 ../mdadm_install $pkgdir/usr/lib/initcpio/install/mdadm
  install -D -m644 ../mdadm_hook $pkgdir/usr/lib/initcpio/hooks/mdadm
  install -D -m644 ../mdadm_udev_install $pkgdir/usr/lib/initcpio/install/mdadm_udev
  # systemd service file
  install -D -m644 $srcdir/mdadm.service $pkgdir/usr/lib/systemd/system/mdadm.service
}
md5sums=('3e255dc71e5144bbcb872788ca647267'
         '8333d405f550317c2bacd5510bf1cb60'
         '00cbed931db4f15b6ce49e3e7d433966'
         '815245a3af16a73ec1c5e5989fb892e9'
         'fbb5542d9bdf87441a11dd7e7a0a17f8'
         '0e35422d0cc007c3654a5e2591a9f9b5'
         'aafb5f9ac8437a284cbf948b9b13b179'
         '4ad87b74a4bc9a34621280abe0e0c3e4'
         '6c2961a0685dc0feb9bb397c39d8f351')
