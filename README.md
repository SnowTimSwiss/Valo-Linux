# Valo Linux

> Debian done right: clean GNOME defaults, laptop-first, built entirely in the open.

Valo is a Debian Testing-based Linux distribution focused on great
out-of-the-box defaults, good laptop behavior, and solid hardware support. No
bloat, no forced package managers, no private build server.

The name is still work in progress.

## Why Valo?

Most Debian-based distributions either feel dated or come with too many
opinions baked in. Valo aims to be the middle ground:

- **Clean GNOME**: sensible defaults, not a Windows clone.
- **Laptop-first**: battery and firmware decisions should work well by default.
- **Fully open build**: every ISO is built by GitHub Actions.
- **Debian-based**: Valo stays close to Debian Testing instead of replacing it.

## How Releases Work

Valo is built entirely with GitHub Actions. No private servers, no black boxes.

1. A new tag such as `v0.1` is pushed to this repository.
2. GitHub Actions builds the ISO using `live-build` on a fresh Ubuntu runner.
3. The finished ISO is uploaded as a GitHub Release asset.
4. Installed systems receive Debian package updates through normal APT.

You can inspect the ISO build pipeline in
[`.github/workflows/build.yml`](.github/workflows/build.yml).

## v0.1 Scope

Valo `v0.1` is the first installable baseline:

- Debian Testing base.
- GNOME desktop through Debian packages.
- Live ISO built with Debian live-build.
- Debian Installer available from the boot menu.
- GNOME Software with Debian package update support.
- Initial Valo integration packages included locally in the ISO:
  - `valo-keyring`
  - `valo-release`
  - `valo-defaults`

It intentionally does not include custom themes, desktop tweaks, or hooks yet.

## Update Model

Valo should stay close to Debian Testing, but Valo-specific defaults still need
a safe path from one installed release to the next. The plan is to keep a very
small Valo APT repository in this same GitHub repository and publish it through
GitHub Pages.

That APT repository is only for Valo-owned packages:

- `valo-keyring`: the public signing key used by the Valo APT repository.
- `valo-release`: Valo release metadata and the Valo APT source definition.
- `valo-defaults`: Valo defaults, configuration snippets, and migration logic.

With that in place, a change from `v0.1` to `v0.2` can be shipped as a normal
package update. Users keep using the familiar Debian workflow:

```sh
sudo apt update
sudo apt upgrade
```

Valo should not mirror Debian packages or maintain a large package archive. The
Valo repository exists only to deliver Valo's own small integration packages.
For `v0.1`, this repository is enabled in testing mode through GitHub Pages at
`https://snowtimswiss.github.io/Valo-Linux`.

More detail is documented in [docs/update-model.md](docs/update-model.md).

## Repository Layout

```text
config/                  live-build configuration
packages/                Valo-owned Debian package sources
.github/workflows/       ISO and repository automation
docs/                    project design notes
```

The current `config/` tree contains the minimal package lists needed for the
`v0.1` live desktop and installer. Themes, hooks, and larger customizations will
be added in separate changes.

## Architecture Support

| Architecture | Status |
|---|---|
| x86_64 | Supported |
| ARM64 | Planned |

ARM64 support should be possible without major changes to the build pipeline,
because `live-build` and GitHub Actions can support ARM64 builds.

## Contributing

Contributions are welcome. Please read [`CONTRIBUTING.md`](CONTRIBUTING.md)
before opening a pull request.

## License

GPL-3.0. See [`LICENSE`](LICENSE) for details.
