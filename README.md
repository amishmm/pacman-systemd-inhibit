# pacman-systemd-inhibit

Inhibit system shutdown, reboot etc. when pacman is upgrading the system.

If the system is accidentally shut down or rebooted while pacman is upgrading it, your system and/or packages may break. This can cause many issues, such as preventing your system from booting. These hooks and script inhibit an accidental system shutdown, reboot, etc., if pacman is upgrading your system.

It also checks battery capacity and prevents any system updates when the capacity is low. This check is not performed if the system is on AC supply.

## How To Install?

### AUR

Install `pacman-systemd-inhibit` from the AUR for an easy setup. See PKGBUILD [here](https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=pacman-systemd-inhibit).

### Manual Configuration

1. Create a symlink file, `/usr/bin/pminhibit-sleep`, symlinked to `/usr/bin/sleep`
2. Place hook files in `/usr/share/libalpm/hooks` directory.
3. Place script file in `/usr/share/libalpm/scripts` directory.
4. If required, configure battery capacity via `/etc/pacman.d/battery.conf`.

## How It Works?

1. When `pacman` begins the upgrade, it calls a PreTransaction hook `00-50-systemd-inhibit.hook`. This hook puts an inhibition lock for shutdown, restart etc. with timeout of 15 minutes. It also checks if battery capacity is sufficient.
2. When `pacman` finishes the upgrade, it calls a PostTransaction hook `zz-50-systemd-inhibit.hook`. This hook removes the inhibition lock.

Note: In case pacman takes more than 15 minutes to upgrade (which is highly unlikely), then the inhibition lock gets removed, but pacman continues to function normally.
