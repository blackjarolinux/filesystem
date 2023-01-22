# Maintainer: Yisus7u7 <dev.yisus@hotmail.com>
pkgname=filesystem
pkgver=2023.1
pkgrel=1
pkgdesc='Base Blackjaro Linux files'
arch=('x86_64')
license=('GPL')
url='https://blackjarolinux.github.io/'
depends=('iana-etc')
backup=('etc/crypttab' 'etc/fstab' 'etc/group' 'etc/gshadow' 'etc/host.conf'
        'etc/hosts' 'etc/issue' 'etc/ld.so.conf' 'etc/nsswitch.conf'
        'etc/passwd' 'etc/profile' 'etc/resolv.conf' 'etc/securetty'
        'etc/shadow' 'etc/shells' 'etc/subuid' 'etc/subgid'
        'etc/hostname' 'etc/modules-load.d/modules.conf' 'etc/vconsole.conf')
install="$pkgname.install"
source=('crypttab' 'env-generator' 'fstab' 'group' 'gshadow' 'host.conf' 'hosts'
        'issue' 'ld.so.conf' 'locale.sh' 'nsswitch.conf' 'os-release' 'profile'
        'passwd' 'resolv.conf' 'securetty' 'shadow' 'shells' 'sysctl' 'sysusers'
        'tmpfiles' 'subuid' 'subgid' 'vconsole.conf'
        'home-local-bin.sh' 'hostname' 'modules.conf')
sha256sums=('e03bede3d258d680548696623d5979c6edf03272e801a813c81ba5a5c64f4f82'
            'ed0cb4f1db4021f8c3b5ce78fdf91d2c0624708f58f36c9cf867f4d93c3bc6da'
            'e54626e74ed8fee4173b62a545ab1c3a3a069e4217a0ee8fc398d9933e9c1696'
            '244f0718ee2a9d6862ae59d6c18c1dd1568651eada91a704574fa527fbac2b3a'
            '90d879374f77bac47f132164c1e7fc4892e994ff1d1ac376efa0c1c26ea37273'
            '4d7b647169063dfedbff5e1e22cee77bd1a4183dbcfd5e802e68939da4bbf733'
            'd9cd8a77d9e0aa5e90d7f4ed74c8745c17b525e720e28e4c44364150003c35f9'
            'aa59e888f2f4b6f565ae7f4057b987bfde07890a2ccde438abee2b93a93d96c0'
            'dad04a370e488aa85fb0a813a5c83cf6fd981ce01883fc59685447b092de84b5'
            '153d848ed51f2774e5a1578ea08e0c8586ecc63f7562697e035b84247edb2f82'
            '8f7623fbf8217d452306e618fe10155cf8a0b7234cdb882fe1a527c1838cf260'
            'f46f4689ee9177f07d98811d8d2e5e43fb890bba7064216332815b7a3d6414c5'
            '5e4088ad8d0853d390fa303f6be8c3f69c33834200cba9e90f7849f1993ca8d0'
            '5e06477834f51abf42ea4e8dc199632afc6afbfd8c44354685a271e9a48d2c0a'
            '5557d8e601b17a80d1ea7de78a9869be69637cb6a02fbfe334e22fdf64e61d4c'
            'd88be2b45b43605ff31dd83d6a138069b6c2e92bc8989b7b9ab9eba8da5f8c7b'
            '6e13705ac4d6f69cdba118c6b70c722346fd3c45224133e6bbfe28aca719563c'
            'c390b31fffc4a2b5d78ae8c89f5317aadef1f71baac09cfb467b675db1406d61'
            '89e43a0b7028f52d5c8e7fb961d962c4b4f4e9595880a6157274ddb2c7c0b6b4'
            'b5b28f395583d141d88c0b955cd05124f9b8cdf003feab01e55885b8e8c1303e'
            '5d8e61479f0093852365090e84d8d95b1e7fccfab068274ee25863bde6ff3e07'
            'e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855'
            'e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855'
            'cd4a55177020a436254bb4baf84e068b98b3b0f6644173a7c853d58d236e00f1'
            'b3a0fe98c0859192fb0fe03a9033ca5c5c8b48e9d23466a355b7c95bd5601051'
            '295a881e674d14ee0c0e77f25236a8fd99a9453e5c33f635f55d2e8c6959c7ea'
            'a8a1cd5c81b11498d43ba0e0b5de53de6f154a395d54171f44d2874b4f659053')

