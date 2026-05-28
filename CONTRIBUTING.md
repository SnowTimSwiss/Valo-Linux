# Contributing to Valo Linux

Thanks for helping shape Valo Linux.

Valo is a Debian 13 (Trixie)-based live system built entirely with Debian
live-build and GitHub Actions. The repository should stay small,
auditable, and reproducible.

## Project Scope

At this stage, contributions should keep the build minimal:

- Use Debian live-build conventions under `config/`.
- Prefer Debian packages and Debian defaults over custom infrastructure.
- Keep changes reviewable and documented.
- Do not add themes, hooks, or large desktop customizations unless the issue or
  pull request explicitly targets that work.

## Repository Layout

The `config/` directory follows the live-build configuration layout described
by the Debian Live Manual:

- `config/package-lists/` for future package list files.
- `config/includes.chroot/` for files copied into the live system filesystem.
- `config/includes.binary/` for files copied onto the ISO filesystem.
- `config/preseed/` for future debconf preseed files.
- `config/hooks/` for future staged build hooks.

Empty directories contain `.gitkeep` files so the intended structure is visible
before Valo has custom build content.

The `packages/` directory is reserved for small Valo-owned Debian packages:

- `valo-keyring` for the Valo APT repository signing key.
- `valo-release` for Valo release metadata and APT source configuration.
- `valo-defaults` for Valo defaults and migrations.

These packages are the supported path for Valo-specific changes that existing
installed systems should receive after installation.

## Local Builds

On a Debian or Debian-like build host with `live-build` installed:

```sh
sudo lb config \
  --mode debian \
  --distribution trixie \
  --architectures amd64 \
  --binary-images iso-hybrid \
  --debian-installer none \
  --archive-areas "main contrib non-free-firmware"

sudo lb build
```

Generated build outputs are ignored by Git.

The `v0.1` ISO is expected to be installable through Debian Installer and to
provide a mostly stock GNOME desktop.

## Releases

Releases are created from Git tags matching `v*`, for example:

```sh
git tag v1.0
git push origin v1.0
```

GitHub Actions builds the ISO on a fresh Ubuntu runner and uploads the ISO plus
its SHA256 checksum to the matching GitHub Release.

## Update Model

Valo uses Debian 13 (Trixie) for the operating system package base. Debian packages
should come from Debian whenever possible.

For Valo-specific configuration changes, Valo will use a tiny signed APT
repository published from this GitHub repository, likely through GitHub Pages.
That repository must only contain Valo-owned integration packages, not a mirror
or fork of Debian.

Installed systems should receive both Debian updates and Valo integration
updates using standard commands such as:

```sh
sudo apt update
sudo apt upgrade
```

Valo versioned releases, such as `v1.0` and `v1.1`, are ISO snapshots for new
installations and recovery media. Existing users move forward through Debian's
package updates plus the small Valo package channel.

If Valo needs to change installed configuration after installation, prefer one
of these approaches:

- Upstream the change to Debian or use an existing Debian package.
- Ship the change through `valo-defaults` with a careful package migration.
- Document a manual migration in the release notes if automation is risky.
- Ship the change only in new ISO images if it is not required for existing
  systems.

See [docs/update-model.md](docs/update-model.md) for the intended repository
design.

## Pull Requests

Before opening a pull request:

- Keep the change focused.
- Explain why the change belongs in Valo rather than upstream Debian.
- Mention whether the change affects ISO builds, installed systems, or both.
- Include build logs or workflow links when changing live-build behavior.

## References

- Debian Live Manual: https://live-team.pages.debian.net/live-manual/html/live-manual.en.html
- live-build `lb config` manual page: https://manpages.debian.org/unstable/live-build/lb_config.1.en.html
