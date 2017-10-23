Anki isn't available in Debian 9.0 "stretch" (the release frozen in 2017)
because of the Qt 5 transition.  See [Debian bug #784612
  ](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=784612).
Upstream versions compatible with stretch begin with 2.1.0, which is
[in alpha](https://anki.tenderapp.com/discussions/beta-testing/208-anki-210-alpha-8)
as of [2017-02-24
  ](https://anki.tenderapp.com/discussions/beta-testing/342-anki-210-alpha-11).

Fortunately, `schroot` provides a simple and lightweight way to create
a Debian 8 "jessie" environment on a "stretch" system and run a stable
Anki from there.

## Usage

After the setup below, this command runs Anki:

```schroot -c anki -- anki```

The resulting Anki has full access to my homedir (notably `~/Anki`),
my network connection, and my desktop environment, including the input
method (`fcitx`) I use for typing Japanese.

## Setup

Install `schroot`, which will facilitate entering the chroot, and
`debootstrap`, which will populate it with "jessie" files:
```
sudo apt install schroot debootstrap
```

Create and populate the chroot's file tree:
```
sudo mkdir -p /srv/anki
sudo mkdir /srv/anki/chroot
sudo debootstrap jessie /srv/anki/chroot http://deb.debian.org/debian/
```

and tell `schroot` about it:
```
sudo tee /etc/schroot/chroot.d/anki.conf >/dev/null <<EOF
[anki]
description=Anki on jessie
type=directory
directory=/srv/anki/chroot
profile=desktop
preserve-environment=true
users=$USER
EOF
```

Finally, customize the chroot with your locale and time zone,
and install Anki and allied packages:
```
sudo cp {,/srv/anki/chroot}/etc/locale.gen
sudo cp -P {,/srv/anki/chroot}/etc/localtime
sudo schroot -c anki -- apt install -y anki locales mplayer fonts-vlgothic fonts-takao
```

Now `schroot -c anki -- anki` should work!

## Variations

The `fonts-vlgothic` package included above is one I use for Japanese.
You may want other fonts packages for other languages.  (E.g., for
Chinese, perhaps `fonts-arphic-uming fonts-wqy-zenhei`.)  Or copy in
fonts from the host; for example:
```
sudo rsync -av {,/srv/anki/chroot}/usr/share/fonts/
sudo schroot -c anki -- fc-cache -v
```

I later made a second chroot for Anki development.  For any
additional chroot, vary the `/srv/anki/$foo` path and the
`schroot` config's filename (`/etc/schroot/chroot.d/$foo.conf`),
header (`[$foo]`), and `description`.
