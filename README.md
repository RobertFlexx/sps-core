# SPS core package collection

This repository is the official base recipe tree for **SPS Linux**
(informally Splux): bootstrap toolchain, libraries, utilities, boot and
device infrastructure, networking, and SPS itself. It contains recipes and
small supporting files, not source archives or binary packages.

SPS the package system is separate from the distro. These recipes are ordinary
SPS recipes, so a Linux From Scratch tree (or any other SPS root) can consume
them. The `linux-desktop` and `linux-custom` kernels, and anything `setup`
does with hostname, fstab, and init, are SPS Linux defaults. An LFS build
using SPS does not have to follow that installer.

Package categories are organizational. A package's `name` record is its
identity; the category is not part of the name. Core uses these categories as
packages are added:

```text
base/  boot/  devel/  fs/  kernel/  lang/  libs/  meta/  net/
```

Category directories are created only when they contain a package.

## Configuration

The normal official configuration is:

```text
git core https://github.com/RobertFlexx/sps-core.git 100
```

`src update` clones it to `/usr/src/sps/core` by default and builds the local
package index. The checkout remains an ordinary Git repository.

```sh
src update
src status
src search bash
src show bash
```

An administrator can put overrides in a higher-priority directory repository:

```text
dir local /usr/local/src/sps-local 200
git core https://github.com/RobertFlexx/sps-core.git 100
```

`src which PACKAGE` shows the selected definition and alternatives.

## Building a package

Enter the package directory and run `mkpkg`:

```sh
cd /usr/src/sps/core/libs/zlib
mkpkg
```

Inspect the archive, install it into a disposable SPS root, run `pkcheck` and
`pkstat`, perform an appropriate smoke test, then remove it with `pkdel`.
The exact recipe format and verification procedure are documented in the SPS
package maintainer guide.

## Contributions

Base changes must keep `main` indexable and buildable. Use canonical stable
upstream archives, exact SHA-256 hashes, reviewed dependencies, and short
recipes using the upstream build system. Do not commit downloads, package
archives, or build logs. A package is ready for `main` only after its source,
build, staged files, install, checks, smoke test, and removal have been
reviewed. Work that has not passed those checks belongs on a testing branch.

Update one package in place and commit it with a package-specific message, for
example `zlib: update to 1.3.2`.
