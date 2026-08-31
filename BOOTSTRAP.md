# SPS bootstrap notes

The core tree is not yet bootstrap-ready. No end-to-end build from a clean host
to a self-hosting, bootable SPS system has passed. This file separates the
current bootstrap contract from work still requiring validation.

## Seed environment

The first pass necessarily starts from host tools. The seed environment must
provide a Linux build host with a C and C++ compiler, assembler and linker,
POSIX shell and AWK, make, tar, patch, basic text/file utilities, a SHA-256
implementation, and the decompressors required by selected sources. Exact
host versions and commands will be recorded with the first successful
bootstrap.

The seed toolchain is an explicit bootstrap input, not an undeclared package
dependency. Ordinary packages must otherwise build in an SPS root containing
only their selected runtime/build closure. Once native toolchain packages are
installed, later passes must use those SPS-owned tools.

## Planned passes

1. Install the `filesystem` package into an empty root.
2. Install Linux API headers and build a temporary binutils/GCC toolchain.
3. Build and install glibc against those headers.
4. Rebuild binutils and GCC as native SPS packages.
5. Build the shell, AWK, make, core text utilities, archives, and compression
   stack.
6. Build TLS, download, and Git support, then install SPS from its official
   release package.
7. Run `src update` inside the target and rebuild the remaining core closure
   through `sget` in dependency order.
8. Add device management, filesystems, networking, service scripts, the
   kernel, and bootloader packages.
9. Boot the result, verify network and repository updates, rebuild a package,
   and test a complete SPS self-upgrade.

Pass-specific bootstrap recipes or commands must remain visible. The normal
package recipes must not be distorted into one universal bootstrap mode.

## System direction

The intended base is GNU libc with eudev for device management and elogind for
session interfaces. systemd is not a base dependency. Service installation and
service enablement are separate operations; packages may provide service files
but do not enable them automatically.

Kernel configuration is a visible distribution baseline beside the kernel
recipe, not hidden build logic. Installing GRUB files must never run
`grub-install` or alter disks or EFI variables. Those are explicit
administrator actions.

Packages requiring system accounts remain on a testing branch until their
account requirement and an idempotent, visible creation policy are reviewed.
Recipes must not choose arbitrary numeric UIDs.

## Proof still required

Before removing the warning at the top of this file, retain external logs for
both toolchain passes and every core package, verify no undeclared host tools or
libraries enter the build, test upgrades of critical packages, boot the root,
and complete the self-hosting sequence above. Package count alone is not
bootstrap evidence.
