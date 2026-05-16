# Flatpak Cheatsheet

Flatpak is a universal package management system for Linux that provides sandboxed application deployment.

**Table of Contents**

1. [Basic Commands](#1-basic-commands)
1. [Searching and Installing Applications](#2-searching-and-installing-applications)
1. [Listing and Information](#3-listing-and-information)
1. [Running and Managing Applications](#4-running-and-managing-applications)
1. [Updating and Maintenance](#5-updating-and-maintenance)
1. [Remote Management](#6-remote-management)
1. [Advanced / Troubleshooting](#7-advanced--troubleshooting)
1. [Useful Tips](#useful-tips)

## 1. Basic Commands

| Command                  | Description                                      | Example                                      |
|--------------------------|--------------------------------------------------|----------------------------------------------|
| `flatpak --version`      | Display the installed Flatpak version            | `flatpak --version`                          |
| `flatpak help`           | Show general help or help for a specific command | `flatpak help install`                       |

## 2. Searching and Installing Applications

| Command                              | Description                                      | Example                                              |
|--------------------------------------|--------------------------------------------------|------------------------------------------------------|
| `flatpak search <query>`             | Search for applications in configured remotes    | `flatpak search gimp`                                |
| `flatpak install <remote> <app>`     | Install an application (or runtime)              | `flatpak install flathub org.gimp.GIMP`              |
| `flatpak install <bundle-file>`      | Install from a local `.flatpak` bundle           | `flatpak install ./app.flatpak`                      |
| `flatpak install --user <remote> <app>` | Install for the current user only             | `flatpak install --user flathub com.example.App`     |

**Common remotes**: `flathub` (primary), `flathub-beta`.

## 3. Listing and Information

| Command                          | Description                                      | Example                                              |
|----------------------------------|--------------------------------------------------|------------------------------------------------------|
| `flatpak list`                   | List all installed applications and runtimes     | `flatpak list`                                       |
| `flatpak list --app`             | List only installed applications                 | `flatpak list --app`                                 |
| `flatpak list --runtime`         | List only installed runtimes                     | `flatpak list --runtime`                             |
| `flatpak info <app>`             | Show detailed information about an installed app | `flatpak info org.gimp.GIMP`                         |
| `flatpak remotes`                | List configured remotes                          | `flatpak remotes`                                    |
| `flatpak remote-info <remote> <app>` | Show information about an app in a remote     | `flatpak remote-info flathub org.gimp.GIMP`          |

## 4. Running and Managing Applications

| Command                          | Description                                      | Example                                              |
|----------------------------------|--------------------------------------------------|------------------------------------------------------|
| `flatpak run <app-id>`           | Launch an installed application                  | `flatpak run org.gimp.GIMP`                          |
| `flatpak override <app-id>`      | Modify sandbox permissions                       | `flatpak override --filesystem=~/Documents org.gimp.GIMP` |
| `flatpak override --reset <app-id>` | Reset all overrides for an app                | `flatpak override --reset org.gimp.GIMP`             |
| `flatpak permissions`            | List current permission overrides                | `flatpak permissions`                                |

## 5. Updating and Maintenance

| Command                        | Description                                      | Example                                              |
|--------------------------------|--------------------------------------------------|------------------------------------------------------|
| `flatpak update`               | Update all installed applications and runtimes   | `flatpak update`                                     |
| `flatpak update <app-id>`      | Update a specific application                    | `flatpak update org.gimp.GIMP`                       |
| `flatpak uninstall <app-id>`   | Remove an application                            | `flatpak uninstall org.gimp.GIMP`                    |
| `flatpak uninstall --unused`   | Remove unused runtimes                           | `flatpak uninstall --unused`                         |
| `flatpak repair`               | Repair the Flatpak installation                  | `flatpak repair`                                     |

## 6. Remote Management

| Command                            | Description                                      | Example                                              |
|------------------------------------|--------------------------------------------------|------------------------------------------------------|
| `flatpak remote-add <name> <url>`  | Add a new remote repository                      | `flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo` |
| `flatpak remote-modify <name>`     | Modify a remote (e.g., change URL)               | `flatpak remote-modify flathub --url=...`            |
| `flatpak remote-delete <name>`     | Remove a remote                                  | `flatpak remote-delete flathub`                      |

## 7. Advanced / Troubleshooting

| Command                    | Description                                      | Example                                              |
|----------------------------|--------------------------------------------------|------------------------------------------------------|
| `flatpak --system`         | Operate on the system installation               | `flatpak --system list`                              |
| `flatpak --user`           | Operate on the user installation                 | `flatpak --user list`                                |
| `flatpak mask <app-id>`    | Prevent an app from being updated                | `flatpak mask org.gimp.GIMP`                         |
| `flatpak unmask <app-id>`  | Remove update mask                               | `flatpak unmask org.gimp.GIMP`                       |
| `flatpak history`          | Show installation/update history                 | `flatpak history`                                    |

## 8. Useful Tips

- Use `--user` for personal installations that do not require administrator privileges.
- Most commands support the `--verbose` (`-v`) or `--quiet` (`-q`) flags.
- Application IDs follow the reverse-domain format (e.g., `org.mozilla.firefox`).
- To enable Flathub (recommended first step on many distributions):

  ```bash
  flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
  ```

## 9. Recommended Flatpaks

| Flatpak | Description | Verified |
|---|---|---|
| [Bolt Launcher](https://flathub.org/en/apps/com.adamcake.Bolt) | Third-party Jagex launcher | ⚠️ |
| [Brave](https://flathub.org/en/apps/com.brave.Browser) | Fast Internet, AI, Adblock | ✅ |
| [Discord](https://flathub.org/en/apps/com.discordapp.Discord) | Talk, play, hang out | ✅ |
| [Firefox](https://flathub.org/en/apps/org.mozilla.firefox) | Fast, Private & Safe Web Browser | ✅ |
| [GIMP](https://flathub.org/en/apps/org.gimp.GIMP) | High-end image creation and manipulation | ✅ |
| [JDownloader](https://flathub.org/en/apps/org.jdownloader.JDownloader) | Download management tool | ⚠️ |
| [Kate](https://flathub.org/en/apps/org.kde.kate) | Advanced text editor | ✅ |
| [MediaInfo](https://flathub.org/en/apps/net.mediaarea.MediaInfo) | Convenient unified display of the most relevant technical and tag data for video and audio files | ⚠️ |
| [Monero](https://flathub.org/en/apps/org.getmonero.Monero) | Monero: the secure, private, untraceable cryptocurrency | ⚠️ |
| [Mullvad Browser](https://flathub.org/en/apps/net.mullvad.MullvadBrowser) | Free the internet from mass surveillance | ⚠️ |
| [OpenShot](https://flathub.org/en/apps/org.openshot.OpenShot) | An easy to use, quick to learn, and surprisingly powerful video editor | ✅ |
| [Plex](https://flathub.org/en/apps/tv.plex.PlexDesktop) | Plex client for desktop computers | ✅ |
| [Podman Desktop](https://flathub.org/en/apps/io.podman_desktop.PodmanDesktop) | Open source graphical tool for containers and Kubernetes | ✅ |
| [PolyMC](https://flathub.org/en/apps/org.polymc.PolyMC) | Custom launcher for Minecraft | ⚠️ |
| [qBittorrent](https://flathub.org/en/apps/org.qbittorrent.qBittorrent) | Open-source Bittorrent client | ✅ |
| [Spotify](https://flathub.org/en/apps/com.spotify.Client) | Online music streaming service | ⚠️ |
| [Stremio](https://flathub.org/en/apps/com.stremio.Stremio) | Secure, modern and seamless entertainment experience | ✅ |
| [Telegram](https://flathub.org/en/apps/org.telegram.desktop) | Pure instant messaging — simple, fast, secure | ✅ |
| [Thunderbird](https://flathub.org/en/apps/org.mozilla.Thunderbird) | Email, newsfeed, chat, and calendaring client | ✅ |
| [TradingView](https://flathub.org/en/apps/com.tradingview.tradingview) | Social network, analysis platform and charting tool for traders | ⚠️ |
| [Waterfox](https://flathub.org/en/apps/net.waterfox.waterfox) | Privacy-focused web browser | ✅ |
| [Wireshark](https://flathub.org/en/apps/org.wireshark.Wireshark) | Network protocol analyzer | ✅ |