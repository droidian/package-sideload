package-sideload
================

This package contains services to allow installing sideloaded packages
(from e.g. Android recovery) at runtime.

systemd's [system-update.target](https://www.freedesktop.org/software/systemd/man/systemd.offline-updates.html) is
leveraged.

Prerequisites for droppers
--------------------------

First of all, the packages get dropped in `/var/cache/package-sideload/bundles/<feature-name>/archives/`,
where `<feature-name>` is the name of the feature set to be installed (for example, `developer-tools`).

The dropper script has also the responsibility to to symlink `/system-update` to
`/var/cache/package-sideload/` on the rootfs, thus allowing sideloading to happen
at the next reboot.

The dropper script should also create a file called `/var/cache/package-sideload/bundles/<feature-name>/packages`,
containing the list of the packages to install (separated by newlines).

Runtime boot flow
-----------------

This package ships a drop-in for PackageKit's Offline updater to prevent it from running
if `/system-update` does not point to `/var/cache/package-sideload`, thus allowing
both systems to coexist.

The boot flow is the following:

1. [`systemd-system-update-generator`](https://www.freedesktop.org/software/systemd/man/systemd-system-update-generator.html#) checks
   for the existence of `/system-update`, and redirects the default target to `system-update.target` as per spec.
2. `package-sideload.service` gets started by systemd:
   1. Packagesets to install are enumerated



apt-get -d -o "dir::cache=/tmp/test" -o "Debug::NoLocking=1" install hello
