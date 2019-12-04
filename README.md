# pacman-systemd-inhibit
Inhibit system shutdown, reboot etc. when pacman is upgrading the system.

Accidental shutdown or reboot of the system when pacman is upgrading the system, can lead to unbootable / broken system or broken packages.

These hooks and script, inhibit accidental system shutdown, reboot etc. when pacman is upgrading the system.

## How to install?
1. Create a symlink file, `/usr/bin/pacman-systemd-inhibit-sleep`, symlinked to `/usr/bin/sleep`
1. Place hook files in `/usr/share/libalpm/hooks` directory.
1. Place script file in `/usr/share/libalpm/scripts` directory.

## How it works?
1. When `pacman` begins the upgrade, it calls a PreTransaction hook `00-50-systemd-inhibit.hook`. This hook puts an inhibition lock for shutdown, restart etc. with timeout of 15 minutes.
1. When `pacman` finishes the upgrade, it calls a PostTransaction hook `zz-50-systemd-inhibit.hook`. This hook removes the inhibition lock.

Note: In case pacman takes more than 15 minutes to upgrade (which is highly unlikely), then the inhibition lock gets removed, but pacman continues to function normally.