package() {
  cd "$pkgdir"

  # setup root filesystem
  for d in boot dev etc home mnt usr var opt srv/http run; do
    install -d -m755 $d
  done
  install -d -m555 proc
  install -d -m555 sys
  install -d -m0750 root
  install -d -m1777 tmp
  # vsftpd won't run with write perms on /srv/ftp
  # ftp (uid 14/gid 11)
  install -d -m555 -g 11 srv/ftp

  # setup /etc and /usr/share/factory/etc
  install -d etc/{ld.so.conf.d,skel,profile.d} usr/share/factory/etc
  for f in fstab group hostname host.conf hosts issue ld.so.conf nsswitch.conf \
  passwd resolv.conf securetty shells profile subuid subgid vconsole.conf; do
    install -m644 "$srcdir"/$f etc/
    install -m644 "$srcdir"/$f usr/share/factory/etc/
  done
  ln -s ../proc/self/mounts etc/mtab
  for f in gshadow shadow crypttab; do
    install -m600 "$srcdir"/$f etc/
    install -m600 "$srcdir"/$f usr/share/factory/etc/
  done

  # add modules-load.d/modules
  install -D -m644 $srcdir/modules.conf etc/modules-load.d/modules.conf

  # Create the manjaro-release file
  echo "Blackjaro Linux" > $pkgdir/etc/blackjaro-release
	ln -s blackjaro-release $pkgdir/etc/arch-release
  install -m644 "$srcdir"/home-local-bin.sh etc/profile.d/home-local-bin.sh
  install -m644 "$srcdir"/locale.sh etc/profile.d/locale.sh
  install -Dm644 "$srcdir"/os-release usr/lib/os-release

  # setup /var
  for d in cache local opt log/old lib/misc empty; do
    install -d -m755 var/$d
  done
  install -d -m1777 var/{tmp,spool/mail}

  # allow setgid games (gid 50) to write scores
  install -d -m775 -g 50 var/games
  ln -s spool/mail var/mail
  ln -s ../run var/run
  ln -s ../run/lock var/lock

  # setup /usr hierarchy
  for d in bin include lib share/misc src; do
    install -d -m755 usr/$d
  done
  for d in {1..8}; do
    install -d -m755 usr/share/man/man$d
  done

  # add lib symlinks
  ln -s usr/lib lib
  [[ $CARCH = 'x86_64' ]] && {
    ln -s usr/lib lib64
    ln -s lib usr/lib64
  }

  # add bin symlinks
  ln -s usr/bin bin
  ln -s usr/bin sbin
  ln -s bin usr/sbin

  # setup /usr/local hierarchy
  for d in bin etc games include lib man sbin share src; do
    install -d -m755 usr/local/$d
  done
  ln -s ../man usr/local/share/man

  # setup systemd-sysctl
  install -D -m644 "$srcdir"/sysctl usr/lib/sysctl.d/10-manjaro.conf

  # setup systemd-sysusers
  install -D -m644 "$srcdir"/sysusers usr/lib/sysusers.d/manjaro.conf

  # setup systemd-tmpfiles
  install -D -m644 "$srcdir"/tmpfiles usr/lib/tmpfiles.d/manjaro.conf

  # setup systemd.environment-generator
  install -D -m755 "$srcdir"/env-generator usr/lib/systemd/system-environment-generators/10-manjaro
}

# vim:set ts=2 sw=2 et:
