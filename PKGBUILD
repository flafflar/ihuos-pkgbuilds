# Maintainer: Achilleas Michailidis <achmichail@gmail.com>
# Maintainer: Sébastien Luttringer
# Contributor: Tom Gundersen <teg@jklm.no>

pkgname=filesystem
pkgver=2023.01.31
pkgrel=1
pkgdesc='Base IHU OS files'
arch=('x86_64')
license=('GPL')
url='https://cs.ihu.gr'
depends=('iana-etc')
backup=('etc/crypttab' 'etc/fstab' 'etc/group' 'etc/gshadow' 'etc/host.conf'
        'etc/hosts' 'etc/issue' 'etc/ld.so.conf' 'etc/nsswitch.conf'
        'etc/passwd' 'etc/profile' 'etc/resolv.conf' 'etc/securetty'
        'etc/shadow' 'etc/shells' 'etc/subuid' 'etc/subgid')
source=('crypttab' 'env-generator' 'fstab' 'group' 'gshadow' 'host.conf' 'hosts'
        'issue' 'ld.so.conf' 'locale.sh' 'nsswitch.conf' 'os-release' 'profile'
        'passwd' 'resolv.conf' 'securetty' 'shadow' 'shells' 'sysctl' 'sysusers'
        'tmpfiles' 'subuid' 'subgid' 'ihuos-logo.svg' 'ihuos-logo.png'
        'ihuos-logo-text.svg' 'ihuos-logo-text-dark.svg')
sha256sums=('e03bede3d258d680548696623d5979c6edf03272e801a813c81ba5a5c64f4f82'
            'ed0cb4f1db4021f8c3b5ce78fdf91d2c0624708f58f36c9cf867f4d93c3bc6da'
            'e54626e74ed8fee4173b62a545ab1c3a3a069e4217a0ee8fc398d9933e9c1696'
            '244f0718ee2a9d6862ae59d6c18c1dd1568651eada91a704574fa527fbac2b3a'
            '90d879374f77bac47f132164c1e7fc4892e994ff1d1ac376efa0c1c26ea37273'
            '4d7b647169063dfedbff5e1e22cee77bd1a4183dbcfd5e802e68939da4bbf733'
            'd9cd8a77d9e0aa5e90d7f4ed74c8745c17b525e720e28e4c44364150003c35f9'
            'dab26b0a097da836113a46a02cc7665b483e1909f51f9ee1c2431fda165ec85f'
            'dad04a370e488aa85fb0a813a5c83cf6fd981ce01883fc59685447b092de84b5'
            '8ca2d8eef6fb5143c9ef7e9174ccfef59ac7ad2deee243574cd10c763156cc10'
            'c8ee7a9faf798caab178ec51afae4146f1efd8a716b7acedf28345b6c75f9697'
            'd841c6c706d4cae0ee99492224a1f50f7ece742bcfa7e556403aab0183d2cb31'
            '5e4088ad8d0853d390fa303f6be8c3f69c33834200cba9e90f7849f1993ca8d0'
            '5e06477834f51abf42ea4e8dc199632afc6afbfd8c44354685a271e9a48d2c0a'
            '5557d8e601b17a80d1ea7de78a9869be69637cb6a02fbfe334e22fdf64e61d4c'
            'd88be2b45b43605ff31dd83d6a138069b6c2e92bc8989b7b9ab9eba8da5f8c7b'
            '6e13705ac4d6f69cdba118c6b70c722346fd3c45224133e6bbfe28aca719563c'
            '0d9ad0a063e74e6b88db52b2f0a93e7b8f6faf1395981edebac493a13d0b59b3'
            '89e43a0b7028f52d5c8e7fb961d962c4b4f4e9595880a6157274ddb2c7c0b6b4'
            '30b97e8f5965744138f7a394e04454db6d509fb89e0a9b615bcd9037df3d6f2a'
            '5d8e61479f0093852365090e84d8d95b1e7fccfab068274ee25863bde6ff3e07'
            'e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855'
            'e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855'
            '1153edbe32cab9202573ce8fc1fd693f7c3fa44722ed95d4e3bf0a4a2cff00e1'
            '2fe622d1a65ad3de7229b5d6c8b055a15bfc8ca42c7b3ad1b020012900eb35fd'
            '81f491ea094c5b3a84a65561354443a52b90f9ad364be59a2d013f586e0f42ae'
            '81f491ea094c5b3a84a65561354443a52b90f9ad364be59a2d013f586e0f42ae')

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
  for f in fstab group host.conf hosts issue ld.so.conf nsswitch.conf \
  passwd resolv.conf securetty shells profile subuid subgid; do
    install -m644 "$srcdir"/$f etc/
    install -m644 "$srcdir"/$f usr/share/factory/etc/
  done
  ln -s ../proc/self/mounts etc/mtab
  for f in gshadow shadow crypttab; do
    install -m600 "$srcdir"/$f etc/
    install -m600 "$srcdir"/$f usr/share/factory/etc/
  done
  touch etc/arch-release
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
  for d in bin include lib share/{misc,pixmaps} src; do
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
  install -D -m644 "$srcdir"/sysctl usr/lib/sysctl.d/10-arch.conf

  # setup systemd-sysusers
  install -D -m644 "$srcdir"/sysusers usr/lib/sysusers.d/arch.conf

  # setup systemd-tmpfiles
  install -D -m644 "$srcdir"/tmpfiles usr/lib/tmpfiles.d/arch.conf

  # setup systemd.environment-generator
  install -D -m755 "$srcdir"/env-generator usr/lib/systemd/system-environment-generators/10-arch

  # add logo
  install -D -m644 "$srcdir"/ihuos-logo{.png,.svg,-text.svg,-text-dark.svg} usr/share/pixmaps
}

# vim:set ts=2 sw=2 et:
