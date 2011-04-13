# Maintainer: Tobias Powalowski <tpowa@archlinux.org>
# Contributor: Judd Vinet <jvinet@zeroflux.org>
pkgname=mdadm
pkgver=3.2.1
pkgrel=2
pkgdesc="A tool for managing/monitoring Linux md device arrays, also known as Software RAID"
arch=(i686 x86_64)
license=('GPL')
url="http://www.cse.unsw.edu.au/~neilb/source/mdadm/"
groups=('base')
conflicts=('mkinitcpio<0.5.99')
depends=('glibc')
backup=('etc/mdadm.conf')
source=(ftp://ftp.kernel.org/pub/linux/utils/raid/mdadm/mdadm-$pkgver.tar.bz2
        mdadm 
        mdadm.conf 
        mdadm_install
        mdadm_hook
        segfault-3.2.1.patch)
install=mdadm.install
replaces=('raidtools')

build() {
  cd $srcdir/$pkgname-$pkgver
  patch -Np1 -i ../segfault-3.2.1.patch
  make CXFLAGS="$CFLAGS"
}

package() {
  cd $srcdir/$pkgname-$pkgver
  make INSTALL=/bin/install DESTDIR=$pkgdir install
  install -D -m644 ../mdadm.conf $pkgdir/etc/mdadm.conf
  install -D -m755 ../mdadm $pkgdir/etc/rc.d/mdadm
  install -D -m644 ../mdadm_install $pkgdir/lib/initcpio/install/mdadm
  install -D -m644 ../mdadm_hook $pkgdir/lib/initcpio/hooks/mdadm
  # symlink for backward compatibility
  ln -sf /lib/initcpio/hooks/mdadm  $pkgdir/lib/initcpio/hooks/raid
  # build static mdassemble for Arch's initramfs
  make MDASSEMBLE_AUTO=1 mdassemble
  install -D -m755 mdassemble $pkgdir/sbin/mdassemble
}
md5sums=('5af65c32193fe0aea1aac4c4d44ac383'
         '7bff0e506fb6017510c8ec4a01896952'
         '00cbed931db4f15b6ce49e3e7d433966'
         '865c3d39e5f5dae58388160b563981f1'
         '1a3eb63832cecd6550f5b0a21d58cfdb')
md5sums=('d1e2549202bd79d9e99f1498d1109530'
         '7bff0e506fb6017510c8ec4a01896952'
         '00cbed931db4f15b6ce49e3e7d433966'
         '865c3d39e5f5dae58388160b563981f1'
         '1a3eb63832cecd6550f5b0a21d58cfdb'
         '2fd25605bd1836a33c689ac442cb73ed')
