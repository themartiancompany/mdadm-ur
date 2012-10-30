# Maintainer: Tobias Powalowski <tpowa@archlinux.org>
# Contributor: Judd Vinet <jvinet@zeroflux.org>
pkgname=mdadm
pkgver=3.2.6
pkgrel=1
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
        disable-werror.patch)
replaces=('raidtools')

build() {
  cd $srcdir/$pkgname-$pkgver
  patch -Np0 -i ../disable-werror.patch
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
  # symlink for backward compatibility
  ln -sf /usr/lib/initcpio/hooks/mdadm  $pkgdir/usr/lib/initcpio/hooks/raid
  # move /lib/* to /usr/lib/
  mv $pkgdir/lib/udev $pkgdir/usr/lib/
  rm -rf $pkgdir/lib
  # systemd service file
  install -D -m644 $srcdir/mdadm.service $pkgdir/usr/lib/systemd/system/mdadm.service
}
md5sums=('3e255dc71e5144bbcb872788ca647267'
         '8333d405f550317c2bacd5510bf1cb60'
         '00cbed931db4f15b6ce49e3e7d433966'
         '609d10888727710cb20db7ac3e096116'
         'fbb5542d9bdf87441a11dd7e7a0a17f8'
         'd1d8e9eb81ce9347de74f3c84a9db09e'
         'aafb5f9ac8437a284cbf948b9b13b179'
         '4ad87b74a4bc9a34621280abe0e0c3e4')
