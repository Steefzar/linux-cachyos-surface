## About this fork: `linux-cachyos-surface`

This fork adds a **CachyOS kernel packaging** on top of upstream
linux-surface — see [`pkg/linux-cachyos-surface/PKGBUILD`](pkg/linux-cachyos-surface/PKGBUILD).
It builds whatever kernel version upstream linux-surface (below) currently
has official patches for, as a normal Arch/CachyOS `pacman` package, instead
of a distro kernel with the patches applied out-of-tree.

> **Want the newest CachyOS kernel instead, even before linux-surface
> officially supports it?** Use the sibling package:
> [`linux-cachyos-surface-latest`](https://github.com/Steefzar/linux-cachyos-surface-latest).

**What it's for:** running a CachyOS kernel (EEVDF/BORE scheduler, LTO, BBR3,
and CachyOS's other tuning) with Surface hardware support, kept up to date
automatically as part of your normal system upgrades — rather than manually
tracking/rebuilding a Surface kernel by hand.

**How to use it:**

*Precompiled packages (easiest — no local kernel build):* builds of this
package (and its sibling) are published as a signed pacman repository served
from GitHub Releases. One-time setup — first trust the signing key:

```sh
curl -s https://raw.githubusercontent.com/Steefzar/linux-cachyos-surface/master/pkg/keys/surface-cachyos.asc \
    | sudo pacman-key --add -
sudo pacman-key --finger F43A86B2BAA715242965C241222C2A1B58F14BA7
sudo pacman-key --lsign-key F43A86B2BAA715242965C241222C2A1B58F14BA7
```

Then add the repository to `/etc/pacman.conf`:

```ini
[surface-cachyos]
Server = https://github.com/Steefzar/surface-kernel-autoupdate/releases/download/repo
```

And install:

```sh
sudo pacman -Syu linux-cachyos-surface linux-cachyos-surface-headers
```

*Build it yourself, auto-updated:* use the installer at
[Steefzar/surface-kernel-autoupdate](https://github.com/Steefzar/surface-kernel-autoupdate),
which sets up a local pacman repo and auto-builds/updates this package (and
its sibling [`linux-cachyos-surface-latest`](https://github.com/Steefzar/linux-cachyos-surface-latest),
which tracks the newest CachyOS release instead of waiting for upstream
patches) as part of your `yay` runs.

*Fully manual:* `makepkg` inside `pkg/linux-cachyos-surface/` like any PKGBUILD.

This package builds on two upstreams: the [CachyOS
kernel](https://github.com/CachyOS/linux) provides the base (its patches ship
pre-applied in the source tarball), and linux-surface (below) provides the
Surface hardware patches applied on top.

### Credits

- [linux-surface/linux-surface](https://github.com/linux-surface/linux-surface)
  (GPL-2.0) — all the Surface hardware-enablement patches this kernel is
  built from originate there; this repo is a fork of theirs and tracks
  their patches directly.
- [CachyOS/linux](https://github.com/CachyOS/linux) and
  [CachyOS/linux-cachyos](https://github.com/CachyOS/linux-cachyos) — the
  CachyOS kernel base (EEVDF/BORE scheduler, BBR3, LTO and other CachyOS
  tuning) this build starts from, and the packaging conventions this
  PKGBUILD is adapted from.

This packaging was set up, is maintained, and is periodically re-synced with
Claude Code (Anthropic) assistance, at the direction of and reviewed by the
repository owner. Commits made with Claude's help are marked
`Co-Authored-By: Claude <noreply@anthropic.com>`.

---

## About upstream: linux-surface

Everything outside `pkg/` in this repository is the work of
[linux-surface/linux-surface](https://github.com/linux-surface/linux-surface),
the project that maintains the kernel patches making Surface hardware work on
Linux (the Surface Aggregator Module, keyboard/touchpad, touch/pen input,
cameras, and more) — this repo is a fork of theirs with packaging added on
top.

- **Is your device supported?** See their
  [supported devices & feature matrix](https://github.com/linux-surface/linux-surface/wiki/Supported-Devices-and-Features#feature-matrix).
- **Hardware tips** (disk encryption, hibernation, TLP caveats, extra tweaks
  in `contrib/`): the [linux-surface wiki](https://github.com/linux-surface/linux-surface/wiki).
- **Support:** problems with *this package or its builds* — open an issue
  here. Hardware and patch questions — upstream's
  [Matrix space](https://matrix.to/#/#linux-surface:matrix.org); please don't
  report issues from this unofficial build there unless you can reproduce
  them on an official linux-surface kernel.

## License

This repository contains patches, which are either derivative work targeting a specific already licensed source, i.e. parts of the Linux kernel, or introduce new parts to the Linux kernel.
These patches fall thus, if not explicitly stated otherwise, under the license of the source they are targeting, or if they introduce new code, the license they explicitly specify inside of the patch.
Please refer to the specific patch and source in question for further information.
License texts can be obtained at https://github.com/torvalds/linux/tree/master/LICENSES.
